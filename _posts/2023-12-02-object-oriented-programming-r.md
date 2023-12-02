---
layout: post
title: Object Oriented Programming in R
date: 2023-12-02
categories: 
tags: 
image: /assets/article_images/2023-12-02-object-oriented-programming-r/RiFg2mpUTX4esQMNQ67ia5.jpg
image2: /assets/article_images/2023-12-02-object-oriented-programming-r/RiFg2mpUTX4esQMNQ67ia5.jpg
---

In a recent project, I wanted to implement my own object class to simplify my code and make it more flexible. That's when I realized that [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) in R can be [unnecessarily complex](https://adv-r.hadley.nz/oo.html). Therefore, in this blog post, I want to make it easy for you to get started. If you're not familiar with object-oriented programming, try the examples and play around with the objects created to get the gist. If you are already familiar with object-oriented programming in other languages but not in R, then the ideas here may help you overcome some of the same issues I encountered.

One caveat is that the examples in this post may not follow the best practices of R. However, it was helpful for me to write the objects in this way so as to [draw a parallel](https://docs.python.org/3/tutorial/classes.html#class-objects). to other programming languages.


## Why Object-Oriented Programming?

At its core, [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) is a programming paradigm in which objects consist of data (attributes) and functions (methods) that specifically manipulate those data. This helps to organize data and code together that can be helpful in maintaining internal consistency. For example, suppose you want to implement a simple `Table` class with three attributes: column names, row names, and a data matrix, and you want to implement a `transpose` method that specifically operates on the `Table` class by swapping the row and column names and transforming the matrix. That code might look something like this:

```R
Table <- R6::R6Class(
    classname="Table",
    public = list(
        rownames = NULL,
        colnames = NULL,
        matrix = NULL,
        transpose = function() {
            rownames <- self$rownames
            self$rownames <- self$colnames
            self$colnames <- rownames
            self$matrix <- transpose_matrix(self$matrix)  # forgive the pseudo-code
        }
    )
)
```
Syntactically, this `transpose` method lives very close to the data it manipulates, making things simple and organized. Now suppose you also want create a custom `dataframe` class implemented as an ordered named list, also with an associated `transpose` method. Suppose later down the line, you find that you need to write a `transpose` function for images too. Without object-oriented programming, using a purely [functional programming](https://adv-r.hadley.nz/fp.html) approach, you can easily end up with this kind monstrosity, also known as a [God function](https://en.wikipedia.org/wiki/God_object):

```R
transpose <- function(obj) {
	if ( class(obj) == 'Table' ) {
	    rownames <- obj$rownames
	    obj$rownames <- self$colnames
	    obj$colnames <- rownames
	    obj$matrix <- transpose_matrix(obj$matrix)
	} else if ( class(obj) == 'dataframe' )) ) {
	    # code that transposes dataframe
	} else if ( class(obj) == ... )) ) {
	    ...  # a method for each class
	}
	    stop("Input must be c(Table, dataframe, ...)")
	}
	return(obj)
}	
```

To prevent this kind antipattern from emerging, there are three main strategies:

1. Simply name separate functions for separate objects, such as `transpose_matrix` and `transpose_dataframe`. This is really just a workaround and not a real system, and it can quickly result in in verbose and redundant code.

2. In [function overloading](https://en.wikipedia.org/wiki/Function_overloading), a single function can behave in many different ways depending on the input. This is R's preferred solution and how S3 and S4 methods are implemented. In R, function overloading is accomplished via [method dispatch](https://adv-r.hadley.nz/s3.html#method-dispatch), where it checks which of the many versions of the same function to try. This can be helpful, but unfortunately, [R does not guarantee that the class of your object uniquely determines its dispatch](https://adv-r.hadley.nz/s3.html#s3-dispatch). If the class of the object is not found, R may instead dispatch a function of the same name that operates on the implicit class of the function, and you may end up getting the wrong output.

3. Of course, the most useful aproach is object-oriented programming, which has strict requirements for attributes and methods. However, because R is designed to be a [functional programming language](https://adv-r.hadley.nz/fp.html), over the years, people have hacked together many solutions to implement object-oriented programming, resulting in numerous systems. According to Hadley Wickham, the [most important systems in use today](https://adv-r.hadley.nz/oo.html) are **S3**, **R6,** and **S4**. Therefore, I set out to learn what each of these systems are and how to create the simplest object using them.


## S3

This is the simplest system, and honestly, it feels like just a hack. Essentially, you use a container class, such as a named `list` or `environment` and update the `class()` field to a specific name. Then, to create a class-specific method, you use a special syntax (function-dot-class) to register that method to be found in the lookup for `.S3methods()`. Here is the minimum reproducible example, based on this [example code](https://gist.github.com/jbryer/1647595) from Jason Bryer, with a few modifications.

```R
#' Constructor
Person <- function(name=NA_character_, age=NA_real_) {

    # attributes
    self = list(
        name = name,
        age = age,
        get = function(x) self[[x]],
        set = function(x, value) self[[x]] <<- value
    )
    
    # methods
    if (length(findClass('Person')) == 0) {
	    
	    update_name <<- function() UseMethod("update_name")
	    update_name.Person <<- function() UseMethod("update_name", "Person")
	    self$update_name <- function(value) {
	        self$name <- value
	        assign('name', self$name, envir=self)
	    }
	
	    increase_age <<- function() UseMethod("increase_age")
	    increase_age.Person <<- function() UseMethod("increase_age", "Person")
	    self$increase_age <- function() {
	        self$age <- self$age + 1
	        assign('age', self$age, envir=self)
	    }
    }

    self <- list2env(self)
    class(self) <- "Person"  # classname

    return(self)
}
```
To verify that this works, try the following:

```R
harrison <- Person(name="harrison", age=31)

# examine
str(harrison)
# Class 'Person' <environment: 0x140b1a4b8>

.S3methods(class="Person")
# [1] increase_age update_name 
# see '?methods' for accessing help and source code

.S4methods(class="Person")
# no methods found

harrison$name  # "harrison"
harrison$update_name("Harrison Wang")
harrison$name  # "Harrison Wang"
    
harrison$age  # 31
harrison$increase_age()
harrison$age  # 32
```
`UseMethod` is optionally used to [register the method](https://adv-r.hadley.nz/s3.html#s3-methods) as an S3 method in the global namespace via the deep-assignment `<<-` operator. To prevent the constructor from repeatedly running the method definitions, I wrapped that section in an if block to check that the `Person` class has not already been created.


## R6

This is the most advanced system, and yet it's also the easiest to use. Unlike S3 and S4, R6 is actually a full-fledged object-oriented programming system that provides all of the features you'd expect in an object-oriented system: public vs. private variables, public vs. private functions, chained assignment, mutable attributes, and mulitple inheritance. Here is the minimum reproducible example that instantiates the same `Person` object as above. This is based on the [example code](https://adv-r.hadley.nz/r6.html#r6-important-methods) from Hadley Wickham's Advanced R.

```R
library('R6')

#' Constructor
Person <- R6Class(
    classname="Person",
    private = NULL,
    public = list(

        # attributes
        name = NA_character_,
        age = NA_real_,

        # init
        initialize = function(name, age) {
            if (!missing(name)) self$name <- name
            if (!missing(age)) self$age <- age
        },

        # methods
        update_name = function(value) self$name <- value,
        increase_age = function() self$age <- self$age + 1
    )
)
```

To verify that this works, run the following code:

```R
harrison <- Person$new(name="harrison", age=31)

# examine
str(harrison)
# Classes 'Person', 'R6' <Person>#   Public:#     age: 31#     clone: function (deep = FALSE) #     increase_age: function () #     initialize: function (name, age) #     name: harrison#     update_name: function (value)  

harrison$name  # "harrison"
harrison$update_name("Harrison Wang")
harrison$name  # "Harrison Wang"

harrison$age  # 31
harrison$increase_age()
harrison$age  # 32
```

Under the hood, it works very similarly to S3 classes by using environments to manage attributes. I love the simplicity of the syntax, and there's no need to jerry-rig anything to make it work. If you're going to pick one to use, this is the one I recommend. The only reason you wouldn't us it is if you need your code is required to run without installing the R6 library.

## S4

This is the most complicated of the three systems, but it is used by a good chunk of Bioconductor, including [Seurat](https://github.com/mojaveazure/seurat-object/blob/master/R/seurat.R#L55-L72), so we're stuck with it. Syntactically, it is very similar to S3, but instead of using a `list` or `environment` directly, you use the more formal `setClass()` function to define the attributes, also known as "slots", which can be accessed using the at `@` operator. Once this definition is set, you can create new instances of the class using the `new()` function. However, like the S3 system, you still have to register the methods separately from the class definition. To understand why this is complicated, I'll provide the minimum reproducible example, then demonstrate why it doesn't work exactly the way we expect. This is based on the [example code](https://adv-r.hadley.nz/s4.html#s4-basics) from Hadley Wickham's Advanced R.

```R
#' Constructor
Person <- function(name=NA_character_, age = NA_real_) {

    # Class Definition
    if (length(findClass('Person')) == 0) {
        setClass(
            "Person",  # classname
            slots = c(
                name = "character",
                age = "numeric"
            ),
            prototype=list(
                name = NA_character_,
                age = NA_real_
            )
        )

        # methods
        setGeneric("update_name", function(obj, value) standardGeneric("update_name"))
        setMethod("update_name", "Person", function(obj, value) {
            obj@name <- value
            obj
        })

        setGeneric("increase_age", function(obj) standardGeneric("increase_age"))
        setMethod("increase_age", "Person", function(obj) {
            obj@age <- obj@age + 1
            obj
        })
    }
    return(new("Person", name=name, age = age))
}
```

To verify that this works, run the following code:

```R
harrison <- Person(name='harrison', age=31)

# examine
str(harrison)

.S3methods(class="Person")
# no methods found

.S4methods(class="Person")
# [1] increase_age update_name 
# see '?methods' for accessing help and source code
    
harrison@name  # harrison
harrison <- update_name(harrison, "Harrison Wang")
harrison@name  # "Harrison Wang"

harrison@age  # 31
harrison <- increase_age(harrison)
harrison@age  # 32
```

At this point, you might notice something different from before: this time, we're overwriting the entire object just to update each field. This is required, because [R functions are copy-on-modify](https://adv-r.hadley.nz/names-values.html#copy-on-modify), which means that within the `update_name` method, running `obj@name <- value` creates a copy of the entire object, which is then returned. Then in the global namespace, we copy this object again to overwrite the original `harrison` object. You can check this behavior by running the following:

```R
harrison@name  # "Harrison Wang"
update_name(harrison, "Harrison C. Wang")
harrison@name  # nothing changed
```
This is obviously problematic. If we have to copy the entire object just to update one field, this will result in our programs having high memory consumption and poor performance. So how can we get around this? After much trial-and-error, I found two solutions.

#### Environments

This is the same trick that the S3 and R6 systems use. Instead of using slots, which are only accessible from the global frame, we simply store all of our attributes in an environment stored in a single slot. This is based on the [example code](https://stackoverflow.com/questions/50475928/s4-class-update-slot-in-another-method-without-returning-the-object-r/50496401#50496401) provided in a StackOverflow post.

```R
#' Constructor
Person <- function(name=NA_character_, age = NA_real_) {

    # Class Definition
    if (length(findClass('Person')) == 0) {

        setClass(
            "Person",
            slots = c(attrs = "environment"),
            prototype=list(
                attrs = list2env(list(
                    name = NA_character_,
                    age = NA_real_
                ))
            )
        )

        # methods
        setGeneric("update_name", function(x, value) standardGeneric("update_name"))
        setMethod("update_name", "Person", function(x, value) x@attrs$name <- value) 

        setGeneric("increase_age", function(x) standardGeneric("increase_age"))
        setMethod("increase_age", "Person", function(x) x@attrs$age <- x@attrs$age + 1)
        
        # Get @attrs directly using $
        # See: https://github.com/mojaveazure/seurat-object/blob/master/R/seurat.R#L2900C1-L2902C2
        "$.Person" <<- function(x, i) x@attrs[[i]]  # getter        
        "$<-.Person" <<- function(x, i, ..., value) x@attrs[[i]] <- value  # setter
    }

    return(new("Person", attrs=list2env(list(name=name, age = age))))
}
```
To verify that this works, run the following code:

```R
harrison <- Person(name='harrison', age=31)

harrison$name  # harrison
update_name(harrison, "Harrison Wang")
harrison$name  # "Harrison Wang"

harrison$age  # 31
increase_age(harrison)
harrison$age  # 32
```
To grant direct access to fields in `@attrs` using dollar-sign `$` operator, I [borrowed this trick](https://github.com/mojaveazure/seurat-object/blob/master/R/seurat.R#L2900C1-L2902C2) from Seurat's [source code](https://github.com/mojaveazure/seurat-object/tree/master). However, this is somewhat unsatisfying. Because of the way S4 is designed, proper use of it means we should actually use the slots instead of this work-around. Also, if we're going to use this approach, we may as well use the S3 system to simplify things.

#### Metaprogramming

The key to understanding this approach is to realize that in the global environment, you can actually access and update the `harrison@name` field, like so:

```R
harrison@name <- "Harrison C. Wang"
```

Therefore, all we need to do is to be able to figure out how to run this command from within the function. Fortunately, we can do that with some ideas from [metaprogramming](https://adv-r.hadley.nz/metaprogramming.html) by using [substitution](https://adv-r.hadley.nz/quasiquotation.html#substitution) to capture and replace the original name of the object being passed into the method. This is based on the [example code](https://stackoverflow.com/questions/22448198/does-r-copy-unevaluated-slots-in-s4-classes-on-assignment/24578991#24578991) provided in a StackOverflow post.

```R
#' Constructor
Person <- function(name=NA_character_, age = NA_real_) {

    # Class Definition
    if (length(findClass('Person')) == 0) {
        setClass(
            "Person",  # classname
            slots = c(
                name = "character",
                age = "numeric"
            )
        )

        # methods
        setGeneric("update_name", function(obj, value) standardGeneric("update_name"))
        setMethod("update_name", "Person", function(obj, value) {
            eval(eval(substitute(expression(obj@name <<- value))))
        })

        setGeneric("increase_age", function(obj) standardGeneric("increase_age"))
        setMethod("increase_age", "Person", function(obj) {
            eval(eval(substitute(expression(obj@age <<- obj@age + 1))))
        })
    }
    return(new("Person", name=name, age = age))
}
```
To verify that this works, run the following code:

```R
harrison <- Person(name='harrison', age=31)

harrison@name  # harrison
update_name(harrison, "Harrison Wang")
harrison@name  # "Harrison Wang"

harrison@age  # 31
increase_age(harrison)
harrison@age  # 32
```
Note that the [StackOverflow post](https://stackoverflow.com/questions/22448198/does-r-copy-unevaluated-slots-in-s4-classes-on-assignment/) containing this solution is worth a read. The top solution recommends that despite being able to update the field using [quasiquotation](https://adv-r.hadley.nz/quasiquotation.html), you should just use a [reference class](http://adv-r.had.co.nz/R5.html) (ie. [R6](https://adv-r.hadley.nz/r6.html)) for better performance.


### Summary

If you are required to use the S4 system to build your project, you can use the two techniques in conjunction to improve the performance of your object. For example, you can use slots for immutable fields and `@attrs` for frequently mutated fields. However, if you really want a truly performant object that behaves nicely, I still recommend using R6, especially if you have the chance to build it from ground up.


## Outro

Despite my opinion on using R6 for its convenience, Hadley Wickham still [recommends using S3 or S4 over R6](https://adv-r.hadley.nz/oo-tradeoffs.html), because knowing S3 and S4 is foundational to understanding how many R packages work. Also, the future of R will be the new [S7](https://www.jumpingrivers.com/blog/r7-oop-object-oriented-programming-r/) system, found [here](https://github.com/RConsortium/S7), which is built on top of S3 and S4. Here is [a talk from Hadley Wickham](https://www.youtube.com/watch?v=P3FxCvSueag) discussing S7's new features.

This tutorial wasn't meant to cover everything. For more insight on object-oriented programming in R, I found [this video](https://www.youtube.com/watch?v=V5M90oonAAg) by Josiah Parry very helpful in understanding the design principle underlying S3 and why objects behave the way that they do.

Congrats on making it to the end! I hope you enjoyed, and if you run into any issues with these systems, just remember: where there is a will, there is a way.

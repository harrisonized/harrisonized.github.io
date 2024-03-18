---
layout: post
title: Managing Packages in R
date: 2024-03-16
categories: 
tags: 
image: /assets/article_images/2024-03-18-managing-packages-in-r/containership.jpg
image2: /assets/article_images/2024-03-18-managing-packages-in-r/containership.jpg

---

I recently wanted to test out the newly released [Seurat 5](https://satijalab.org/seurat/articles/announcements.html#changes-in-seurat-v5). However, once I was ready to switch back to Seurat v4, I ran into a great deal of difficulty trying to reinstall the required dependencies. If this has ever happened to you, or if you want to prevent such a situation from arising, this blog post is for you.

##  Introduction

According to [R's documentation](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Managing-libraries):

> R packages are installed into *libraries*, which are directories in the file system containing a subdirectory for each package installed there.

This is a little cryptic, so let's unpack what some terminology.

In most other languages, a **library** is the codebase containing functions and objects that you can import from said library. For example, [dplyr](https://dplyr.tidyverse.org/index.html) is a library. In python, the equivalent [pandas](https://pandas.pydata.org/) is the "Python Data Analysis Library" according to its webpage. A library is called that, because it allows you to import functions and objects, much like you can visit a library in real life to borrow books.

When you install a library, it comes in a **package**. This is because the source code has to be compiled (i.e. packaged) before it can be run. Sometimes, the package comes pre-compiled and the underlying source code is not available. Hence, a library and a package is not exctly the same thing, although many times, they can be referred to interchangeably.

When you run a script, you import functions from multiple libraries, such as 	`dplyr` and `Seurat`. A collection of libraries is stored in an **environment**, which allows you to keep isolated collections of libraries in different environments so as to prevent dependency conflicts. The software that enables you to manage these environments is called an **environment management tool**. A popular one is [Conda](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html), which is mainly used to manage [python environments](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-python.html). (Note: Conda can also be used to manage [R environments](https://docs.anaconda.com/free/working-with-conda/packages/using-r-language/), although this is typically not recommended.) Another popular one is Python's [virtual environment](https://docs.python.org/3/library/venv.html). R also has [renv](https://github.com/rstudio/renv) and [packrat](https://github.com/rstudio/packrat), but neither of them are commonly used, and as you will see below, using R's intrinsic package manager is much more straightforward.

Finally, environment management tools themselves can be managed using a **container**, of which the most well-known one is [Docker](https://www.docker.com/). This not only allows you to reproduce the environment, but it will also allow you to reproduce the operating system. This is standard in industry but beyond the scope of this guide.

Unfortunately, R does not adhere to the standard terminology. Instead, a **library** is called a *package*, an **environment** is called a *library*, and its **environment management tool** is instead called a *package management system*. Also, in R, an [environment](https://adv-r.hadley.nz/environments.html) means something entirely different: it is an internal data structure that behaves similar to a hashmap. This poor terminology makes it extremely difficult to search for this topic.

Hopefully, this guide will be enough to clear up the confusion and help you get started. For the purposes of this guide, we will be using R's terminology rather than the standard terminology, just to be consistent.

## Installing Packages

Suppose you want to install [dplyr](https://dplyr.tidyverse.org/index.html). You might enter the following command into RStudio:

```R
install.packages('dplyr')
```

You will then get feedback telling you where the package came from and where it was installed:

```R
Installing package into '/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library'(as ‘lib’ is unspecified)trying URL 'https://cran.rstudio.com/bin/macosx/big-sur-arm64/contrib/4.3/dplyr_1.1.4.tgz'Content type 'application/x-gzip' length 1595883 bytes (1.5 MB)==================================================downloaded 1.5 MBThe downloaded binary packages are in	/var/folders/vv/f4ftf3qs34z8f896ktyh7p_00000gq/T//RtmpwdMIaN/downloaded_packages
```

If you now go to the specified folder, you'll see a lot of the packages available, including dplyr. On a Macbook, if the path specified is outside your home directory, this is your System Library. This will typically be in `/Library/Frameworks/`. If you open the dplyr folder, here's what you'll see:

```
dplyr├─ data├─ DESCRIPTION├─ doc├─ help├─ html├─ INDEX├─ libs├─ LICENSE├─ Meta├─ NAMESPACE├─ NEWS.md└─ R
```

You'll recognize many of the components available from the [source code](https://github.com/tidyverse/dplyr), but when you open the R folder, rather than the source code itself, you'll see `dplyr`, `dplyr.rdb`, and `dplyr.rdx`. These are [compiled versions of the source code](https://reproducible-builds.org/news/2017/05/03/reproduciing-r-packages/), and it's where R gets the functions when you import it using `library('dplyr')`.

Note that in this example, I called `install.packages()` using the default parameters, and the installation was extremely fast. This is because R grabbed the latest version of the package, which comes pre-compiled from [CRAN](https://cran.r-project.org/). In fact, all the pre-compiled packages available are listed [here](https://cran.rstudio.com/bin/macosx/big-sur-arm64/contrib/4.3/). However, since I'm on R version 4.3.0, when I go to import dplyr using `library('dplyr')`, I now get this warning:

```R
Warning message:package ‘dplyr’ was built under R version 4.3.1
```

In [CRAN's repository](https://cran.rstudio.com/bin/macosx/big-sur-arm64/contrib/4.3/), whenever a new version gets released, the older version gets removed. You can install an older version by using `remotes::install_version()`:

```R
remotes::install_version("dplyr", version="1.0.10")
```

In this case, instead of pre-compiled code being available directly from CRAN, your machine will actually just build it using C, and you'll get a lot of output:

```R
* installing *source* package ‘dplyr’ ...** package ‘dplyr’ successfully unpacked and MD5 sums checked** using staged installation** libsusing C++ compiler: ‘Apple clang version 15.0.0 (clang-1500.3.9.4)’using SDK: ‘’

clang++ -arch arm64 -std=gnu++17 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG   -I/opt/R/arm64/include    -fPIC  -falign-functions=64 -Wall -g -O2  -c chop.cpp -o chop.o
...

ld: warning: -single_module is obsoleteld: warning: -multiply_defined is obsoleteinstalling to /Users/harison/Library/R/arm64/4.3/library/00LOCK-dplyr/00new/dplyr/libs** R** data*** moving datasets to lazyload DB** inst** byte-compile and prepare package for lazy loading** help*** installing help indices*** copying figures** building package indices** installing vignettes** testing if installed package can be loaded from temporary location** checking absolute paths in shared objects and dynamic libraries** testing if installed package can be loaded from final location** testing if installed package keeps a record of temporary installation path* DONE (dplyr)
```

If you install a package such as [dplyr](https://github.com/tidyverse/dplyr) from Github, you'll also have to compile it. Note that on a Macbook, in order for libraries to be compiled successfully, you [must](https://stackoverflow.com/questions/68647868/unable-to-install-packages-via-renvrestore-r-was-unable-to-find-one-or-mor) install [gfortran-6.1.pkg](https://cran.r-project.org/bin/macosx/tools/gfortran-6.1.pkg) from [here](https://cran.r-project.org/bin/macosx/tools/). It cannot be [homebrew](https://brew.sh/)'s version of [gfortran](https://formulae.brew.sh/formula/gcc), otherwise, you'll run into [this error](https://stackoverflow.com/questions/77836548/library-gfortran-not-found-when-installing-r-packages), for which I spent far too much time trying to figure out [how to fix this error](https://blog.cynkra.com/posts/2021-03-16-gfortran-macos/), which ultimately didn't end up working.


## Controlling Where Packages are Installed

The key to package management is that you can install packages into different locations. To get started with, try to install a package into a location that doesn't exist:

```R
> install.packages("dplyr", lib="")Warning in install.packages :  'lib = ""' is not writableWould you like to use a personal library instead? (yes/No/cancel)
```

If you select `yes` here, R will then automatically create the default user library path for you. On my machine, this is located at:

```
/Users/harrison/Library/R/arm64/4.3/library
```

If you were to create other library paths, this is where I would recommend placing them. (Of course, you can place your libraries wherever you want, but I like to keep things all together.) For example, to install the latest version of [Seurat](https://satijalab.org/seurat/articles/install_v5) (v5.0.2 at the time of this writing), I ran the following command:

```R
install.packages("Seurat", lib="~/Library/R/arm64/seurat-5.0/library")
```

I installed [Seurat v5.0.2](https://github.com/satijalab/seurat/releases/tag/v5.0.2) into `seurat-5.0` and [Seurat v4.3.0](https://github.com/satijalab/seurat/releases/tag/v4.3.0) into `seurat-4.3`, keeping each of these and their dependencies isolated. This is perfect, because [Seurat v4.3.0 requires Matrix v1.6-1](https://github.com/cole-trapnell-lab/monocle3/issues/690), Seurat v5.0.2 requires Matrix v1.6-5, and when you try to use Seurat v4.3.0 with Matrix v1.6-5, you'll get [this error](https://github.com/cole-trapnell-lab/monocle3/issues/690).

## Controlling How Packages are Imported

When you run the `.libPaths()` command, you get a list of libraries in the order they are searched. For example, when you call `library('dplyr')`, R will first search the first library, then search the second library if the first is not found, and so on until it finds a match or gives you an error. By default, R will find your User Library first, followed by your System Library.

```R
> .libPaths()
[1] "/Users/harrison/Library/R/arm64/4.3/library"                           [2] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
```

To change this ad hoc, you can pass a list directly into `.libPaths()`:

```R
.libPaths(
    c("/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library",
      "~/Library/R/arm64/4.3/library")
)

.libPaths()
# [1] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
# [2] "/Users/harrison/Library/R/arm64/4.3/library"
```
However, since this requires hardcoding paths and you have to remember doing this for every script, I would not recommend this approach.

A better, more reliable way to change the order is to set the `.Renviron` file, a plaintext file that goes in your home directory. In this file, you can specify two environmental variables: `R_LIBS` specifies your System Library, and `R_LIBS_USER` specifies your User Libraries. Here are the various ways to achieve the different orderings:

1. As mentioned above, by default, if you do not set anything, by default, R imports from your User Library first and the System Library. You can also have the variables set, but hash-commented:

	```
	# R_LIBS="/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
	# R_LIBS_USER="/Users/harrison/Library/R/arm64/4.3/library"
	```
	
	```R
	> .libPaths()
	[1] "/Users/harrison/Library/R/arm64/4.3/library"
	[2] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
	```
	
2. To keep the same order (User Library, then System Library) but add additional User Libraries, set that using the `R_LIBS_USER` variable, which takes a colon-separated list. For example:

	```
	# R_LIBS="/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
R_LIBS_USER="/Users/harrison/Library/R/arm64/seurat-5.0/library:/Users/harrison/Library/R/arm64/seurat-4.3/library:/Users/harrison/Library/R/arm64/4.3/library"
	```
	
	```R
	> .libPaths()	[1] "/Users/harrison/Library/R/arm64/seurat-5.0/library"                    	[2] "/Users/harrison/Library/R/arm64/seurat-4.3/library"                    	[3] "/Users/harrison/Library/R/arm64/4.3/library"
	[4] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library" 
	```
	
	The order of the colon-separated list dictates the order of the User Libraries.

3. To reverse the order and import the System Library before the User Library, set both `R_LIBS` and `R_LIBS_USER`.

	```
	R_LIBS="/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
R_LIBS_USER="/Users/harrison/Library/R/arm64/4.3/library"
	```
	
	```R
	> .libPaths()
	[1] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"	[2] "/Users/harrison/Library/R/arm64/4.3/library" 
	```

4. Finally, to prevent the User Libraries from being used, you must deliberately unset it. Otherwise, the default User Library (`~/Library/R/arm64/4.3/library`) will automatically be found. 

	```
	R_LIBS="/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library"
R_LIBS_USER=""
	```
	
	```R
	> .libPaths()	[1] "/Library/Frameworks/R.framework/Versions/4.3-arm64/Resources/library" 
	```
	
This order specified is preseved regardless of whether you use RStudio or an Rscript. Also, regardless of your settings, [you cannot unset the System Library](https://stackoverflow.com/questions/54946732/dont-use-r-system-library), as this is where all of the base packages live. You can, however, set it to a path that doesn't exist, and in that case, you will not even be able to run base R, so make sure you set this with care.

#  Outro

Hopefully, this guide will save you the headache of going through dependency hell. For sure it is not the only way to manage packages, but I think this is the most straightforward and reliable way to set packages. Happy coding, and until next time!

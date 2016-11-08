# Install R and R packages locally

### Install R locally
This is a quick summary of the steps needed to install [R](https://cran.r-project.org) locally (either on your own computer or a server).   Here I am installing [R 3.2.3 from CRAN](https://cran.r-project.org/src/base/R-3/R-3.2.3.tar.gz). 

1. Create new directory called `R` and subdirectory with the version of R that you are installing. Download tar.gz file from CRAN. Unpack and change into directory. 

>	$ mkdir -p $HOME/packages/R/3.3/	

>	$ mkdir src

>	$ cd src

>	$ wget https://cran.r-project.org/src/base/R-3/R-3.3.1.tar.gz

>	$ tar xzvf R-3.3.1.tar.gz 	

2. Configure and build R:
	
>	$ cd R-3.3.1

>	$ ./configure --prefix=$HOME/packages/R/3.3/

>	$ make	

>	$ make check	

>	$ make install

This step builds and installs R in a local directory.  The important part is to use the `--prefix` parameter which specifies where R should be installed (i.e. in a non-default location).  If you get any "permission denied" issues, it's possible you don't have the ability to write to the location specified in `--prefix`. 
	
3. Add the following line to the `.bash_profile` file in the home directory. 
	
> export PATH=$HOME/packages/R/3.3/bin:$PATH

This step modifies our environment to look for R locally versus globally. This is necessary to use both `R` and `Rscript`. 
	
4. If it's not already created, create a file called `.Rprofile` and place in home directory. Add the following to the file: 
	
> options(repos = structure(c(CRAN = "http://cran.rstudio.com)))

5. If it's not already created, create `.Renviron` and place in home directory. Add the following to the file: 
 
> export R_LIBS=$HOME/packages/R/3.3/lib64/R/library


### Install R packages locally

Once R has been installed locally, start R and install a package like normal. 

	> install.packages("ggplot2") 

The important part to note is that R will look for packages from `"http://cran.rstudio.com` (defined in Step 4 above) and will install them in our local library path (defined in Step 5 above). What it really looks like is 

	> install.packages("ggplot2", repos = http://cran.rstudio.com, lib = "~/R/3.3/lib64/R/library") 
	
As we have already define the local library path and repo location, we can install like above without defining `repos` and `lib`. 

### Install Bioconductor packages

Load the BiocInstaller package and then install the packages like normal. 

	> source("https://bioconductor.org/biocLite.R") ## note: Use http:// if https:// URLs are not supported
	> biocLite("quantro")
	
Alternatively, you can use

	> library(BiocInstaller)
	> biocLite("quantro")

## Helpful Resources:

* [Help with R on Harvard Odyssey clusters from Gabor Csardi](http://airoldilab.github.io/odyssey/)
* [Compiling and installing software on Odyssey](https://rc.fas.harvard.edu/resources/documentation/software-on-odyssey/installing-software-yourself/)
* [Installing local R packages on Odyssey](https://rc.fas.harvard.edu/resources/documentation/software/r/)
* [Installing local R packages from OSU](https://www.osc.edu/documentation/howto/install-local-R-packages)
* [Building latest R-devel or R-3.3.+ on cluster (07/20/2016)](http://pj.freefaculty.org/blog/?p=315) - If try to install the latest R on linux but it says your zlib, bzip2, etc is out of date, then follow this link. R no longer comes packaged with these libraries, so you must install them yourself. 




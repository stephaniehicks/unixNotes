# Install R-devel and Bioconductor devel to update BioC package

### Install R-devel locally
This is a quick summary of the steps needed to install [R-devel](https://stat.ethz.ch/R/daily/) locally (either on your own computer or a server). This is needed to use the [devel version of Bioconductor](https://www.bioconductor.org/developers/how-to/useDevel/)

1. Download the latest [R-devel](https://stat.ethz.ch/R/daily/) and unzip folder. 

>	$ cd ~/src

>	$ wget --no-check-certificate https://stat.ethz.ch/R/daily/R-devel.tar.gz

>	$ tar xzvf R-devel.tar.gz	

2. Configure and build R-devel:
	
>	$ cd R-devel

>	$ ./configure --prefix=$HOME/packages/R/R-devel/

>	$ make	

>	$ make check	

>	$ make install

This step builds and installs R-devel in a local directory.  The important part is to use the `--prefix` parameter which specifies where R-devel should be installed (i.e. in a non-default location).  If you get any "permission denied" issues, it's possible you don't have the ability to write to the location specified in `--prefix`. 
	
To open the R-devel version `R` (or `Rscript`) 

>	$ ~/packages/R/R-devel/bin/R
>	$ ~/packages/R/R-devel/bin/Rscript

3. You may have to change the `.Renviron` file to look in the R-devel to install libraries. You can check in R using `.libPaths()`
 
> export R_LIBS=$HOME/packages/R/R-devel/lib64/R/library



### Install BioC devel

Once you open R-devel 

>	$ ~/packages/R/R-devel/bin/R

First, make sure BiocInstaller is not installed, and then install. You can check if you are using the devel version using `isDevel()`. 

	> remove.packages("BiocInstaller")  # repeat until R says there is no
                                  # package 'BiocInstaller' to remove
	> source("https://bioconductor.org/biocLite.R")  # install correct version
	> BiocInstaller::biocValid()

Install current Bioconductor packages

	> source("https://bioconductor.org/biocLite.R")
	> biocLite()
	> biocLite("minfi") # specific packages
	

### Checkout svn repository on Bioconductor 

For detailed instructions, read [Bioconductor svn documentation for developers](https://www.bioconductor.org/developers/source-control/)

>	$ svn co --username <> --password <> https://hedgehog.fhcrc.org/bioconductor/trunk/madman/Rpacks/quantro

You can add the argument `--username <insertname> --password <insertpassword>` to the `svn co` function if needed. 

Change into the repository and open R-devel. 

> 	$ cd quantro

> 	$ packages/R/R-devel/bin/R 


### Make changes in R-devel and BioC devel

After you have made changes, use `devtools::load_all()`, `devtools::document()`, `devtools::install()`. If you are building on a server, use the following to knit `.Rnw` file. 

> 	$ ~/packages/R/R-devel/bin/Rscript -e "library(knitr); knit2pdf('vignette.Rnw')"

Run `R CMD build` and `R CMD check` until no more warnings, errors, notes. 

> 	$ ~/packages/R/R-devel/bin/R CMD build --no-build-vignettes --resave-data quantro
>	$ ~/packages/R/R-devel/bin/R CMD check --no-build-vignettes quantro_x.y.z.tar.gz
> 	$ ~/packages/R/R-devel/bin/R CMD BiocCheck --no-check-vignettes quantro_x.y.z.tar.gz

### Use `svn` to add, delete and commit changes

Use `svn status`, `svn add` and `svn delete`. Other useful commands are `svn diff` and `svn log`.  

Commit your changes and they changes should be instantly seen in the remote repository.  

>	$ svn commit -m "add message" --username <> --password <>


# Install kallisto locally (without admin privilages)

I followed this [guide to install kallisto local build without root](https://pachterlab.github.io/kallisto/local_build.html) to install [kallisto](https://pachterlab.github.io/kallisto/) from [source](https://pachterlab.github.io/kallisto/source) on odyssey cluster. 

## Install dependencies

I installed the following dependencies:

1. [cmake](https://cmake.org)

```
wget https://cmake.org/files/v3.7/cmake-3.7.0-rc3.tar.gz
tar xvzf cmake-3.7.0-rc3.tar.gz
cd cmake-3.7.0-rc3
./configure --prefix=$HOME/packages
make      
make install
cmake --version
```

2. zlib

Already installed. 

3. HDF5

```
wget https://support.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.17.tar.gz
tar xvzf hdf5-1.8.17.tar.gz
cd hdf5-1.8.17
./configure --prefix=$HOME/packages
make
make check
make install
make check-install
```

## Export paths 

```
export PATH=$HOME/bin:$PATH
export LD_LIBRARY_PATH=$HOME/packages/lib/:$LD_LIBRARY_PATH
export CFLAGS="-I$HOME/packages/include" 
export LDFLAGS="-L$HOME/packages/lib" 
```


## Install kallisto

```
wget https://github.com/pachterlab/kallisto/archive/v0.43.0.tar.gz
tar xvzf v0.43.0.tar.gz
cd kallisto-0.43.0
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=$HOME/packages ..
make
make install
kallisto version

#### Notes 

* I had to change line 13 in `src/h5utils.h` to hard code path to `#include "hdf5.h"` to `#include "/n/home15/shicks/packages/hdf5.h"`





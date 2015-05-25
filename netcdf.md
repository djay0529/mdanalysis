The AMBER NetCDF binary trajectory reader ([Issue 109](https://code.google.com/p/mdanalysis/issues/detail?id=109)) relies on the [netcdf4-python](https://github.com/Unidata/netcdf4-python) package, which in turns depends on
  * hdf5 >= 1.8.7
  * netcdf4 (>4.1)

On modern versions of Ubuntu Linux (>= 11.04) one can install the prerequisite packages
```
apt-get install -y libhdf5-serial-dev libnetcdf-dev
```
and pip-install the [netcdf4-python](https://github.com/Unidata/netcdf4-python) package
```
pip install netCDF4 
```
(See, for instance, the [Install Recipe for Ubuntu 12.04](InstallRecipes#Ubuntu_12.04_%22Precise_Pangolin%22.md).)


Older releases (such as 10.04) require you to build the libraries yourself as described below:

## Installation Examples ##
Even if your specific operating system is not listed, read through the examples and you should be able to adapt them to your situation. Also note that the exact versions of libraries listed the examples are likely outdated: replace them with the current version!

### Debian ###
For debian users getting the error: _ValueError: did not find HDF5 headers_, with the `easy_install` method for installing netcdf but having installed hdf5/netcdf from the official debian repositories, try:
```
export HDF5_LIBDIR=/usr/lib/x86_64-linux-gnu/hdf5/serial 
export HDF5_INCDIR=/usr/include/hdf5/serial
```
and then
```
easy_install netCDF4
```

### Compiling on Ubuntu 10.04 Lucid Lynx ###
Download latest source for the **HDF5** library from [ftp://ftp.hdfgroup.org/HDF5/current/src](ftp://ftp.hdfgroup.org/HDF5/current/src)
```
wget ftp://ftp.hdfgroup.org/HDF5/current/src/hdf5-1.8.9.tar.bz2
tar -jxvf hdf5-1.8.9.tar.bz2
cd hdf5-1.8.9
```
Install into `/usr/local`:
```
HDF5_DIR=/usr/local
./configure --prefix=$HDF5_DIR --enable-hl --enable-shared
make
sudo make install
```


Download latest source code for the **netcdf4** library from [ftp://ftp.unidata.ucar.edu/pub/netcdf/](ftp://ftp.unidata.ucar.edu/pub/netcdf/)
```
wget ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4.2.0.tar.gz
cd netcdf-4.2
./configure --prefix=/usr/local --enable-netcdf-4 --enable-shared --enable-dap --disable-doxygen CPPFLAGS="-I$HDF5_DIR/include" LDFLAGS="-L$HDF5_DIR/lib"
make
sudo make install
```
This also installs into `/usr/local`.

After netcdf4 has been installed, MDAnalysis can be installed with `python setup.py install` and it will automatically install [netcdf4-python](http://code.google.com/p/netcdf4-python/) in the process.

### CentOS 5.8 as user ###
I am installing the HDF5 (from [ftp://ftp.hdfgroup.org/HDF5/current/src](ftp://ftp.hdfgroup.org/HDF5/current/src)) and netcdf (from [ftp://ftp.unidata.ucar.edu/pub/netcdf/](ftp://ftp.unidata.ucar.edu/pub/netcdf/)) libraries in my home directory under `$HOME/Library`. Because this is non-standard location I have to set two environment variables to make this work:
```
export HDF5_DIR=$HOME/Library
export NETCDF4_DIR=$HDF5_DIR 

# compile latest HDF5
wget ftp://ftp.hdfgroup.org/HDF5/current/src/hdf5-1.8.11.tar.bz2
tar xvf hdf5-1.8.11.tar.bz2  && cd hdf5-1.8.11
./configure --prefix=$HDF5_DIR --enable-hl --enable-shared
make && make install

cd ..

# compile latest netcdf4
wget ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4.3.0.tar.gz
tar xvf netcdf-4.3.0.tar.gz && cd netcdf-4.3.0
./configure --prefix=$NETCDF4_DIR --enable-netcdf-4 --enable-shared --enable-dap --disable-doxygen CPPFLAGS="-I$HDF5_DIR/include" LDFLAGS="-L$HDF5_DIR/lib"
make && make install

# install netcdf4-python
easy_install-2.6 --user netcdf4
```


## Problems ##

### error: 'H5F\_LIBVER\_18' undeclared (first use in this function) ###
As [Re: netcdfgroup Trouble compiling netcdf 4.1.3 on Natty Narwhal](http://www.unidata.ucar.edu/mailing_lists/archives/netcdfgroup/2011/msg00241.html) says: Your HDF5 library is out of date. Upgrade to HDF5-1.8.7 and try again. Don't forget to do `make clean`
first.

### /usr/bin/Doxygen not found ###
Add the `--disable-doxygen` flag to netcdf's configure. Will omit generating doxygen docs.
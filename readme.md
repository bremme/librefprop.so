#Welcome to librefprop.so!
These files allow you to compile the REFPROP fluid property database as a shared library for Linux and MacOS systems. This enables you to use the Fortran sources developed by NIST providing an alternative to the REFPROP.dll for Windows. 

## Installation Instructions
For installation on a Linux or OSX machine, please follow the steps described below. By default, the library and the header file are placed in system directories. Please change the paths if you do not have write access to this part of your file system. 

0.  Make sure that you have gcc and gfortran, for OSX use either [HPC](http://hpc.sourceforge.net/) **or** [Homebrew](http://brew.sh/) **and** install the [OSX command line tools](https://developer.apple.com/downloads). On a Linux machine, something like `apt-get install gcc` might do the job.
1.  Get a copy of this repository, either by downloading the latest [release](https://github.com/jowr/librefprop.so/releases/latest) or the current development version as [zip file](https://github.com/jowr/librefprop.so/archive/master.zip) or simply clone the [repository](https://github.com/jowr/librefprop.so.git) with git.
2.  Change the paths in the Makefile around line 40, if needed.
3.  Copy the REFPROP Fortran code to the *fortran* directory.
4.  Put the *fluids* and *mixtures* folders from REFPROP into the *files* folder.
5.  Call `make` to prepare the files. 
6.  Use `sudo make install` (system install) or `make install` (as normal user) to copy the files to the destination directories.

You can remove the files again by calling `make uninstall` (as root user). 

## Testing the Installation
There is a simple Fortran file to test the library. You can call `make fortest` and run the executable with `./bin/ex_mix_for` to display some R410 two-phase properties:

| Temperature | Pressure  | Density, liquid | Density, vapour |
|-------------|-----------|-----------------|-----------------|
| 300.0000    | 1740.5894 |   14.4550       |   0.9628        |
| 300.0000    | 1735.1589 |   14.2345       |   0.9603        |


## Python Integration
There is a basic python package based on the examples from
[NIST](http://www.boulder.nist.gov/div838/theory/refprop/LINKING/Linking.htm#PythonApplications "NIST homepage")
in the `pyrp` folder. 

Please note that there is a much more mature Python interface available at https://github.com/BenThelen/python-refprop. Thank you Ben for sharing it!

## Matlab Integration
There is a Matlab prototype file available from
[NIST](http://www.boulder.nist.gov/div838/theory/refprop/LINKING/Linking.htm#MatLabApplications "NIST homepage"). Unfortunately, you have to change a few things in order to use the library on MacOS and GNU/Linux.

There is a makefile section and a shell script that help you with this. After installing the library as described above, you can run `make matlab` in order to use REFPROP with Matlab. Then run `make matlab-install’ as root user for a system-wide installation. 

The test.m is a simple code you can use to check if the intergration works.

### Installation steps
  * Get the Git repository and install the library *(Skip this if your administrator already installed REFPROP for you)*.
  * Go to the directory with the downloaded librefprop.so files and open a command prompt.
  * Run `make matlab` to download files and edit them as written in the terminal.
  * Make sure to put the `refpropm.m` file into a folder that is in your MATLABPATH, run `addpath(FOLDERNAME)` if required.

### MATLAB 64 bit Integration
This part was contributed partly by [nkampy](https://github.com/nkampy) and [speredenn](https://github.com/speredenn) and is still experimental. Please open new issues if you encounter any problems. Problems are likely to be encountered in setting up matlab with gcc, needed to use the builtin MEX functionality, which is required for the load library command in the thunk.m file. We hope that the user community and [nkampy's](https://github.com/nkampy) comments, left at the mathworks website ([here](http://www.mathworks.com/matlabcentral/answers/125301-maverick-r2014a-loadlibrary-error-loaddefinedlibrary) and [here](http://www.mathworks.com/matlabcentral/answers/124597-how-to-setup-gfortran-on-mac-osx-10-9-and-matlab-r2014a)), will help figuring out a good solution.

## No root user access
It is possible to use the shared libraries without root access. However, you need to make sure that the libraries get found and it is recommended to add something like `export LD_LIBRARY_PATH=/home/USERNAME/lib:/home/USERNAME/refprop:$LD_LIBRARY_PATH` to the calls to executables that need REFPROP. The makefile will print more instructions when running `make install` as a non-root user.

## Known Problems
  * Older compilers might not work properly with the OpenMP directives used in the original Fortran code. If you experience any problems related to OpenMP, try removing OpenMP support by setting `USEOPENMP  :=FALSE` in line 51 of the Makefile.

## General Remarks
Please note that you need a working and licensed copy of REFPROP in order to use the software provided here. This is not a replacement for REFPROP. You can purchase REFPROP at http://www.nist.gov/srd/nist23.cfm

If you are interested in fluid property modelling, you might also be interested in [CoolProp](https://github.com/ibell/coolprop), an open-source thermodynamic fluid property package with over 100 compressible and over 50 incompressible fluids.

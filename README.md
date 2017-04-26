# GDAL, HDF5 with Python2 binding

This repo produces a Docker image that can read/write HDF5 files with the following compression algorithms implemented as HDF5 plugin filters:

- [blosc-32001](https://github.com/Blosc/hdf5-blosc)
- [HDF5 LPC-Rice filter v1.0-r75-32010](https://sourceforge.net/projects/lpcrice/)

The image contains `docker-entrypoint.sh` script that honors these environment variables:

- RUNUSER_UID - Numeric UID of the real user so programs are run as non-root user, `runuser` in the container
- RUNUSER_HOME - Home directory of `runuser` in the container. Use a Docker mount volume to persist it.

Below explains some crucial steps in the Dockerfile.

### hdf5-blosc

This requires the latest C-Blosc library. Ubuntu Xenial provides version 1.7.0 of `libblosc-dev libblosc1` but `hdf5-blosc` doesn't support any C-Blosc 1.8.0 or earlier releases. See https://github.com/Blosc/hdf5-blosc/blob/master/RELEASE_NOTES.txt

`hdf5-blosc` installs the libraries to `/usr/local/lib/libblosc*` so it is necessary to create the symlinks in `/usr/local/hdf5/lib/plugin/`.

### lpcrice-hdf-filter

This `Makefile` in the [source tarball](https://sourceforge.net/projects/lpcrice/files/latest/download) does not compile out of the box on Ubuntu Xenual due to missing include directory and library. The output also needs to be copied to `/usr/local/hdf5/lib/plugin/`.

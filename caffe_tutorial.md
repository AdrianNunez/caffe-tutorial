# Caffe Installation

This guide is based on my own experience with Caffe. I use Ubuntu 15.10.

### General Dependencies

	sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler libgflags-dev libgoogle-glog-dev liblmdb-dev

### Boost Installation

	sudo apt-get install libboost-all-dev

### ATLAS Installation

	sudo apt-get install libatlas-base-dev

### CUDA Installation

```
cd ~
wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
chmod +x cuda_7.5.18_linux.run
./cuda_7.5.18_linux.run --extract=/home/$USER
sudo ./cuda-linux64-rel-7.5.18-19867135.run
sudo bash -c "echo /usr/local/cuda/lib64/  /etc/ld.so.conf.d/cuda.conf"
sudo ldconfig
```

#### Hacking CUDA

In /usr/local/cuda/include/host_config.h change the following line:

```
--- #error -- unsupported GNU version! gcc versions later than 4.9 are not supported!
+++ //#error -- unsupported GNU version! gcc versions later than 4.9 are not supported!
```

### Install Caffe

```
cd ~
git clone https://github.com/BVLC/caffe.git
cd caffe
cp Makefile.config.example Makefile.config
```

Now we will modify the Makefile.config file. Change the INCLUDE_DIRS y LIBRARY_DIRS variables so that they end up like this:

```
gedit Makefile.config
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial
```

```
make all
make test
sudo make runtest
```

# Errors with Caffe

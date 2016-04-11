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

###### Check failed: error == cudaSuccess (30 vs. 0)
Use sudo once.

###### Check failed: error == cudaSuccess (8 vs. 0)  invalid device function
Bug

###### error == cudaSuccess (38 vs. 0)  no CUDA-capable device is detected
I just rebooted the computer.

###### fatal error: boost/shared_ptr.hpp: No such file or directory
Execute the following:

	sudo apt-get install libboost-all-dev

###### fatal error: hdf5.h: No such file or directory
Error with hdf5, I used the following to solve the error:

```
gedit Makefile.config
Modificar INCLUDE_DIRS añadiendo al final /usr/include/hdf5/serial/
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
```

###### fatal error: numpy/arrayobject.h: No such file or directory
Error from numpy library, I used the following to solve the error:

```
sudo apt-get install python-numpy
make clean
make pycaffe
```

###### caffe.ph.h not found

From the caffe root:

```
protoc src/caffe/proto/caffe.proto --cpp_out=.
mkdir include/caffe/proto
mv src/caffe/proto/caffe.pb.h include/caffe/proto
```

###### undefined reference to boost::system::system_category()
This error appears when I use Caffe with Eclipse. Solved by including boost_system:

	Eclipse: Project > Properties > C/C++ Build > Settings > Linker > Libraries: añadir boost_system a -l


###### undefined reference to `google::LogMessage
This error appears when I use Caffe with Eclipse. Solved by including glog:

	Eclipse: Project > Properties > C/C++ Build > Settings > Linker > Libraries: añadir glog a -l




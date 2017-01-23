# caffe_install_cmake
Notes for caffe install for centos_7.0:

1.install dependencies:
  sudo yum install protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel
  sudo yum install gflags-devel glog-devel lmdb-devel
  
2.install CUDA,7.0 for example:
  download: wget http://developer.download.nvidia.com/compute/cuda/7_0/Prod/local_installers/cuda_7.0.28_linux.run
  install:
  chmod +x ./cuda_7.0.28_linux.run
  # 阅读许可协议，按q退出阅读，accept接受
  Do you accept the previously read EULA? (accept/decline/quit): accept
  # 安装驱动，y
  Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 352.39? ((y)es/(n)o/(q)uit): y
  # 管理员权限安装，y
  Do you wish to run the installation with 'sudo'? ((y)es/(n)o): y
  # OpenGL安装，y
  Do you want to install the OpenGL libraries? ((y)es/(n)o/(q)uit) [ default is yes ]: y
  # 安装cuda，必须y，要不白装
  Install the CUDA 7.5 Toolkit? ((y)es/(n)o/(q)uit): y
  # 安装位置，默认的位置就不错
  Enter Toolkit Location [ default is /usr/local/cuda-7.5 ]:
  # 建立符号链接（可理解成快捷方式）如果本机第一次装cuda，建议y，这里本机装过7.0，所以没必要再链接一次
  #这样系统默认指向的cuda是7.0而不是7.5
  Do you want to install a symbolic link at /usr/local/cuda? ((y)es/(n)o/(q)uit): n
  # 例子
  Install the CUDA 7.5 Samples? ((y)es/(n)o/(q)uit): y
  Enter CUDA Samples Location [ default is /home/lan_dev ]:
  Installing the NVIDIA display driver...
  
3.install openCV:
  wget ftp://192.168.1.12/Public/meteo/caffe_env/opencv-2.4.13.zip
  unzip opencv-2.4.13.zip
  cd opencv-2.4.13
  mkdir build && cd build
  cmake ..
  make -j8
  make install
   
4.install cuDNN:
  download,unzip and copy to the path:
  cuda/include   →   /usr/local/include
  cuda/lib64 →   /usr/local/lib64

5.install caffe:
  download caffe
  mkdir build && cd build
  cmake \
  -D BOOST_ROOT=/usr/local/boost/ \
  -D CMAKE_INSTALL_PREFIX=/usr/local/moji/caffe \
  -D GLOG_LIBRARY=/usr/lib64/libglog.so \
  -D GLOG_LIBRARIES=/usr/lib64/libglog.so \
  -D GLOG_INCLUDE_DIR=/usr/include/  \
  -D PROTOBUF_LIBRARIES=/usr/lib64/libprotobuf.so \
  -D PROTOBUF_INCLUDE_DIR=/usr/include/ \
  -D PROTOBUF_LIBRARY=/usr/lib64/libprotobuf.so \
  -D GFLAGS_LIBRARIES=/usr/lib64/libgflags.so \
  -D GFLAGS_INCLUDE_DIR=/usr/include/ \
  -D GFLAGS_LIBRARY=/usr/lib64/libgflags.so \
  -D Atlas_CBLAS_LIBRARY=/usr/lib64/atlas/libtatlas.so \
  -D Atlas_BLAS_LIBRARY=/usr/lib64/atlas/libtatlas.so \
  -D Atlas_LAPACK_LIBRARY=/usr/lib64/atlas/libtatlas.so \
  -D PYTHON_EXECUTABLE=/usr/bin/python2.7 \
  -D PYTHON_INCLUDE_DIR=/usr/include/ \
  -D PYTHON_LIBRARIES=/usr/lib64/libpython2.7.so \
  -D PYTHON_LIBRARY=/usr/lib64/libpython2.7.so ..

FROM nvidia/cuda:9.1-cudnn7-devel-ubuntu16.04

LABEL maintainer="ichizero"

RUN apt update

RUN apt install -y \
  build-essential \
  cmake \
  wget \
  unzip \
  git

# Python3
RUN apt install -y \
  python3-dev \
  python3-pip \
  python3-wheel \
  python3-setuptools \
  python3-tk

RUN pip3 --no-cache-dir install \
  cupy-cuda91 \
  numpy

# ref) http://www.python36.com/how-to-install-opencv340-on-ubuntu1604/
RUN apt install -y \
  libgtk2.0-dev \
  pkg-config \
  libavcodec-dev \
  libavformat-dev \
  libswscale-dev

# To process images
RUN apt install -y \
  python-dev \
  python-numpy \
  libtbb2 \
  libtbb-dev \
  libjpeg-dev \
  libpng-dev \
  libtiff-dev \
  libjasper-dev \
  libdc1394-22-dev

# To process videos
RUN apt install -y \
  libavcodec-dev \
  libavformat-dev \
  libswscale-dev \
  libv4l-dev

RUN apt install -y \
  libxvidcore-dev \
  libx264-dev

# For GUI
RUN apt install -y libgtk-3-dev

# For optimization
RUN apt install -y libatlas-base-dev gfortran pylint

WORKDIR /
RUN wget https://github.com/opencv/opencv/archive/3.4.1.zip -O opencv-3.4.1.zip \
  && wget https://github.com/opencv/opencv_contrib/archive/3.4.1.zip -O opencv_contrib-3.4.1.zip \
  && unzip -qq opencv-3.4.1.zip \
  && unzip -qq opencv_contrib-3.4.1.zip \
  && cd opencv-3.4.1 \
  && mkdir build \
  && cd build \
  && cmake -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.1/modules \
  -D CUDA_GENERATION=Auto \
  -D WITH_CUDA=ON \
  -D ENABLE_FAST_MATH=1 \
  -D CUDA_FAST_MATH=1 \
  -D WITH_CUBLAS=1 \
  -D CUDA_NVCC_FLAGS="-D_FORCE_INLINES --expt-relaxed-constexpr" \
  -D BUILD_TIFF=ON \
  -D BUILD_opencv_java=OFF \
  -D ENABLE_AVX=ON \
  -D FORCE_VTK=ON \
  -D WITH_OPENGL=ON \
  -D WITH_OPENCL=ON \
  -D WITH_IPP=ON \
  -D WITH_TBB=ON \
  -D WITH_QT=ON \
  -D WITH_GDAL=ON \
  -D WITH_EIGEN=ON \
  -D WITH_XINE=ON\
  -D WITH_V4L=ON \
  -D BUILD_TESTS=OFF \
  -D BUILD_PERF_TESTS=OFF \
  -D BUILD_opencv_python3=yes \
  -D PYTHON_EXECUTABLE=$(which python3) \
  -D PYTHON_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
  -D PYTHON_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") .. \
  && make -j8 \
  && make install \
  && /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' \
  && ldconfig \
  && rm /opencv-3.4.1.zip \
  && rm /opencv_contrib-3.4.1.zip \
  && rm -r /opencv-3.4.1 \
  && rm -r /opencv_contrib-3.4.1

RUN apt clean
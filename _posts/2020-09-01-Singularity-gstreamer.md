---
title: "Singularity and gstreamer"
date: 2020-09-01
categories: ["singularity", "gstreamer"]
---

## Singularity Recipe file:
```
Bootstrap: docker
From: nvidia/cuda:11.0-runtime-ubuntu20.04

%files 
        NV-418.67.tar /opt

%environment
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib64/:/usr/lib64/gstreamer-1.0/
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/NV-418.67/:/usr/local/cuda/lib64
        export PATH=/usr/local/cuda/bin:/opt/NV-418.67/:$PATH
        export CPATH=/usr/local/cuda/include:$CPATH
        export CUDA_HOME=/usr/local/cuda
        export GST_PLUGIN_PATH_1_0=/usr/lib/x86_64-linux-gnu/gstreamer-1.0/:/usr/lib/x86_64-linux-gnu/gstreamer1.0:/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/

%post
        RELEASE=1.17.2
        export DEBIAN_FRONTEND=noninteractive

        apt update
        apt install -y gcc make cmake g++ meson xz-utils wget less git libglib2.0-dev flex bison gobjc++ libopengl-dev libavfilter7 libavfilter-dev libglu1-mesa-dev freeglut3-dev libgl-dev libglobjects1 libglobjects-dev xterm x264 x265 python-gobject python3-gst-1.0 python3-gi python-gi-dev python3.8-dev libjson-glib-1.0-0 libjson-glib-1.0-common libjson-glib-dev libgstrtspserver-1.0-0 libgstrtspserver-1.0-dev
        apt install -y libopencv-core-dev libopencv-core4.2 libopencv-contrib-dev libopencv-contrib4.2 libopencv-dev 

        cd /opt
        tar xf NV-418.67.tar
        cp /opt/NV-418.67/libnvcuvid.so.418.67 /opt/NV-418.67/libnvcuvid.so.1
        cp /opt/NV-418.67/libnvcuvid.so.418.67 /opt/NV-418.67/libnvcuvid.so
        cp /opt/NV-418.67/libnvidia-encode.so /usr/lib/x86_64-linux-gnu/
        cp /opt/NV-418.67/libnvcuvid.so /usr/lib/x86_64-linux-gnu/


        cd /opt
        wget https://gstreamer.freedesktop.org/src/gstreamer/gstreamer-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-plugins-base/gst-plugins-base-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-plugins-good/gst-plugins-good-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-plugins-bad/gst-plugins-bad-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-plugins-ugly/gst-plugins-ugly-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-libav/gst-libav-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-python/gst-python-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-rtsp-server/gst-rtsp-server-${RELEASE}.tar.xz
        wget https://gstreamer.freedesktop.org/src/gst-devtools/gst-devtools-${RELEASE}.tar.xz

        unxz gstreamer-${RELEASE}.tar.xz
        tar xf gstreamer-${RELEASE}.tar
         cd gstreamer-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release
         ninja
         ninja install
         
        export GST_PLUGIN_PATH_1_0=/usr/lib/x86_64-linux-gnu/gstreamer-1.0/:/usr/lib/x86_64-linux-gnu/gstreamer1.0:/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/

        cd /opt
        TARGET=gst-plugins-base
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-plugins-good
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-plugins-bad
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig -Dnvcodec=enabled
         ninja
         ninja install

        cd /opt
        TARGET=gst-plugins-ugly
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-libav
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-rtsp-server
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-devtools
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install

        cd /opt
        TARGET=gst-python
        unxz ${TARGET}-${RELEASE}.tar.xz
        tar xf ${TARGET}-${RELEASE}.tar
         cd ${TARGET}-${RELEASE}
         mkdir build-sing
         cd build-sing
         meson  --prefix=/usr -Dbuildtype=release -Dpkg_config_path=/usr/lib64/pkgconfig
         ninja
         ninja install
```


## Links
* http://www.linuxfromscratch.org/blfs/view/svn/multimedia/gst10-plugins-base.html
* http://gpucomputing.shef.ac.uk/education/creating_gpu_singularity/
* http://lifestyletransfer.com/how-to-install-nvidia-gstreamer-plugins-nvenc-nvdec-on-ubuntu/
* https://medium.com/@nareshkumarganesan/nvidia-hardware-accelerated-video-encoding-decoding-nvcodec-gstreamer-4b8eab662bf1

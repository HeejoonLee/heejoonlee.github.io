---
title: "ORB-SLAM 2 Test Run"
date: 2022-05-15 04:51:00 +0900
categories: [Computer Vision, SLAM]
tags: [computer vision, slam, orb-slam]
---

## Prerequisites
* OpenCV
* Eigen3
* Pangolin
* Xming(required only in WSL2)

### OpenCV
ref: [https://linuxize.com/post/how-to-install-opencv-on-ubuntu-18-04/][opencv_ref]
1. Install dependencies:
```bash
$ sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev
```

2. Create *opencv* directory and clone OpenCV and OpenCV_contrib repos:
```bash
$ mkdir opencv && cd opencv
opencv$ git clone https://github.com/opencv/opencv.git
opencv$ git clone https://github.com/opencv/opencv_contrib.git
```

3. Select required OpenCV version using *git checkout*. **OpenCV 3.9.0** is selected for this installation:
```bash
$ cd ~/opencv/opencv
opencv/opencv$ git checkout 3.9.0
$ cd ~/opencv/opencv_contrib
opencv/opencv_contrib$ git checkout 3.9.0
```

4. Create a *build* directory in opencv repo and run *CMake*:
```bash
$ cd ~/opencv/opencv
opencv/opencv$ mkdir build && cd build
opencv/opencv/build$ cmake -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
```

5. Compile OpenCV:
```bash
opencv/opencv/build$ make -j8
```

6. Install OpenCV:
```bash
opencv/opencv/build$ sudo make install
```


### Eigen3

```bash
$ sudo apt install libeigen3-dev
```

### Pangolin

1. Clone **Pangolin** repository
```bash
$ git clone --recursive https://github.com/stevenlovegrove/Pangolin.git
$ cd Pangolin
```

2. Install dependencies
```bash
$ ./scripts/install_prerequisites.sh recommended
```

3. Choose *v0.5* branch using `git checkout v0.5`
```bash
$ git checkout v0.5
```
    * Any versions higher than *v0.5* are not compatible with *ORB-SLAM2*

4. Configure and build
```bash
$ cmake -B build
$ cmake --build build
```

### Xming

TODO: (https://evandde.github.io/wsl2-x/)


## ORB-SLAM 2

1. Clone **ORB-SLAM 2** repository
```bash
$ git clone https://github.com/raulmur/ORB_SLAM2.git ORB_SLAM2
```

2. Run *build.sh*
```bash
$ cd ORB_SLAM2
$ ./build.sh
```

---

### ERRORS
#### ORB-SLAM 2

1. **error: usleep is not declared in this scope**

    Add `#include <unistd.h>` in `include/System.h`

    (ref: [https://github.com/raulmur/ORB_SLAM2/issues/337][fix2])

2. **error: std::map must have the same value_type as its allocator**

    Switch *const* and *KeyFrame\** in line 50 of `include/LoopClosing.h`:

    before
    ```cpp
    Eigen::aligned_allocator<std::pair<const KeyFrame*, g2o::Sim3> > > KeyFrameAndPose;
    ```
    after
    ```cpp
    Eigen::aligned_allocator<std::pair<KeyFrame* const, g2o::Sim3> > > KeyFrameAndPose;
    ```
    (ref: [http://blog.leanote.com/post/gaunthan/ORB-SLAM2-compiling-problems][fix1])

3. **OpenCV > 2.4.3 not found.**

    Install OpenCV

4. **Could NOT find Eigen3 (missing: EIGEN3_INCLUDE_DIR EIGEN3_VERSION_OK)**

    Install Eigen3


#### Pangolin

Error Message:
```
 By not providing "FindPangolin.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Pangolin",
  but CMake did not find one.
```

Fix:

Install **Pangolin**

Pangolin fix reference: [https://blog.csdn.net/Robert_Q/article/details/121690089][csdn-blog]

Pangolin issues with X11: <https://woojjang.tistory.com/52>

[csdn-blog]: https://blog.csdn.net/Robert_Q/article/details/121690089
[fix1]: http://blog.leanote.com/post/gaunthan/ORB-SLAM2-compiling-problems
[fix2]: https://github.com/raulmur/ORB_SLAM2/issues/337
[opencv_ref]: https://linuxize.com/post/how-to-install-opencv-on-ubuntu-18-04/
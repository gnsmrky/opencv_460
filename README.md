[This repo](https://github.com/gnsmrky/opencv_460.git) is a fork of [OpenCV 4.6.0](https://github.com/opencv/opencv/tree/4.6.0) with added notes for building with Python 3 and Vulkan support.

## Build OpenCV 4.6.0
Steps to build OpenCV 4.6.0 on the following distros.  It builds and installs OpenCV 4.6.0 and python binding to user local folder.

1. Ubuntu 22.04 LTS
1. Windows 11 WSL2 Ubuntu 22.04
1. Chromebook OS 102 (Bullseye)

For Ubuntu prior 22.04 or Debian prior Bullseye, follow [LunarG Vulkan SDK Packages](https://packages.lunarg.com/) to setup Vulkan SDK. 

### Build steps:  

1. Install dependencies.

   ```
   sudo apt install build-essential cmake libvulkan-dev python3-dev python3-pip python3-numpy 
   ```

1. Setup python dependencies.
   - Setup and switch to the desired `virtualenv` if needed.
   ```
   pip3 install virtualenv
   python3 -m virtualenv ~/py3env
   source ~/py3env/bin/activate
   ```

   - Install `numpy`  (needed of building python bindings)
    ```
    pip install numpy
    ```

1. Get [this repo](https://github.com/gnsmrky/opencv_460.git).
    ```
    git clone https://github.com/gnsmrky/opencv_460.git
    ```

1. Create build folder.
    ```
    cd opencv_460
    mkdir build_460 && cd build_460
    ```

1. `cmake` config (auto python package path finding)
   - run `cmake` with vulkan support.
    ```
    cmake -D CMAKE_BUILD_TYPE=Release \
    -D BUILD_opencv_python2=OFF \
    -D BUILD_opencv_python3=ON \
    -D HAVE_VULKAN=ON \
    -D PYTHON3_EXECUTABLE=$(which python3) \
    -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
    -D PYTHON3_PACKAGES_PATH=$(python3 -c "from sysconfig import get_path; print(get_path('platlib'));")  \
    -D CMAKE_INSTALL_PREFIX=~/usr/local ..
    ```

### Build & install:  
- Installs to `~/usr/local` as specified by `cmake -D CMAKE_INSTALL_PREFIX`
- Python binding is installed to `virtualenv` if used (`py3env` in the above steps).
```
make -j8
make install
```

### Test OpenCV with python:
Run the following should print out `4.6.0`.
```
python3 -c "import cv2; print(cv2.__version__);"
```


### Reference - `cmake` config output:
   - Check the `Python 3: install path` is generated correctly.  If not, `numpy` probably was not installed correctly.)  
   - Ubuntu 22.04 LTS `cmake` output.
```
--   OpenCV modules:
--     To be built:                 calib3d core dnn features2d flann gapi highgui imgcodecs imgproc ml objdetect photo python3 stitching ts video videoio
--     Disabled:                    world
--     Disabled by dependency:      -
--     Unavailable:                 java python2
--     Applications:                tests perf_tests apps
--     Documentation:               NO
--     Non-free algorithms:         NO
-- 
--   GUI:                           GTK2
--     GTK+:                        YES (ver 2.24.33)
--       GThread :                  YES (ver 2.72.1)
--       GtkGlExt:                  NO
--     VTK support:                 NO
-- 
--   Media I/O: 
--     ZLib:                        /usr/lib/x86_64-linux-gnu/libz.so (ver 1.2.11)
--     JPEG:                        /usr/lib/x86_64-linux-gnu/libjpeg.so (ver 80)
--     WEBP:                        build (ver encoder: 0x020f)
--     PNG:                         /usr/lib/x86_64-linux-gnu/libpng.so (ver 1.6.37)
--     TIFF:                        /usr/lib/x86_64-linux-gnu/libtiff.so (ver 42 / 4.3.0)
--     JPEG 2000:                   build (ver 2.4.0)
--     OpenEXR:                     build (ver 2.3.0)
--     HDR:                         YES
--     SUNRASTER:                   YES
--     PXM:                         YES
--     PFM:                         YES
-- 
--   Video I/O:
--     DC1394:                      NO
--     FFMPEG:                      YES
--       avcodec:                   YES (58.134.100)
--       avformat:                  YES (58.76.100)
--       avutil:                    YES (56.70.100)
--       swscale:                   YES (5.9.100)
--       avresample:                NO
--     GStreamer:                   NO
--     v4l/v4l2:                    YES (linux/videodev2.h)
-- 
--   Parallel framework:            pthreads
-- 
--   Trace:                         YES (with Intel ITT)
-- 
--   Other third-party libraries:
--     Intel IPP:                   2020.0.0 Gold [2020.0.0]
--            at:                   /home/gnsmrky/_dev/opencv_460/build_460/3rdparty/ippicv/ippicv_lnx/icv
--     Intel IPP IW:                sources (2020.0.0)
--               at:                /home/gnsmrky/_dev/opencv_460/build_460/3rdparty/ippicv/ippicv_lnx/iw
--     VA:                          NO
--     Lapack:                      NO
--     Eigen:                       NO
--     Custom HAL:                  NO
--     Protobuf:                    build (3.19.1)
-- 
--   Vulkan:                        YES
--     Include path:                NO
--     Link libraries:              Dynamic load
-- 
--   OpenCL:                        YES (no extra features)
--     Include path:                /home/gnsmrky/_dev/opencv_460/3rdparty/include/opencl/1.2
--     Link libraries:              Dynamic load
-- 
--   Python 3:
--     Interpreter:                 /home/gnsmrky/py3env/bin/python3 (ver 3.10.4)
--     Libraries:                   /usr/lib/x86_64-linux-gnu/libpython3.10.so (ver 3.10.4)
--     numpy:                       /home/gnsmrky/py3env/lib/python3.10/site-packages/numpy/core/include (ver 1.22.4)
--     install path:                /home/gnsmrky/py3env/lib/python3.10/site-packages/cv2/python-3.10
-- 
--   Python (for build):            /home/gnsmrky/py3env/bin/python3
-- 
--   Java:                          
--     ant:                         NO
--     JNI:                         NO
--     Java wrappers:               NO
--     Java tests:                  NO
-- 
--   Install to:                    /home/gnsmrky/usr/local
-- -----------------------------------------------------------------
-- 
-- Configuring done
-- Generating done
-- Build files have been written to: /home/gnsmrky/_dev/opencv_460/build_460
```



# Original Readme...
## OpenCV: Open Source Computer Vision Library

### Resources

* Homepage: <https://opencv.org>
  * Courses: <https://opencv.org/courses>
* Docs: <https://docs.opencv.org/4.x/>
* Q&A forum: <https://forum.opencv.org>
  * previous forum (read only): <http://answers.opencv.org>
* Issue tracking: <https://github.com/opencv/opencv/issues>
* Additional OpenCV functionality: <https://github.com/opencv/opencv_contrib> 


### Contributing

Please read the [contribution guidelines](https://github.com/opencv/opencv/wiki/How_to_contribute) before starting work on a pull request.

#### Summary of the guidelines:

* One pull request per issue;
* Choose the right base branch;
* Include tests and documentation;
* Clean up "oops" commits before submitting;
* Follow the [coding style guide](https://github.com/opencv/opencv/wiki/Coding_Style_Guide).

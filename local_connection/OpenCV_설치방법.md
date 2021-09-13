# OPEN CV 설치방법

우리는 객체 탐지를 위해 Open cv 4.5.1 버젼을 사용할 것입니다.

## build-essential 패키지 사용

여기서 필요한 것은 C 및 C++ 컴파일러입니다. 그리고 라이브러리와 make 같은 도구들이 있어야 하는데, 이런 포함된 패지키가  build-essential 입니다.
그리고 cmake는 openCV의 모듈 및 다른 패키지 설정하는데에도 필요합니다.

 ``` sudo apt-get install build-essential cmake ```
 
 ## 필요한 패키지 설치
 
 OPEN CV에서 필요한 패키지들을 설치합니다.
 
 ``` sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev ``` 
 
 ## FFmpeg 구동하기 위한 패키지 설치
 
 비디오 스트리밍을 하기 위해 FFmpeg 패키지를 설치합니다.
 특정 코덱의 비디오 스트리밍을 기록하거나 읽는데 사용됩니다.

``` sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libxvidcore-dev libx264-dev libxine2-dev ```

## Video4Linux 패키지 설치

리눅스에서 실시간으로 스트리밍 하기 위한 드라이버와 API가 담겨있습니다.

``` sudo apt-get install libv4l-dev v4l-utils ```

## GStreamer 패키지 설치

FFmpeg처럼 특정 코덱의 비디오 스트리밍을 기록하거나 읽는데 사용되는 GStreamer 패키지 입니다.

``` sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly ```

## libgtk2.0-dev 패키지 설치

이미지나 영상의 윈도우(창) 생성을 위해 GUI는 GTK를 이용합니다.

``` sudo apt-get install libgtk2.0-dev ```

## OpenGL에 필요한 라이브러리 설치

그래픽 지원을 위해 설치합니다.

``` sudo apt-get install libgtk2.0-dev ```

## OpenCV 최적화 라이브러리 설치

``` sudo apt-get install libatlas-base-dev gfortran libeigen3-dev ```

## 파이썬에 필요한 헤드파일과 라이브러리 설치

```  sudo apt-get install python2.7-dev python3-dev python-numpy python3-numpy ```

## OpenCV 설정 및 컴파일 설치

소스코드를 저장하기 위해 임시 디렉토리를 만들어줍니다.
홈 디렉토리에 ```mkdir opencv ```를 통해 opencv 디렉토리를 생성해주고, 
``` cd opencv ```를 통해 opencv의 디렉토리로 이동합니다.

## OpenCV 4.5.1 소스코드 다운 및 압축풀기

OpenCV 4.5.1 소스코드를 다운로드 합니다. 
``` wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.1.zip ```

그리고 unzip을 통해 압축을 해제합니다.
``` unzip oepncv.zip ```

추가적인 모듈 opencv_contrib도 다운로드 합니다.
``` wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.1.zip ```

그리고 unzip을 통해 압축을 해제합니다.
``` unzip oepncv_contrib.zip ```


## Opencv-4.5.1 컴파일

Open CV는 build 디렉토리에서 컴파일을 해야합니다.
``` cd opencv/opencv-4.5.1 ```로 opencv-4.5.1 디렉토리로 이동 후에 ``` mkdir build ```로 build 디렉토리를 생성합니다.

그리고 ``` cd build ```로 build 디렉토리로 이동합니다.
cmake를 활용하여 다음과 같은 컴파일을 설정합니다.

해당 작업은 무선랜이 연결된 곳에서 하기 바랍니다.

```
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.5.1/modules 


cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=OFF -D
WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D
INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=OFF -D
BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D ENABLE_NEON=ON -D ENABLE_VFPV3=ON -D
WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON -D OPENCV_ENABLE_NONFREE=ON -D
OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.5.1/modules -D WITH_V4L=ON -D
WITH_FFMPEG=ON -D WITH_XINE=ON -D ENABLE_PRECOMPILED_HEADERS=OFF -D
BUILD_NEW_PYTHON_SUPPORT=ON -D OPENCV_GENERATE_PKGCONFIG=ON ../
```

## make 컴파일

build디렉토리에서 다음과 같은 명령어를 입력하여 컴파일을 합니다.

``` time make -j4 ```

## 컴파일 설치

``` sudo make install ```

## OpenCV 설치 확인

```pkg-config --modversion opencv4```를 통해 4.5.1 버젼 확인을 할 수 있습니다.

이 작업이 성공적으로 완료되면, 문제없이 open cv를 사용할 수 있습니다.


# 로컬 환경에서 스트리밍 하기

같은 동일 네트워크에서 스트리밍을 하는 방법입니다.
영상스트리밍 패키지는 Mjpeg-streamer를 사용합니다.

(이미지)

해당 패키지는 다른 스트리밍 패키지에 비해 가장 빠른 응답시간을 보여줍니다.
그리고 딥러닝 모델 CNN, YOLO에서 잘 작동합니다.



>준비물

라즈베리파이보드, 카메라 v2 또는 12MP 고품질 HQ모델 광각렌즈, 무선랜

OPEN-CV 4.5.1이 기본적으로 설치되어야 하고 카메라 권한이 허용되어야 합니다.

OPEN CV설치 방법은 위에 파일로 업로드되어있습니다.

>필요한 라이브러리

camke, python-pil, python3-pil, libjpeg-dev, build-essential



# Mjpg_streamer 설치하는방법

## git clone에서 다운로드
git clone을 통해 mjpg-streamer에 대한 패키지를 다운로드 합니다.


먼저 git을 설치합니다.
``` sudo apt-get install git```

그리고 소스코드를 다운로드 합니다.
``` git clone https://github.com/jacksonliam/mjpg-streamer.git ```

## mjpg-streamer 컴파일

mjpg-streamer를 컴파일 하기 위에 다음과 같은 패키지를 설치합니다.

``` sudo apt-get install cmake python-pil python3-pil libjpeg-dev build-essential  ```

## 컴파일 에러발생시 대처사항

보통 OPEN CV를 사용할 경우 컴파일 에러가 발생되는데, nano 및 vi 편집기를 열어서 수정합니다:

``` nano mjpg-streamer/mjpg-streamer-experimental/plugins/input_opencv/input_opencv.cpp ```

문자열 중 compression_params.push_back을 찾아서 다음과 같이 수정합니다.

```compression_params.push_back(CV_IMWRITE_JPEG_QUALITY);``` 이 부분을
```compression_params.push_back(cv::IMWRITE_JPEG_QUALITY);```로 수정하면 됩니다.

## mjpg-streamer 컴파일 및 설치 진행

mjpg-streamer 소스코드가 있는 디렉토리에서 컴파일과 설치를 해야합니다.
``` cd mjpg-streamer/mjpg-streamer-experimental/ ```

그리고 터미널 창에 CMAKE_BUILD_TYPE=Debug를 생성합니다.
``` make CMAKE_BUILD_TYPE=Debug ```

그리고 make를 설치합니다.
``` sudo make install ```

이제 mjpg-streamer에 대한 컴파일을 완료하였습니다. 

## 스트리밍 코드 작성

컴파일 완료 후 홈 디렉토리로 이동합니다. 
``` cd ```

홈 디렉토리에서 mjpg.sh를 수정합니다. 
``` sudo nano mjpg.sh ```

그리고 다음과 같이 입력합니다. 

``` 
(export STREAMER_PATH=$HOME/mjpg/mjpg-streamer/mjpg-streamer-experimental
export LD_LIBRARY_PATH=$STREAMER_PATH
$STREAMER_PATH/mjpg_streamer -i"input_raspicam.so -fps 15 vf" -o"output_http.so -p 8091 -w $STREAMER_PATH/www')

```
우리는 15프레임을 이용하고, 네트워크 포트번호는 8091을 이용할 것입니다.

## 스트리밍 하기

먼저 스트리밍하기 앞서, 무선랜을 사용하는 ip를 확인해야합니다.
``` ipconfig ```를 이용하여 본인의 ip를 확인합니다.

그리고 본인의 ip주소가 예를 들어 172.129.3.15라고 하면, 브라우저에 172.129.3.15:8091을 입력합니다.

8091은 이전에 nano에서 편집했던 포트번호가 기준입니다.

#
(브라우저 주소 이미지)
#

모델에 적용을 하기위해 웹페이지 없이 영상만 출력하기 위해 172.129.3.15:8091/?action=snapshot를 입력합니다.

즉 아이피 주소 뒤에 ?action=snapshot 만 추가하면 됩니다.

#
(모델 이미지 및 gpu 구동 이미지)
#

이렇게 mjpg-streamer를 통해 영상스트리밍을 받아서 YOLO, CNN모델에 적용을 시켜보았습니다.
해당 스트리밍 방법은 동일 네트워크 환경에서 구동이 가능하며, 외부 네트워크에서는 이 방법이 아닌 AWS를 활용한 방법으로 구동하였습니다.

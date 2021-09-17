# 라즈베리파이를 이용하여 AWS와 연동하기

이번에는 로컬환경이 아닌 AWS(아마존 웹 서비스)의 Kinesis video stream을 이용한 스트리밍하는 방법입니다.

먼저 AWS의 계정을 생성한 다음 검색을 통해 Kinesis video stream 서비스에 들어갑니다.

![캡처1](https://user-images.githubusercontent.com/77596373/133746135-399d991e-2b01-4dd6-8a21-1e42086b7878.PNG)

그리고 '비디오 스트림 생성'을 눌러줍니다.

![캡처2](https://user-images.githubusercontent.com/77596373/133746317-2d558bf1-ab4c-4186-9728-421be3aa2d4b.PNG)

다음과 같이 본인이 원하는 스트림 이름을 영문으로 지정해줍니다. 

스트리밍 생성이 완료되었다면, 이에 접속할 수 있는 엑세스 키를 발급받아야 합니다.

검색을 통해 IAM서비스에 접속합니다.

![캡처3](https://user-images.githubusercontent.com/77596373/133746497-d96a2b86-aea0-451a-8092-6662f532eb6e.PNG)

IAM대시보드에 접속하면 빠른링크란의 '내 보안 자격 증명'을 클릭합니다.

![캡처4](https://user-images.githubusercontent.com/77596373/133746611-a12afa41-efe7-4b6a-9e25-26fc5ab33987.PNG)

그리고 '엑세스 키 만들기'를 클릭하면 

![캡처6](https://user-images.githubusercontent.com/77596373/133746822-6b9644ff-bfec-49e1-a420-f153082241fe.PNG)

**반드시 ".csv파일 다운로드"를 클릭하세요.**
이 창에서 엑세스키ID와 비밀 엑세스키(암호)를 확인할 수 있습니다.

이 작업은 Kinesis Vdieo Stream 서비스를 이용할 때 사용됩니다.

## 라즈베리파이에서 KVS(Kinesis Video Stream)에 접속하여 스트리밍 하기

라즈베리파이 터미널에 들어갑니다.

우리는 C++생산자 라이브러리를 사용합니다.

라즈비안 종속성에 필요한 빌드인 git, cmake, libtool, libtool-bin, automake, bison, g++, curl, pkg-config, flex, openjdk-8-jdk의 라이브러리들을 설치합니다.

Git 설치: ```sudo apt-get install git```

CMake 설치: ```sudo apt-get install cmake```

Libtool 설치: ```sudo apt-get install libtool```

libtool-bin 설치: ```sudo apt-get install libtool-bin```

GNU Automake 설치: ```sudo apt-get install automake```

GNU Bison 설치: ```sudo apt-get install bison```

G++ 설치: ```sudo apt-get install g++```

curl 설치: ```sudo apt-get install curl```

pkg-config 설치: ```sudo apt-get install pkg-config```

Flex 설치: ```sudo apt-get install flex```

OpenJDK 설치: ```sudo apt-get install openjdk-8-jdk```

환경변수 설정은 다음과 같이 할 수 있습니다.

터미널에서 ``` sudo nano ~/.bashrc ``` 입력 후 마지막에 환경변수를 설정합니다.

```
export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))
export PATH=$PATH:$JAVA_HOME/bin
```

이 환경변수들은 코드의 맨 마지막에 추가해주시면 됩니다. 추가한 후에 새로운 터미널 창을 실행시켜 ``` source ~/.bashrc ```로 적용을 합니다.

그리고 환경변수 설정 확인을 위해 터미널 창에서 ''' echo $JAVA_HOME '''로 잘 적용되었는지 확인할 수 있습니다.

해당 라이브러리 설치 및 작업이 완료되었다면, 홈 디렉토리에서 터미널을 실행시킵니다.

### 빌드하기
깃 클론(git clone)을 이용하여 AWS kinesis Video Streams를 다운로드 합니다.

``` git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git ```

그리고 디렉토리를 생성합니다. 

``` mkdir -p amazon-kinesis-video-streams-producer-sdk-cpp/build ```

그리고 다음 디렉토리로 이동해서 작업을 합니다.

``` cd amazon-kinesis-video-streams-producer-sdk-cpp/build ```

해당위치의 디렉토리에서 cmake로 GStreamer와 JNI를 빌드 해줍니다.

``` cmake .. -DBUILD_GSTREAMER_PLUGIN=ON -DBUILD_JNI=TRUE ```

그리고 라즈비안에서 사용할 라이브러리를 설치합니다.

``` sudo apt-get install libssl-dev libcurl4-openssl-dev liblog4cplus-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools ```

컴파일을 하기 위해 make를 실행합니다.

''' make '''

### GStremer 플러그인(Kvssink) 하기

이제 Gstreamer 플러그인은 build 디렉토리에 위치합니다.

환경변수 설정을 위해서 build의 상위 디렉토리로 이동합니다.

``` cd .. ```

그리고 이전 java 환경변수 설정과 같이 Gstreamer 플러그인 환경변수를 설정합니다.

환경변수 설정은 새로운 터미널 창을 열어서  ``` sudo nano ~/.bashrc ```을 입력후 마지막에 추가해 주시기 바랍니다.

그리고 

**본인이 설치한 위치를 다시한번 확인해 주시기 바랍니다. 사용자 이름 지정에 따라 /home/pi 가 다를 수 있으니 확인하시기 바랍니다.**

```
export GST_PLUGIN_PATH=/home/pi/amazon-kinesis-video-streams-producer-sdk-cpp/build
export LD_LIBRARY_PATH=/home/pi/amazon-kinesis-video-streams-producer-sdk-cpp/open-source/local/lib
```

저장 후 ``` source ~/.bashrc ```로 적용시킨 후 이제 터미널에서 다음과 같이 입력합니다. 

``` gst-inspect-1.0 kvssink  ```

정상적으로 실행이 되면 플러그인의 정보들이 실행됩니다.

혹시 No such element or plugin 'kvssink'가 나올경우 환경변수 설정에 오타 및 빌드 경로 확인이나,

터미널에 두 개를 차례대로 입력한 후에 ``` gst-inspect-1.0 kvssink  ```를 실행해주시기 바랍니다.  
```
export GST_PLUGIN_PATH=/home/pi/amazon-kinesis-video-streams-producer-sdk-cpp/build
```

```
export LD_LIBRARY_PATH=/home/pi/amazon-kinesis-video-streams-producer-sdk-cpp/open-source/local/lib
```

정상적으로 kvssink가 실행되었으면, 다음과 같은 코드를 터미널에 입력합니다.

```
gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=15/1 ! x264enc  bframes=0 key-int-max=45 bitrate=500 ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name="생성한 스트림 이름(NAME)" storage-size=512 access-key="IAM에서 발급받은 엑세스 키(KEY)" secret-key="IAM에세 발급받은 비밀번호 " aws-region="ap-northeast-2(서울 기준)"
```
여기서 제가 한글로 한 부분인 스트림 이름과 IAM에서 발급받은 엑세스키, 비밀번호, 그리고 리젼(위치)입니다. "" 따옴표 안에 그대로 키 및 값들을 넣으시면 됩니다.

리젼은 다음과 같은 화면에서 찾을 수 있습니다.

![K-002](https://user-images.githubusercontent.com/77596373/133757200-6461691f-41f0-4dca-a44b-fa88abffc2cc.png)

옆에 영어로된 리젼이 나와있으므로, 본인이 미국 버지니아, 오하이오, 서울 등 확인해서 적절하게 값을 입력해 주시면됩니다.

그리고 대시보드에서 스트리밍을 이용할 수 있습니다. 다만, 영상이 스트리밍 되는데 한 5분~10분정도 소요됩니다. 저는 서버 타임스태프로 확인했습니다. 그리고 클라우드를 거치다 보니 어느정도 딜레이가 발생됩니다.

![캡처8](https://user-images.githubusercontent.com/77596373/133757454-b9e7e2ec-f4a9-4635-ba80-57c83792151d.PNG)


만일 계속해서 오류가 발생하면서 스트리밍이 되지 않는다면 다음과 같은 웹사이트에서 확인할 수 있습니다. 
https://aws-samples.github.io/amazon-kinesis-video-streams-media-viewer/

여기서는 Region, AWS Access Key, AWS Secret Key, Stream name에 값을 넣어주시고 'Start playback'을 실행시키면 됩니다.


![8](https://user-images.githubusercontent.com/77596373/133757671-cc22b691-4ca3-41e2-bf5d-d1bc5553eca6.PNG)

AWS의 Kinesis Video Stream을 이용하여 네트워크 주소의 제약 없이 실시간으로 스트리밍을 할 수 있으며,

강아지 입마개 탐지인 CNN, YOLO모델에 적용시킬 수 있습니다.

















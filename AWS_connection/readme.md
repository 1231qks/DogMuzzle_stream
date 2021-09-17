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

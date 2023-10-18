---
title: Windows의 Unix Like 기능
date: 2022-10-08 14:00:00 +0900
categories: [Windows, 기술소개]
tags: [Windows, WSL, Linux, Unix Like]
render_with_liquid: false
---

오늘은 Windows에서 개발할 때 유용한 도구들에 대해 소개 해 드리려고 합니다.

저는 여태 리눅스 계열의 OS에서만 작업해 왔는데요.
이유는 Windows에서 여러가지 작업하기가 무척 어렵고 번거로웠기 때문입니다.

예를 들면 terminal 앱도 별로 이쁘지 않고, linux에서 할 수 있었는데 Windows에서 하지 못하는 것들도 많았구요.

그런데, 요즘 점점 Windows도 여러가지 기능들이 추가되면서 unix-like화 되어가는 것 같습니다.
저도 요즘 종종 Widnwos 위에서 바로 개발 하는 경우도 늘고 있구요.

각설하고, 소개를 해보도록 하겠습니다.

# WSL

> 아래에 설명 할 내용들은 Microsoft 공식 의견과 다를 수 있습니다.<br>[Microsoft 공식 설명](https://learn.microsoft.com/ko-kr/windows/wsl/)을 이해하시는 것을 추천 드립니다.
{: .prompt-warning }

[WSL](https://learn.microsoft.com/en-us/windows/wsl/about)은 Windows Subsystem for Linux의 약자입니다.

Microsoft가 2015년 개발한 Unix-Like OS와의 호환성을 위해 개발한 일종의 가상화 계층이라고 할 수 있습니다.

WSL은 버전 1과 2 둘로 나뉘어져있고, 특수한 상황이 아닌 경우 2를 권장합니다.

기본적인 구조는 아래와 같습니다.

## WSL1

WSL1은 아래와 같이 기본적으로 Windows NT Kernel 위에서 동작합니다.
![WSL1의 구조](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/WSL1%EA%B5%AC%EC%A1%B0.webp)
_WSL1의 구조_

POSIX 규격에 맞는 API를 Windows NT Kernel API로 번역하는 구조로 개발 되었다고 이해하면 될것 같습니다.

그 API를 이용하여 User Land에 Ubuntu, CentOS, Redhat 등의 Linux Kernel 기반 운영체제를 얹은 형태가 되는 것 이지요. 

다만 이렇게 개발하다보니 큰 문제가 있었습니다.

### WSL1의 문제점

POSIX 규격을 최대한 준수하고자 노력한다고 해도 이미 이질성 있는 운영 체제에 POSIX 규격에 맞추어 API를 개발하는 것은 매우 어려웠습니다.

간단히 예를 들면, foo.txt를 특정 프로그램이 사용 중일 경우 Windows NT Kernel 에서는 삭제나 변경이 불가능 합니다. 그러나 Linux Kernel에서는 가능합니다.

이런 이질적인 부분들 때문에 완벽한 호환이란 불가능하고 Side Effect도 많이 생길 수 밖에 없었지요.

그래서 microsoft에서는 Windows NT Kernel과 독립된 Linux Kernel을 올리기로 하게 됩니다.

## WSL2

WSL2의 구조는 아래와 같습니다.

![WSL2의 구조](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/WSL2%EA%B5%AC%EC%A1%B0.webp)
_WSL2의 구조_

위와 같이 Hipervisor 위에서 Windows NT Kernel과 [WSL용 Linux Kernel](https://github.com/microsoft/WSL2-Linux-Kernel)은 서로 완전히 분리된 구조를 갖고 있습니다.

다만 Hipervisor 위에서 동작하기 때문에 OS 전체적인 성능은 WSL1에 비해 약간 느립니다.

![WSL 기능비교](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/WSL1%EA%B3%BC2%EC%9D%98%EC%B0%A8%EC%9D%B4%EC%A0%90.png)
_WSL 기능비교_

### WSL1을 선택해야 하는 경우

Microsoft에서는 WSL2를 사용 하는 것을 권장합니다.
대부분의 상황에서는 WSL1보다 WSL2가 더 나은 선택이기도 합니다.

다만 일부 예외적인 상황에서는 WSL1이 더욱 적합하기도 합니다.

WSL1이 더욱 적합한 경우는 아래와 같습니다.

1. 프로젝트 파일을 Windows 파일 시스템에 저장해야 하는 경우
  WSL Linux 배포를 사용하여 Windows 파일 시스템의 프로젝트 파일에 액세스해야 하는 경우 WSL2는 부적합합니다.

  앞서 설명드렸다 시피 Windows 파일 시스템과 Linux 파일 시스템은 완전히 분리되어있기 때문에 WSL2에서는 파일 접근도 느리고, [inotify](https://ko.wikipedia.org/wiki/Inotify)와 같은 기능도 사용할 수 없습니다.
  
  Windows 애플리케이션을 사용하여 Linux 파일에 액세스하는 경우에도 WSL1이 더 나은 선택이 될 수 있습니다.

2. 프로젝트가 직렬 포트 또는 USB 디바이스에 액세스해야 하는 경우
  현재 WSL2는 기본적으로 USB 디바이스에 연결할 수 없습니다.
  
  WSL2가 개발되어감에 따라 GPU 지원 등 하드웨어에 접근할 수 있는 기능이 구현되고 있고, [USBIPD-WIN](https://learn.microsoft.com/ko-kr/windows/wsl/connect-usb)를 통해 USB 접근을 할 수 있게 되었습니다만, 아직 모든 하드웨어를 native하게 지원하지는 않습니다.

3. (아직까지는) 버그가 매우 많습니다.
  [WSL2 repository](https://github.com/microsoft/WSL/issues)를 보면 아직 매우 많은 이슈가 있다는 점을 알 수 있습니다.
  
  대표적으로 [메모리 요구사항이 엄격한](https://github.com/microsoft/WSL/issues/4166)문제가 있습니다.
  
  때문에 WSL1에서 stable된 프로젝트를 WSL2로 이주 하는 것은 아직까지는 고민을 많이 해 봐야 합니다.

4. 일반적인 가상환경과 같이 네트워크 설정을 해 주어야 할 수도 있습니다.

  앞서 설명드린것과 같이 WSL2는 가상 환경 위에서 동작하기 때문에 일반적인 가상환경에서 설정해 주어야 하는 것들(ex. 네트워크 설정 등)을 해주어야 할 수 있습니다.

  때문에 가상머신 설정의 번거로움 때문에 WSL을 사용하려고 할 경우 WSL2는 좋은 대안이 되지 못할 수 있습니다.

# 사용해보기

## WSL 설치

우선 설치 할 윈도우의 버전을 확인 해야 합니다. 

아래 명령어로 확인할 수 있습니다.

```console
> winver
```
![winver 실행 결과](/assets/img//2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/winver%20%EC%8B%A4%ED%96%89%20%ED%99%94%EB%A9%B4.png)
_winver 실행 결과_

### Windows 10 버전 2004 이상(빌드 19041 이상) 또는 Windows 11

축하합니다! 아래 명령어 한 줄이면 바로 설치가 됩니다!
```console
> wsl --install
```

![WSL 설치 화면](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/wsl%EC%84%A4%EC%B9%98%ED%99%94%EB%A9%B4.png)
_WSL2 설치 화면_

### Windows 10 버전 2004 이전 버전 

안타깝게도 설치 과정이 다소 번거롭습니다.

가능하다면, 윈도우 버전을 업데이트 하는 것을 권장 드리나, 업데이트가 불가할 경우 아래와 같이 설치 할 수 있습니다.

설치에 앞서 아래와 같은 최소 요구사항을 충족하는지 확인합니다.
- x64 시스템의 경우: 버전 1903 이상, 빌드 18362 이상
- ARM64 시스템의 경우: 버전 2004 이상, 빌드 19041 이상

관리자 버전으로 연 PowerShell에서 아래 명령어를 입력하여 WSL사용설정을 합니다.

```console
> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

WSL2를 사용하려는 경우 아래와 같이 명령어를 입력합니다.

```console
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart #가상화 기능 사용 설정
wsl --set-default-version 2 # WSL2 사용 설정
```

### WSL1 에서 WSL2로 이주

```console
> wsl --set-version <distro name> 2 # <distro name>은 변경할 배포판의 이름을 입력합니다.
> wsl --set-version Ubuntu-20.04 2 # ex
```

### 원하는 배포판 설치

[Microsoft Store](https://aka.ms/wslstore)를 열고 즐겨 찾는 Linux 배포를 선택합니다.

![Microsoft Store](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/MSStore%ED%99%94%EB%A9%B4.png)
_Microsoft Store 화면_

실행 시키면 아래와 같이 설치가 진행됩니다.

![Ubuntu 설치 화면](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/Ubuntu%20on%20WSL%20%EC%84%A4%EC%B9%98%20%ED%99%94%EB%A9%B4.png)
_Ubuntu 설치 화면_
![Ubuntu 설치 후 실행 화면](/assets/img/2022-10-08-Windows%EC%9D%98-Unix-Like-%EA%B8%B0%EB%8A%A5/Ubuntu%20on%20WSL%20%EC%84%A4%EC%B9%98%20%ED%99%94%EB%A9%B42.png)
_Ubuntu 설치 후 실행 화면_


# TODOs

이렇게 WSL을 설치 해 보았습니다.

다음 시간에는 WSL로 프로젝트를 진행할 때 사용하기 좋은 툴에 대해서 소개하는 시간을 가져보도록 하겠습니다.

감사합니다.
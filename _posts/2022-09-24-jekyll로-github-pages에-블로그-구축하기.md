---
title: Jekyll로 github pages에 블로그 구축하기
date: 2022-09-24 12:00:00 +0900
categories: [Tutorial, Web]
tags: [github pages, jekyll, chirpy, blog]
render_with_liquid: false
---

오늘은 제가 블로그를 구축했던 방법에 대해 이야기 해보려고 합니다.

**블로그 spec.**
1. [Github Pages](https://pages.github.com/)
2. [Jekyll](https://jekyllrb.com/) 루비 기반의 정적 웹사이트 생성기
3. [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) jkyll 테마
4. [Github Actions](https://docs.github.com/en/actions) 기반의 자동 배포
5. 웹페이지 생성을 위한 기본적인 HTML, Javascript, CSS 문법
6. [Markdown](https://daringfireball.net/projects/markdown/)

뭔가 어려운 말들이 엄청 적혀있는거 같지요?🤔

하지만 하나하나 모두 알고 있지 않아도 괜찮습니다. (깊게 뜯어 고치려면 필요하겠지만요.)

이번 기사에서는 기본적인 블로그 만들기에 대해 진행 해 보겠습니다.

# 준비물
## 1. Linux
linux 는 선택사항 입니다만, 제 경험상 Windows에서 Jekyll 빌드하고 Github repository에 pull하면 side effect가 좀 많았습니다.

저는 그래서 [Ubuntu](https://ubuntu.com/download)나 [WSL](https://learn.microsoft.com/ko-kr/windows/wsl/install)위에서 진행하시는 것을 권장합니다만, 쓰고싶지 않으셔도 괜찮습니다.

우리에겐 만능 Google님이 있으니까요!

## 2. Ruby
Jekyll은 Ruby 위에서 동작하는 웹사이트 생성기이기 때문에 Ruby 설치가 필요합니다.

[jekyll 공식 홈페이지](https://jekyllrb.com/docs/installation/)의 설치 방법을 따라 진행해 봅시다.

### UBUNTU, Debian

아래와 같이 Ruby 및 필수 패키지를 설치합니다.
```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

gem 설치 경로를 설정합니다.
```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

jekyll과 bundler를 설치합니다.

bundler는 npm, pip 등과 같은 일종의 패키지와 프로젝트를 관리해주는 툴 이라고 생각하시면 됩니다.
```bash
gem install jekyll bundler
```

### Windows 
[ruby installer](https://rubyinstaller.org/)를 이용해 설치하시면 됩니다.

설치 하신 후에 WT, PowerShell, CMD 등의 명령창에서 jekyll과 bundler를 설치합니다.
```bash
gem install jekyll bundler
```

### macOS
[Homebrew](https://brew.sh/index_ko)를 설치합니다. 있다면 건너뛰셔도 됩니다.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

아래와 같이 ruby를 설치합니다.
```bash
brew install chruby ruby-install
ruby-install ruby

echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.2" >> ~/.zshrc # run 'chruby' to see actual version
```

아래와 같이 jekyll을 설치합니다.
```bash
gem install jekyll
```

## 3. 테마

우리 블로그의 테마를 선택합니다.
[jekyllthemes](https://jekyllthemes.org/)와 같은 테마를 모아놓은 사이트에서 고르시거나 구글링하여 고르시면 됩니다.

물론 Jekyll로 직접 페이지를 구현할 수도 있습니다!

이번 기사에서는 이 블로그의 테마인 Chirpy 테마를 이용해 블로그를 만들어보겠습니다.

## 4. Github 계정

Github 사용 방법은 여기서 따로 이야기하지는 않겠습니다.

# Chirpy 테마 적용하기

테마를 적용하는 방법은 여러가지가 있습니다.
1. 소스를 다운로드 후 repository 생성
2. Github에서 지원하는 Template 사용 기능을 통해 생성
3. Demo 페이지 fork

이 중에서 2번은 아래와 같이 Public template에 한해서만 사용할 수 있다는 단점이 있습니다.
![chirpy starter](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/chirpy%20starter.PNG)
_chirpy starter repository_

제 경험상 3번이 side effect가 발생할 가능성이 제일 낮기 때문에 3번으로 진행하겠습니다.

## repository fork
[chirpy demo repository](https://github.com/cotes2020/jekyll-theme-chirpy/)의 fork 버튼을 눌러 내 repository로 fork합니다.
![fork](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/fork.PNG)
_jekyll-theme-chirpy repository fork 버튼_

이때 repository 이름은 ```<github 아이디>.github.io```의 형태로 만들어야 합니다.

![내 repository](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/%EB%82%B4%20repository.PNG)
_Fork 한 후 생성된 repository_

## repository clone
이제 생성한 repository를 수정하기 위해 local에 clone합니다.

code 버튼을 누른 후에 마음에 드는 방식으로 clone 해주면 됩니다.

초심자의 경우 제 추천은 Open with GitHub Desktop을 이용하여 clone하는 방법입니다.

다른 git client를 이용하려면 보안 설정 같은 것들이 필요할 수 있거든요 😅

아무튼 이제 local에서 블로그를 한번 실행해봅시다!

## 테스트

아래 명령어를 입력해서 실행 해 봅시다
```bash
bundle
jekyll serve
```

아래와 같이 출력되면 정상적으로 실행 된것입니다.
```bash
Configuration file: C:/Users/kimuj/OneDrive/Documents/GitHub/KimEJ.github.io/_config.yml
 Theme Config file: C:/Users/kimuj/OneDrive/Documents/GitHub/KimEJ.github.io/_config.yml
            Source: C:/Users/kimuj/OneDrive/Documents/GitHub/KimEJ.github.io
       Destination: C:/Users/kimuj/OneDrive/Documents/GitHub/KimEJ.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 1.577 seconds.
 Auto-regeneration: enabled for 'C:/Users/kimuj/OneDrive/Documents/GitHub/KimEJ.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

> ```command not found``` 같은 메시지가 출력된다면 jekyll설치가 제대로 안된 것 입니다. [jekyll 공식 홈페이지](https://jekyllrb.com/docs/installation/)를 참고하여 다시 설치 해 주세요
{: .prompt-tip }


이제 ```http://127.0.0.1:4000/```로 한번 접속 해 보겠습니다.

![test serve 화면](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/test%20serve.PNG)
_test serve 화면_

위와 같이 화면이 나온다면 정상적으로 만든 것 입니다!

## 템플릿 초기화

아래와 같은 방법으로 템플릿을 초기화합니다.

### Linux & macOS
```bash
tools/init.sh
[INFO] Initialization successful!
```
위와 같이 출력된다면 정상적으로 초기화 된 것입니다!

### windows
windows에서는 자동으로 초기화 시켜 줄 방법이 없습니다...😅

아래 파일들을 직접 삭제하여 초기화를 진행해 줍니다.
- .travis.yml
- _posts 폴더 하위의 파일들
- docs 폴더

## Git Push
이제 우리 템플릿을 github에 올려봅시다.

![actions 탭](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/actions.PNG)
_actions 탭의 자동 배포 장면_

github에 올리고 repository의 actions 탭을 보면 위와 같이 뭔가 2개가 생겼습니다.

잠시 후에 보면 아래와 같이 성공했다는 표시가 뜨게 됩니다.

![actions 성공](/assets/img/2022-09-24-jekyll%EB%A1%9C-github-pages%EC%97%90-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/actions-success.PNG)
_actions 성공 장면_

```<github 아이디>.github.io``` 페이지로 이동해보면 우리가 올린 페이지가 뜨게 됩니다!

# TODOs

우리가 올린 블로그는 제 블로그랑 사뭇 다를겁니다.

아직 기사도 하나도 없구요.

다음 기사에서는 블로그 커스텀과 포스팅 방법에 대해 이야기 해 보려고 합니다!

다음 기사에서 만나요!
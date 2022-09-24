---
title: Jekyll로 github pages에 블로그 구축하기
date: 2022-09-23 10:10:00 +0900
categories: [Tutorial]
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

뭔가 그럴듯 하게 적었지만 일일히 전부 알 필요는 없습니다. (깊게 뜯어 고치려면 필요하겠지만요.)
이번 포스팅에서는 기본적인 블로그 만들기와 포스팅 하는 방법에 대해 진행 해 보겠습니다.

# 준비물
## 1. Linux
linux 는 선택사항 입니다만, 제 경험상 Windows에서 Jekyll 빌드하고 Github repository에 pull하면 side effect가 좀 많았습니다.
저는 그래서 [Ubuntu](https://ubuntu.com/download)나 [WSL](https://learn.microsoft.com/ko-kr/windows/wsl/install)위에서 진행하시는 것을 권장합니다만, 쓰고싶지 않으셔도 괜찮습니다. 우리에겐 만능 Google님이 있으니까요!

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

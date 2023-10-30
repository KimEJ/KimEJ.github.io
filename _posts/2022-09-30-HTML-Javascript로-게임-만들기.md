---
title: HTML Javascript로 게임 만들기
date: 2022-09-30 21:48:00 +0900
categories: [Tutorial, Web]
tags: [Phaser, Javascript, HTML, Game]
render_with_liquid: false
---

오늘은 HTML과 Javascript로 게임을 만들어보려고 합니다.

> 이번 기사에서 진행하는 튜토리얼의 최종 코드는 [Github Repository](https://github.com/KimEJ/phaser-demo)에 올려두었습니다.
{: .prompt-info }

> 또한 해당 코드는 [Github Pages](https://kimej.github.io/phaser-demo/)로 배포 되고 있으므로 시작 전에 한번 구경 해 보셔도 좋습니다!
{: .prompt-info }

여러분들은 HTML/Javascript로 게임을 만든다면 어떻게 생각하시나요?

"에이~ 웹 페이지 만드는 기술로 게임을 어떻게 만들어요~" 하실 분들도 있겠지만 잘 생각해보면 이미 Javascript로 된 게임을 경험 해 보셨을 겁니다.

[![추억의 게임 agar.io](/assets/img/2022-09-30-HTML-Javascript로-게임-만들기/agar.io.png)](https://agar.io/)
_추억의 게임 agar.io_

여러분들도 어렸을 때 한번쯤 웹 페이지에서 게임 해 본적 있지 않나요?

오늘은 그런 웹 게임을 HTML과 Javascript를 이용해서 만들어보려고 합니다.

# 준비물
## 1. Python
chrome을 비롯한 대부분의 웹브라우저에서는 [보안 상의 이유로](https://blog.chromium.org/2008/12/security-in-depth-local-web-pages.html) 로컬 웹 페이지에서 파일 접근이 불가능합니다.

따라서 서버를 통해 배포를 해야 합니다.

이번 기사에서는 Python을 이용할 예정이지만, 굳이 Python이 아니어도 괜찮습니다!

웹 페이지 배포만 가능하다면 Node.JS, Ruby 등 여러분이 마음에 드는 방법으로 배포하면 됩니다!

## 2. [Phaser3](https://phaser.io/)

![Phaser Logo](/assets/img/2022-09-30-HTML-Javascript로-게임-만들기/phaser%20Logo.png)

Javascript기반의 게임엔진은 많이 있습니다.

오늘은 Phaser라는 프레임워크로 진행을 해보려고 합니다.

자세한 설명은 차차 이야기 하도록 해요.

# 왜 Phaser3를 쓰나요?

사실 HTML Javascript로 게임을 만들 수 있는 툴은 매우 많습니다.

[![게임엔진 순위](/assets/img/2022-09-30-HTML-Javascript로-게임-만들기/html%20game%20engine%20rank.png)](https://html5gameengine.com/)
_HTML 기반의 게임 엔진 순위_

그 중에서 Phaser3의 장단점은 아래와 같습니다.

## 장점

### 1. 많은 레퍼런스와 가이드

제일 큰 요인은 역시 레퍼런스 등의 예제와 가이드가 많았다는 점 입니다.

이번 기사에서는 깊은 내용은 다루지 않을 예정이기 때문에 쉽게 찾아보기 좋은 엔진으로 진행 해 보려고 합니다.

개발할 때 찾기도 쉽구요.

### 2. 간단한 구조

Phaser는 게임 개발에 특화되어있기 때문에 게임 개발에 필요한 API는 거의 다 있습니다.

반대로 말하면 다른 범용 물리엔진에 비해 부족할 수는 있지만 시뮬레이션 수준이 아닌 이상 크게 제약이 있지는 않습니다.


![Phaser 기반 게임들](/assets/img/2022-09-30-HTML-Javascript로-게임-만들기/phaser%20%EA%B8%B0%EB%B0%98%20%EA%B2%8C%EC%9E%84.png)
_Phaser 기반 게임들_

## 단점

### 1. 많이 무겁다.

Phaser의 가장 큰 단점은 다른 엔진들에 비해 많이 무겁다는 점 입니다...

그래서 조금 본격적인 게임을 만들기에는 조금 무리가 있는것도 사실입니다.

하지만 1인 개발 수준에서는 충분히 강력하다고 생각합니다. 사실 웹이라는 플랫폼에서의 한계도 있기도 하구요.

### 2. 2D 게임만 만들 수 있음

Pixi.js 나 Unity Tiny에 비해 3D가 불가능 하다는 단점이 있습니다.

물론 편법을 써서 2.5D의 형태로는 가능합니다.

다만 상용 3D 게임을 만드려면 다른 엔진을 써야 합니다...

# 게임을 만들어보자!

> 아래의 설명은 <https://github.com/KimEJ/phaser-demo> 리포지토리를 기반으로 진행됩니다.
{: .prompt-info }

이제부터 본격적으로 게임을 만들어 볼 시간입니다.

## assets

코드 작성에 앞서 캐릭터나 배경과 같은 것들을 제작하거나 구해야 합니다.

저 같은 경우엔 그림 실력은 하나도 없는 관계로 아래의 에셋 배포 사이트의 무료 에셋을 주로 이용합니다.

- <https://resourcebank.or.kr/>
- <https://itch.io/>

더 많은 유/무료 에셋 배포 사이트들이 많으니 마음에 드시는 에셋을 준비하시면 됩니다!

> 이번 프로젝트에서 이용한 에셋의 주소는 다음과 같습니다 <br><https://brullov.itch.io/oak-woods><br><https://brullov.itch.io/generic-char-asset>
{: .prompt-info }

## lib 다운로드

여러가지 방법이 있습니다만, 순수 HTML Javascript로 만들 것이기 때문에 아래 두가지 방법만 소개 하려고 합니다.

### CDN에서 가져오기

아래와 같이 간단하게 CDN에서 가져올 수 있습니다.

```HTML
<script src="//cdn.jsdelivr.net/npm/phaser@3.11.0/dist/phaser.js"></script>
```

### 라이브러리 소스를 포함하여 배포하기

아니면 [다운로드 페이지](https://phaser.io/download/stable)에서 javascript 소스를 직접 다운로드 받아서 아래와 같이 직접 배포할 수도 있습니다.

```HTML
<!-- 배포 디렉터리의 'js/' 밑에 다운받은 phaser.min.js를 옮긴 후 불러온다. -->
<script src="./js/phaser.min.js"></script>
```

## 기본 구조

코드의 기본 구조는 아래와 같습니다.

```html
<!doctype html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8" />
    <title>phaser demo</title>
    
    <script src="./js/phaser.min.js"></script>
    <!-- 
      아래와 같이 CDN으로 받아서 사용해도 괜찮습니다!
      <script src="//cdn.jsdelivr.net/npm/phaser@3.11.0/dist/phaser.js"></script>
    -->
    <style type="text/css">
        body {
            margin: 0;
        }
    </style>
</head>
<body>
  <script type="text/javascript">
    // Phaser의 옵션을 설정하는 Object
    var config = {
      // type 옵션은 게임 렌더링에 대한 설정입니다.
      // AUTO로 설정할 경우 브라우저가 지원하는 방법으로 렌더링 됩니다.
      type: Phaser.AUTO,
      width: 800,
      height: 600,
      // scene은 게임상의 공간을 의미합니다.
      // scene의 생명 주기에 따라 실행할 함수를 지정하는 옵션입니다.
      scene: {
        preload: preload,
        create: create,
        update: update
      }
    };

    var game = new Phaser.Game(config);

    function preload () {...}
    function create () {...}
    function update () {...}
  </script>
</body>
</html>
```

## 장면(Scene)의 구성

phaser는 기본적으로 scene단위로 동작을 하게 됩니다.

아래는 scene의 생명주기마다 실행되는 함수에 대한 설명이에요.

> 아래에 설명된 내용이 전부는 아닙니다. <br>자세한 내용은 [phaser 공식 문서](https://photonstorm.github.io/phaser3-docs/)를 참고 해 주세요
{: .prompt-warning }

### **preload**

preaload 함수는 scene이 생성되기 전에 실행됩니다.

이미지, 애니메이션, 오디오 등을 로드 하는 코드가 들어가요.

#### 이미지 로드

이미지 로드는 아래와 같이 할 수 있어요.

```javascript
this.load.image('KEY', '이미지/위치.png');
```

첫 번째 인자에는 앞으로 해당 이미지를 사용할 때 쓸 이름이 들어가요.
두 번째 인자에는 앞으로 해당 이미지의 위치가 들어가요.

#### Sprite Sheet 로드

Sprite Sheet란 각 장면이 연결되어있는 이미지를 말해요.

아래처럼 말이죠.

![spritesheet](/assets/img/2022-09-30-HTML-Javascript로-게임-만들기/char_blue_2.png)
_Sprite Sheet_

이런 sprite sheet는 아래와 같이 로드할 수 있어요.

```javascript
this.load.spritesheet('KEY', '이미지/위치.png', { frameWidth: 00, frameHeight: 00 });
```

첫 번째 인자와 두 번째 인자는 ```image()```와 동일해요.

세 번째 인자에는 한 프레임의 사이즈를 적어줘요.


#### audio 로드

오디오는 아래와 같이 로드 할 수 있어요.
```javascript
this.load.audio('KEY', '오디오/위치.mp3');
```

각 인자는 ```image()```와 동일해요.

여러가지 브라우저가 각자 지원하는 오디오 포맷을 제공하고 싶을 경우 아래와 같이 쓸 수도 있어요.

```javascript
this.load.audio('KEY', ['오디오/위치.mp3', '오디오/위치.ogg']);
```
위와 같이 작성할 경우 mp3 포맷을 지원하지 않는 브라우저의 경우 ogg 로 음악을 재생 할 수 있어요.

### **created**

created 함수는 scene 생성 시 실행됩니다.

이미지나 오디오등을 뿌려주거나 이벤트에 대한 handler를 생성하는 코드를 넣을 수 있습니다.

#### 이미지 출력

이미지를 아래와 같이 뿌려줄 수 있어요.

```javascript
this.add.image(X, Y, 'KEY')
```

첫 번째, 두 번째 인자에는 해당 이미지의 위치가 들어가요.
세 번째 인자에는 뿌려줄 이미지를 load 할 때 지정한 이름이 들어가요.

#### animation 생성

각 동작에 대한 애니메이션을 아래와 같이생성할 수 있어요.

```javascript
this.anims.create({
  key: 'KEY',
  frames: this.anims.generateFrameNumbers('spritesheet key', { start: 0, end: 5 }),
  frameRate: 10,
  repeat: -1
});
```

object의 각 항목은 아래와 같습니다.
- key: 해당 애니메이션의 이름이 들어갑니다.
- frames: 애니메이션,  ```this.anims.generateFrameNumbers('spritesheet 이름', { start: [시작 frame], end: [종료 frame] })```의 형태로 spritesheet를 불러올 수 있어요.
- frameRate: 초당 출력할 프레임 갯수
- repeat: 애니메이션 반복 횟수, -1일 경우 무한반복 합니다.

#### audio 재생

오디오는 아래와 같이 로드 할 수 있어요.
```javascript
this.add.audio('KEY').play()
```

#### 키 입력 이벤트 핸들링

아래와 같이 키 입력 이벤트를 핸들링 할 수 있습니다.

```javascript
this.input.keyboard.on('keydown-A', event => {...});
```
첫 번째 인자는 핸들링 할 이벤트 이름입니다.
두 번째 인자는 핸들링 함수입니다.

이벤트의 종류는 [이곳](https://photonstorm.github.io/phaser3-docs/Phaser.Input.Keyboard.Events.html)을 참고 해 주세요!

#### 충돌 핸들링

아래와 같이 충돌할 경우 두 객체를 분리 할 수 있습니다.

```javascript
this.physics.add.collider(a, b);
```

또한 아래와 같이 충돌 할 경우 함수를 실행 할 수도 있습니다.

```javascript
this.physics.add.collider(a, b, (a, b) => {...});
```

분리를 할 필요가 없을 경우 아래 함수를 사용할 수도 있습니다.
```javascript
this.physics.add.overlap(a, b, (a, b) = {...});
```

### **update**

update 함수는 scene이 갱신될 때 실행됩니다.

즉 매 프레임마다 실행되는 함수라고 할 수 있습니다.

상태를 polling 할 때 사용할 수 있습니다.

#### 키 입력 polling 

아래와 같이 키 입력이 있는 상태인지 확인할 수 있어요

```javascript
... // create 함수에서 아래와 같이 cursor 가 생성되어있어야 합니다.
cursors = this.input.keyboard.createCursorKeys();

... // update 함수
// 오른쪽 방향키를 누르고 있는 상태일 경우 true가 됩니다.
console.log(cursors.right.isDown)
```

```createCursorKeys()```함수는 방향키와 스페이스, 쉬프트 객체를 가지고 있는 cursorKeys형 object를 반환합니다.

#### 객체의 상태 확인

아래와 같이 상태를 확인할 수 있습니다.

```javascript
... // create 함수에서 아래와 같이 객체가 생성되어있고, 충돌 감지 설정이 되어있어야 합니다.
player = this.physics.add.sprite(100, 180, 'avatar')
this.physics.add.collider(player);

... // update 함수

// 아래와 같이 바닥에 붙어있는 상태인지 확인합니다.
console.log(player.body.onFloor())
// 또는 아래와 같이 객체의 아랫 부분이 다른 객체와 닿아있는지 확인 할 수 있습니다.
console.log(player.body.touching.down)

// 아래와 같이 이전 프레임과 현재 프레임의 위치 차이를 확인 할 수 있습니다.
console.log(player.body.deltaY())
console.log(player.body.deltaX())
// 또는 아래와 같이 속도를 구할 수도 있습니다.
console.log(player.body.velocity.y)
console.log(player.body.velocity.x)

// 아래와 같이 애니메이션이 플레이 중인지 확인할 수 있으며,
console.log(player.anims.isPlaying)
// 아래와 같이 재생중인 애니메이션의 이름도 확인할 수 있습니다.
console.log(player.anims.currentAnim.key)
```

#### 객체의 상태 변경

아래와 같이 상태를 변경할 수 있습니다.

```javascript
... // create 함수에서 아래와 같이 객체가 생성되어있어야 합니다.
player = this.physics.add.sprite(100, 180, 'avatar')

... // update 함수
// 아래와 같이 애니메이션을 재생시킬 수 있습니다.
player.anims.play('KEY', bool);
// 첫 번째 인자는 재생시킬 애니메이션의 이름입니다.
// 두 번째 인자는 이미 애니메이션이 재생중인 경우 재생 할 것인지를 설정하는 인자입니다.

// 아래와 같이 객체의 위치를 변경시킬 수도 있습니다.
player.setVelocityY(00);
player.setVelocityX(00);
// 또한 아래와 같이 방향을 바꿀 수도 있습니다.
player.flipX = true // 이미지를 뒤집은 상태
player.flipX = false // 이미지를 뒤집지 않은 상태
```

# 게임을 실행해 봅시다!

지금 만든 디렉토리 위에서 아래와 같이 Python 명령어로 http 서버를 열 수 있습니다.
```bash
$ python -m http.server
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```
이후 <localhost:8000>으로 접속하면 만들어본 게임을 플레이 할 수 있습니다! 

# TODOs

[데모](https://kimej.github.io/phaser-demo/)를 보시면 알겠지만, 실제 게임이라고 부르기 민망할 정도로 아무것도 없습니다...😅

앞으로 천천히 게임을 만들어보면서 게임 개발에 대한 기사를 천천히 작성해나가볼까 합니다.

그럼 이만 다음시간에 뵙겠습니다.

[새 소식](https://kimej.github.io/posts/%EB%84%A4%EC%9D%B4%EB%B2%84-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%8B%9C%EC%9E%91/)

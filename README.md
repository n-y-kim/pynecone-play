Test repo for pynecone! 🌲 공식 문서 번역하면서 끄적끄적 테스트

🌱 **Table of contents** 🌱
- [Getting Started](#getting-started)
  - [Introduction](#introduction)
    - [Motivation](#motivation)
    - [First Example](#first-example)
    - [The Structure of a Pynceone App(파인콘 앱의 구조)](#the-structure-of-a-pynceone-app파인콘-앱의-구조)
      - [Import](#import)
      - [State(정의)](#state정의)
      - [Event Handlers](#event-handlers)
      - [Frontend](#frontend)
      - [Routing](#routing)
      - [Compiling](#compiling)
    - [Next Steps](#next-steps)
  - [Project Structure](#project-structure)
    - [.web](#web)
    - [assets](#assets)
    - [Main Project](#main-project)
    - [Config](#config)
- [Components](#components)
  - [Overview](#overview)
    - [Component Basics](#component-basics)
    - [Another Example](#another-example)
    - [Pages](#pages)
  - [Props](#props)

## Getting Started

### Introduction

파인콘(Pynecone)은 웹앱을 빌드하고 배포하기 위한 풀스택 프레임워크다.

#### Motivation

파인콘은 아래의 목표들을 기반으로 만들어졌다:

- **순수 Python**<br>
    모든 것에 파이썬을 쓸 것. 새로운 언어를 배워야 하는 것에 대한 걱정은 할 필요 없다.
- **배우기 쉬울 것**<br>
    몇 분 안에 본인만의 새로운 앱을 만들고 공유하자. 어떠한 웹 개발 경험도 필요 없다.
- **완벽한 유동성**<br>
    일반적으로 사용되는 웹 프레임워크들처럼 유동적인 성능을 유지할 것. 파인콘은 시작하기 매우 쉽지만, 고급 기능 및 예제를 만들어 낼 수 있는 파워풀함을 가지고 있다.
- **배터리가 포함되어 있을 것(의역: 모든 것을 갖출 것)**<br>
  다른 다양한 외부 툴을 사용하기 위해 노력할 필요 없다. 파인콘은 프론트엔드, 백엔드, 그리고 배포까지 모두 담당한다.

#### First Example

이 부분은 직접 [pynecone에](https://pynecone.io/docs/getting-started/introduction) 방문해 확인하자. 만약 이 레포가 파인콘 번역 페이지를 따로 만든다면 코드를 직접 구현해서 넣어두겠다.

간단한 마이너스/플러스 버튼이 있는 화면 예제 코드:

```python
import pynecone as pc


class State(pc.State):
    count: int = 0

    def increment(self):
        self.count += 1

    def decrement(self):
        self.count -= 1


def index():
    return pc.hstack(
        pc.button(
            "Decrement",
            color_scheme="red",
            border_radius="1em",
            on_click=State.decrement,
        ),
        pc.heading(State.count, font_size="2em"),
        pc.button(
            "Increment",
            color_scheme="green",
            border_radius="1em",
            on_click=State.increment,
        ),
    )


app = pc.App(state=State)
app.add_page(index)
app.compile()
```

#### The Structure of a Pynceone App(파인콘 앱의 구조)

파인콘 앱의 구조가 어떻게 되어 있는지 자세히 들여다 보자.

##### Import

```python
import pynecone as pc
```

pynecone 라이브러리를 import 하면서 모든 것이 시작된다. 모든 파인콘 함수들과 클래스들은 `pc.` 접두사로 시작한다.

##### State(정의)

```python
class State(pc.State):
    count: int = 0
```

State는 앱 내에서 변경될 수 있는 모든 변수들(**vars**라고 함)과 변수들을 변경하는 함수를 정의한다.

위의 예시 코드에서 우리의 state는 counter의 현재 값을 가지고 있는 하나의 변수(var), `count` 를 가지고 있다. 우리는 `count`를 0으로 초기화했다.

##### Event Handlers

```python
def increment(self):
    self.count += 1

def decrement(self):
    self.count -= 1
```

State 안에는 state 변수(var)들을 변경하는 함수, 즉 **이벤트 핸들러(event handler)** 들을 정의한다.

이벤트 핸들러들은 파인콘 내의 state를 변경할 수 있는 유일한 방법이다. 이벤트 핸들러는 버튼 클릭이나 텍스트 입력 등과 같은 사용자의 액션에 따른 응답으로 호출될 수 있다. 이러한 액션들은 **이벤트**라고 한다.

현재 우리의 카운팅 앱은 `increment` 과 `decrement` 라는 두 개의 이벤트 핸들러를 가지고 있다.

##### Frontend

```python
def index():
    return pc.hstack(
        pc.button(
            "Decrement",
            color_scheme="red",
            border_radius="1em",
            on_click=State.decrement,
        ),
        pc.heading(State.count, font_size="2em"),
        pc.button(
            "Increment",
            color_scheme="green",
            border_radius="1em",
            on_click=State.increment,
        ),
    )
```

이 함수는 앱의 프론트엔드를 정의한다.

우리는 `pc.hstack`, `pc.button`, `pc.heading` 등과 같은 파인콘의 컴포넌트들을 사용하여 프론트엔드를 정의한다. 컴포넌트들은 중첩되어서 더 복잡한 레이아웃을 만드는데 사용할 수 있고, CSS를 활용해서 스타일링 할 수도 있다.

파인콘은 시작을 쉽게 돕기 위해서 [50개 이상의 빌트인 컴포넌트들](https://pynecone.io/docs/library)을 지원한다. 파인콘 개발팀은 계속해서 더 많은 컴포넌트들을 추가하고 있고, [본인의 리액트 컴포넌트도 쉽게 포장](https://pynecone.io/docs/advanced-guide/wrapping-react)할 수 있다. 

```python
pc.heading(State.count, font_size="2em"),
```

컴포넌트들은 앱의 state 변수를 참조할 수 있다. `pc.heading` 컴포넌트는 `State.count`을 참조하여 카운터(counter)의 현재 값을 보여준다. state를 참조하는 모든 컴포넌트들은 state가 변경될 떄마다 반응형으로 업데이트된다. 

```python
pc.button(
    "Decrement",
    color_scheme="red",
    border_radius="1em",
    on_click=State.decrement,
),
```

컴포넌트들은 이벤트 트리거들을 이벤트 핸들러에 바인딩해서 state와 통신한다. 예시로, `on_click` 은 사용자가 컴포넌트를 클릭했을 때 트리거되는 이벤트다.

예시 앱의 첫 번째 버튼은 `on_click` 이벤트를 `State.decrement` 이벤트 핸들러에 바인딩하고, 두 번째 버튼은 `State.increment` 핸들러에 바인딩한다.

##### Routing

```python
app = pc.App(state=State)
```

그 다음, 앱을 정의하고 어떤 state를 쓸 지 알려준다. 

```python
app.add_page(index)
```

그리고 counter 컴포넌트에 앱의 루트 URL 경로를 추가한다.

##### Compiling

```python
app.compile()
```

마지막으로 앱을 컴파일 하고 나면 앱을 실행할 준비는 모두 끝났다.

#### Next Steps

이제 끝이다! 우리는 프론트엔드 백엔드 모두를 40 줄도 안되는 코드로 만들어 냈다. 이제부터 우리는 하나의 명령어로 계속해서 웹을 개발 또는 배포할 수 있다.

계속해서 문서를 읽고 스스로 Pynecone을 어떻게 쓸 수 있는지 배워보자!

<hr>

### Project Structure

`myapp`이라는 새로운 앱 생성하기

```bash
mkdir myapp
cd myapp
pc init
```

생성되는 파일 구조:

```bash
myapp
├── .web
├── assets
├── myapp
│   ├── __init__.py
│   └── myapp.py
└── pcconfig.py
```

#### .web

파이콘 프론트엔드는 NextJS 앱으로 컴파일을 한다. 컴파일 결과물(output)은 `.web` 폴더에 저장된다. 해당 폴더를 만져야 할 일은 없지만, 이 폴더 내용물을 보는 건 디버깅에 도움이 된다.

각각 pynecone 페이지는 `.web/pages` 폴더 내에 상응하는 `.js` 파일로 컴파일 된다.

#### assets

`assets` 은 공개적으로 접근할 수 있는 모든 정적인 asset을 저장할 수 있는 곳이다. asset에는 이미지, 폰트, 그리고 기타 다른 파일을 말한다.

예시로, `assets/image.png` 을 저장했다면 앱에서 이런 코드로 보여줄 수 있다:

```python
pc.image(src="image.png")
```

#### Main Project

처음 프로젝트를 초기화하면 앱 이름과 동일한 폴더가 생성된다. 여기가 바로 앱의 로직을 작성하는 곳이다.
파인콘은 `myapp/myapp.py` 파일에 기본 디폴트 앱을 생성한다. 이 파일을 수정해서 앱을 취향대로 커스터마이징 할 수 있다.

#### Config

`pcconfig.py` 파일에는 앱의 환경설정이 담겨있다. 처음에 기본은 이렇게 생겼다:

```python
import pynecone as pc

config = pc.Config(
    app_name = "myapp",
    bun_path="$HOME/.bun/bin/bun",
    db_url="sqlite:///pynecone.db",
    env=pc.Env.DEV,
)
```

파인콘은 [bun](https://bun.sh/) 을 사용해서 자바스크립트 라이브러리를 관리한다. Bun은 `pc init` 초기화 명령 시 `~/.bun/bin/bun`에 자동으로 설치된다. config 파일에서 다른 특정 경로를 입력할 수도 있다.

데이터베이스 url은 데이터베이스 섹션에서 추가적으로 설명한다.

환경(env)은 `pc.Env.Dev` 또는 `pc.Env.PROD` 로 설정할 수 있다. self hosting 섹션에서 추가적으로 설명한다.

<hr>

## Components

### Overview

Component는 파이콘의 프런트 엔드를 구성하는 요소이다. 컴포넌트는 UI를 독립적이고, 재사용 할 수 있는 조각으로 분리해 줄 뿐만 아니라, 각 조각에 대해서도 독립적으로 구상하고 생각할 수 있게 해준다.

만약 리액트(React)에 친숙하다면, 파인콘 컴포넌트들은 단순히 리액트 컴포넌트를 감싸고 있는 '포장지' 임을 쉽게 이해할 수 있다.

컴포넌트들은 파이썬 함수로 생성된다. 컴포넌트들은 [props](https://pynecone.io/docs/components/props) 라는 키워드 인자로 구성할 수 있으며, 복잡한 UI를 만들기 위해서 중첩될 수 있다.

#### Component Basics

컴포넌트들은 Children과 Props로 구성된다.

| Children | Props |
| -- | -- |
| <ul><li>pynecone 컴포넌트가 컴포넌트 내부에 속해 있는 것</li><li>Children은 컴포넌트에 대한 위치 인수([positional arguments](https://www.codingem.com/what-is-a-positional-argument-in-python/))로 전달 됨.</li></ul>  | <ul><li>컴포넌트의 동작 및 모양에 영향을 미치는 속성</li><li>Props는 컴포넌트에 대한 키워드 인수(keyword arguments)로 전달 됨.</li></ul> |

`pc.text` 컴포넌트를 통해 예시를 살펴보자.

```python
pc.text("Hello World!", color="blue", font_size="1.5em")
```

여기서 `"Hello World!"`는 표시할 child 텍스트이고, `color`와 `font_size`는 글자의 모양을 변경할 수 있는 props이다.

> **일반적인 Python 데이터 타입을 컴포넌트의 children으로 전달할 수 있다.** 이는 문자열, 숫자, 그리고 다양한 기타 간단한 데이터 타입들을 전달하는 데 유용하다.

#### Another Example

이제 다른 컴포넌트들이 컴포넌트 내에 포함되어 있는 조금 더 복잡한 컴포넌트를 살펴보자. `pc.hstack` 컴포넌트는 자신의 children을 수평으로 정렬하는 컴포넌트이다.

<코드 결과는 pynecone 공식 홈페이지에서 확인...!>

```python
pc.hstack(
    pc.circular_progress(
        pc.circular_progress_label("50", color="green"),
        value=50,
    ),
    pc.circular_progress(
        pc.circular_progress_label(
            "∞", color="rgb(107,99,246)"
        ),
        is_indeterminate=True,
    ),
)
```

어떤 props들은 component에 따라 달라진다. 예를 들어 `pc.circular_progress` 컴포넌트의 `value` prop은 프로그레스 바의 진행 정도를 조절한다.

`color` 같은 스타일링 prop들은 많은 컴포넌트들에서 이용된다.

> [컴포넌트 라이브러리 페이지](https://pynecone.io/docs/library)에서 각 컴포넌트들의 props를 확인할 수 있다.

#### Pages

파인콘 앱들은 페이지(page)들로 구성된다. 페이지는 특정 URL 경로를 컴포넌트로 연결한다. 

컴포넌트를 반환(리턴)하는 함수를 이용해 페이지를 만들 수 있다. 기본적으로 함수 이름이 경로로 사용되지만, 경로를 직접 지정할 수도 있다.

```python
def index():
    return pc.text("Root Page")


def about():
    return pc.text("About Page")


app = pc.App()
app.add_page(index, path="/")
app.add_page(about, path="/about")
```

이 예시에서 우리는 `index` 라는 페이지를 루트 경로에 추가했다. 만약 `dev` 모드로 앱을 실행하고 있다면, `http://localhost:8000`으로 접속하면 확인할 수 있다.

유사하게 `about` 페이지는 `http://localhost:8000/about` 경로로 접속할 수 있다.

### Props


# pynecone-play

Test repo for pynecone! 🌲 공식 문서 번역하면서 끄적끄적 테스트

## Getting Started

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

<p>
    <div style="color:blue; font_size=1.5em">Hello World!</div>
</p>

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

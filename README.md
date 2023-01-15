# pynecone-play
Test repo for pynecone! 🌲

공식 문서 번역하면서 끄적끄적 테스트

## Getting Started

### Project Structure

`myapp`이라는 새로운 앱 생성하기
```bash
$ mkdir myapp
$ cd myapp
$ pc init
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
# pre-commit


<br>

## pre-commit란?

<br>
커밋 메시지를 작성하기 전에 호출이 되는 명령어입니다. <br>
커밋이 되기 전 문법 오류나 스타일, 정렬, 타입 오류 등을 체크할 때 사용합니다. <br>
개발자의 기호에 따라 선택하고 커스텀까지 할 수 있습니다. <br>
<br>
<br>

## pre-commit 적용 순서
<br>

1 git init <br>
2 pip install pre-commit <br>
3 pre-commit install  <br>
```
>>> pre-commit installed at .git/hooks/pre-commit
```

<br>
<br>

4 pre-commit run 
```
>>> An error has occurred: InvalidConfigError: 
=====> .pre-commit-config.yaml is not a file
Check the log at /Users/daco/.cache/pre-commit/pre-commit.log
```
<br>

이렇게 세팅 없이 바로 run을 하면 오류가 뜹니다. <br>
'pre-commit run' 전에 '.pre-commit-config.yaml' 를 세팅해주어야 합니다. 

<br>
<br>


5 .pre-commit-config.yaml 파일을 생성하고 다음처럼 작성합니다.<br>
(파이썬 버전은 사용하는 버전을 적어주세요.)<br>

```
repos:
  - repo: https://github.com/ambv/black
    rev: ''
    hooks:
      - id: black
        language_version: python3.8 
  - repo: https://gitlab.com/pycqa/flake8
    rev: ''
    hooks:
      - id: flake8
  - repo: https://github.com/pycqa/isort
    rev: ''
    hooks:
      - id: isort
  - repo: https://github.com/necaris/pre-commit-pyright
    rev: ''
    hooks:
      - id: pyright
```
<br>

각 항목을 설명하자면 
- black : 코드 포멧터, 코드 스타일을 통일시켜 줍니다.
- flake8 : 코드 린터, PEP8 규악을 지켰는지 검사합니다.
- isort : 파이썬 import를 정렬해주는 라이브러리입니다.
- pyright : 파이썬에서 타입을 체크해주는 라이브러리입니다. (mypy로 대체가능)

<br>

항목들은 개발자의 기호에 따라 취사선택할 수 있습니다.<br>
필요하다고 생각하는 것들만 추가하면 됩니다.<br>
rev 에서 ''은 원하는 버전으로 바꿀 수 있습니다. (넣지 않아도 실행됩니다.)
<br>


6 다시 pre-commit run

```
black................................................(no files to check)Skipped
flake8...............................................(no files to check)Skipped
isort................................................(no files to check)Skipped
pyright..............................................(no files to check)Skipped
```
<br> 다음과 같은 결과가 나오면 정상입니다. <br>
만약 다른 결과가 나온다면 에러메시지에 따라 수정해주세요. <br>
<br>

7 정상적으로 작동하는지 테스트를 해봅니다.
```
예시) 일부러 비정상적인 스타일을 만듭니다.

from fastapi import FastAPI
app = FastAPI()
@app.get("/")
async def root():
    return {"message": "Hello World"}
```


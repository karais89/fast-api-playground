# 첫 단계
- https://fastapi.tiangolo.com/tutorial/first-steps/#step-2-create-a-fastapi-instance

가장 간단한 FastAPI 파일은 다음과 같습니다.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

그것을 `main.py` 파일로 복사하십시오.

라이브 서버 실행 :
```shell
uvicorn main:app --reload
```

### Note
`uvicorn main:app`은 다음을 참조합니다.
- `main` : main.py 파일 (python "모듈").
- `app` : main.py의 내부에서 생성 된 객체 = fastapi () 라인을 사용합니다.
- `--reload` : 코드가 변경된 후 서버를 다시 시작하십시오. 개발에만 사용하십시오.

출력에서는 같은 줄이 있습니다.
```text
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```
이 행은 해당 로컬 컴퓨터에서 앱이 제공되는 URL을 표시합니다.

## Check it

http://127.0.0.1:8000에서 브라우저를 엽니다.

JSON 응답은 다음과 같이 표시됩니다.

```python
{"message": "Hello World"}
```

## 대화 형 API 문서
이제 http://127.0.0.1:8000/docs로 이동하십시오.

자동 대화 형 API 문서 (Swagger UI가 제공)를 볼 수 있습니다.

## 대체 API 문서

그리고 이제 http://127.0.0.1:8000/REDOC로 이동하십시오

당신은 대체 자동 문서 (Redoc에서 제공)를 볼 수 있습니다.

## OpenAPI

FASTAPI는 API를 정의하기 위해 OpenAPI 표준을 사용하여 모든 API를 사용하여 "스키마"를 생성합니다.

### "Schema"

"스키마"는 무언가의 정의 또는 설명입니다. 그것을 구현하는 코드는 아니지만 추상 설명 만 설명합니다.

### API "schema"

이 경우 OpenAPI는 API의 스키마를 정의하는 방법을 지시하는 사양입니다.

이 스키마 정의에는 API 경로, 가능한 매개 변수 등이 포함됩니다.

### Data "schema"

"스키마"라는 용어는 JSON 콘텐츠와 같은 일부 데이터의 모양을 나타낼 수도 있습니다.

이 경우 JSON 속성 및 데이터 유형 등을 의미합니다.

### OpenAPI and JSON Schema

OpenAPI는 API에 대한 API 스키마를 정의합니다. 
그리고이 스키마는 JSON 데이터 스키마의 표준 인 JSON 스키마를 사용하여 API가 보낸 데이터의 정의 (또는 "스키마")를 포함합니다.

### Check the openapi.json

원시 OpenAPI 스키마가 어떻게 보이는지 궁금한 경우 FastApi는 모든 API에 대한 설명과 함께 JSON (스키마)을 자동으로 생성합니다.

http://127.0.0.1:8000/openapi.json에서 직접 볼 수 있습니다.

그것은 다음과 같이 시작하는 JSON을 보여줍니다.

````json
{
    "openapi": "3.0.2",
    "info": {
        "title": "FastAPI",
        "version": "0.1.0"
    },
    "paths": {
        "/items/": {
            "get": {
                "responses": {
                    "200": {
                        "description": "Successful Response",
                        "content": {
                            "application/json": {
                              
...
````

## What is OpenAPI for

OpenAPI 스키마는 두 대화 형 문서 시스템이 포함 된 두 가지 대화 형 문서 시스템의 전원을 제공합니다.

OpenAPI를 기반으로 한 수십 가지 대안이 있습니다. FastAPI로 구축 된 응용 프로그램에 해당 대안을 쉽게 추가 할 수 있습니다.

또한 API와 통신하는 클라이언트에 대해 자동으로 코드를 생성 할 수도 있습니다. 예를 들어, 프론트 엔드, 모바일 또는 IoT 응용 프로그램입니다.

## Recap, step by step

### Step 1: import FastAPI

```python
from fastapi import FastAPI


app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

`FastAPI`는 API의 모든 기능을 제공하는 Python 클래스입니다.

#### Technical Details

`FastAPI`는 `Starlette`에서 직접 상속되는 클래스입니다.

`FastAPI`와 함께 모든 `Starlette` 기능을 사용할 수 있습니다.

### Step 2: create a FastAPI "instance"

```python
from fastapi import FastAPI


app = FastAPI()



@app.get("/")
async def root():
    return {"message": "Hello World"}
```

여기서 `app` 변수는 클래스 `FastAPI`의 "인스턴스"가 됩니다.

이것은 모든 API를 만드는 것과 관련된 상호 작용의 주요 요점이 될 것입니다.

이 `app` 프로그램은 명령의 `uvicorn`이 참조하는 것과 동일합니다.

```shell
uvicorn main:app --reload

INFO: Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

다음과 같은 앱을 만드는 경우 :

```python
from fastapi import FastAPI


my_awesome_api = FastAPI()



@my_awesome_api.get("/")
async def root():
    return {"message": "Hello World"}
```

파일을 `main.py`에 넣으면 다음과 같이 `uvicorn` 호출합니다.

```shell
uvicorn main:my_awesome_api --reload

INFO: Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

### Step 3: create a path operation

Path

"경로"는 첫 번째 `/`를 시작하는 URL의 마지막 부분을 나타냅니다.

그래서, URL은 다음과 같습니다.

```text
https://example.com/items/foo
```

... 그 path은 될 것입니다

```text
/items/foo
```

#### info
"경로"는 일반적으로 "엔드 포인트"또는 "경로"라고 합니다.

API를 구축하는 동안 "경로"는 "우려"와 "자원"을 분리하는 가장 중요한 방법입니다.

### Operation
"Operation" 여기에서는 http "메소드"중 하나를 나타냅니다.

다음 중 하나:
- `POST`
- `GET`
- `PUT`
- `DELETE`

... 그리고 더 많은 이국적인 것들 :
- `OPTIONS`
- `HEAD`
- `PATCH`
- `TRACE`

HTTP 프로토콜에서 이러한 "메소드의 하나 (또는 그 이상)를 사용하여 각 경로와 통신 할 수 있습니다.

---

API를 구축 할 때 일반적으로 이러한 특정 HTTP 방법을 사용하여 특정 작업을 수행합니다.

일반적으로 다음을 사용합니다.
- `POST` : 데이터를 작성합니다.
- `GET` : 데이터를 읽습니다.
- `PUT` : 데이터를 업데이트 합니다.
- `DELETE` : 데이터를 삭제 합니다.

따라서 OpenAPI에서 각 HTTP 메소드를 "operation"이라고합니다.

우리는 "operations"이라고 부를 것입니다.

### 경로 조작 decorator 정의

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

`@app.get("/")`은 아래의 함수가 다음과 같이 처리하는 요청을 담당하는 FastAPI 에게 다음을 수행합니다.
- the path /
- get operation 사용

#### @decorator Info

파이썬의 @something 구문을 "Decorator"라고합니다.

당신은 그것을 함수 위에 놓습니다. 예쁜 장식 모자처럼 (나는 그것이 용어가 어디에서 왔는지 추측한다).

"decorator"는 아래의 기능을 취하고 그것으로 뭔가를합니다.

우리의 경우이 Decorator는 아래의 함수가 경로에 해당하는 기능이 있음을 Fastapi에 알려줍니다.

그것은 "경로 작업 장식"입니다.

다른 작업을 사용할 수도 있습니다.
- @app.post()
- @app.put()
- @app.delete()

그리고 더 많은 이국적인 것들 :
- @app.options()
- @app.head()
- @app.patch()
- @app.trace()

#### Tip

원하는대로 각 작업 (HTTP 메소드)을 무료로 사용할 수 있습니다.

FastAPI는 특정 의미를 적용하지 않습니다.

여기에있는 정보는 요구 사항이 아닌 가이드 라인으로 표시됩니다.

예를 들어 GraphQL 을 사용할 때는 정상적으로 POST 작업 만 사용하여 모든 작업을 수행합니다.

## Step 4: define the path operation function

이것은 우리의 "경로 조작 기능"입니다.
- path: is /. 
- operation: is get.
- function: is the function below the "decorator" (below @app.get("/")).

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

이것은 파이썬 기능입니다.

GET 조작을 사용하여 URL "/" 요청을 받을 때마다 FastAPI에 의해 호출됩니다.

이 경우 비동기 기능입니다.

또한 비동기 DEF 대신 정상적인 기능으로 정의 할 수도 있습니다.

## Step 5: return the content

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

`dict`, `list`, SINGULALE 값을 `str`, `int` 등으로 반환 할 수 있습니다.

Pydantic 모델을 반환 할 수도 있습니다 (나중에 더 많은 것을 볼 수 있습니다).

JSON(ORMS등)으로 자동으로 변환 될 다른 많은 다른 개체와 모델이 있습니다. 

좋아하는 것들을 사용해보십시오. 이미 지원되는 것이 매우 가능합니다.

## Recap
- FastAPI 불러옵니다.
- 앱 인스턴스를 만듭니다.
- 경로 작업 장식을 씁니다 (like @app.get("/")).
- 경로 작동 기능을 작성하십시오 (like def root(): ... above).
- 개발 서버를 실행하십시오 (like uvicorn main:app --reload).
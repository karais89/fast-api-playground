# Query Parameters
- https://fastapi.tiangolo.com/tutorial/query-params/

경로 매개 변수의 일부가 아닌 다른 함수 매개 변수를 선언하면 자동으로 "쿼리" 매개 변수로 해석 됩니다.

```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

쿼리는 다음과 같은 이어진 키 값 쌍 세트입니다. url 에서 `?` 뒤에 오고 `&` 문자로 문자를 구분됩니다.

예를 들어, URL에서 :
```text
http://127.0.0.1:8000/items/?skip=0&limit=10
```
... 쿼리 매개 변수는 다음과 같습니다
- `skip` : 값이 `0` 입니다.
- `limit` : 값이 `10` 입니다.

그들이 URL의 일부이기 때문에 "자연적으로" 문자열입니다.

그러나 (위의 예제에서, `int`) 파이썬 형식으로 선언 할 때 해당 유형으로 변환되고 유효성이 검사됩니다.

경로 매개 변수에 적용된 모든 동일한 프로세스가 쿼리 매개 변수에도 적용됩니다.
- 편집기 지원 (분명히)
- 데이터 "구문 분석"
- 데이터 유효성 검사
- 자동 문서

## 기본값

쿼리 매개 변수가 경로의 고정 부분이 아니므로 선택 사항이며 기본값을 가질 수 있습니다.

위의 예에서는 `Skip=0` 및 `limit=10`의 기본값이 있습니다.

그래서, 다음 URL은:
```text
http://127.0.0.1:8000/items/
```
아래 URL과 같을 것입니다 :
```text
http://127.0.0.1:8000/items/?skip=0&limit=10
```
그러나 예를 들어,
```text
http://127.0.0.1:8000/items/?skip=20
```
함수의 매개 변수 값은 다음과 같습니다.

- `skip=20` : URL에서 설정한 값
- `limit=10` : 기본값으로 설정한 값

## 선택적 매개 변수
같은 방법으로 기본값을 `None`로 설정하여 선택적 쿼리 매개 변수를 선언 할 수 있습니다.
```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Optional[str] = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
```
이 경우 함수 매개 변수 `q`는 선택 사항이며 기본적으로 `None` 일 것입니다.

### check

또한 `FastAPI`는 PATH 매개 변수 `ITEM_ID`가 경로 매개 변수이고 Q는 경로 매개 변수가 아니므로 QUERY 매개 변수입니다.

### Note

`FastAPI`는 Q가 = None 이기 때문에 Q가 선택 사항임을 알 수 있습니다.

`Optional` `Optional[str]`은 `FastAPI`에서 사용되지 않습니다.

(`FastAPI`는 `str` 파트 만 사용합니다). `Optional[str]`는 편집기에서 코드에서 오류를 찾는 데 도움이 됩니다.

## 검색어 매개 변수 유형 변환

`bool` 유형을 선언 할 수도 있으며 변환됩니다.

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Optional[str] = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```

이 경우, 당신이 가는 경우 :
```text
http://127.0.0.1:8000/items/foo?short=1
```
혹은
```text
http://127.0.0.1:8000/items/foo?short=True
```
혹은
```text
http://127.0.0.1:8000/items/foo?short=true
```
혹은
```text
http://127.0.0.1:8000/items/foo?short=on
```
혹은
```text
http://127.0.0.1:8000/items/foo?short=yes
```
또는 다른 사례 변형 (대문자, 첫 번째 문자가 있는 경우 등), 귀하의 기능은 `bool` 값이 TRUE로 짧은 매개 변수를 볼 수 있습니다. 그렇지 않으면 거짓으로.

## 다중 경로 및 쿼리 매개 변수

여러 경로 매개 변수와 쿼리 매개 변수를 동시에 선언 할 수 있으며 `FastAPI`는 어느 것이 무엇인지 알고 있습니다.

그리고 특정 순서로 선언 할 필요가 없습니다.

이름으로 감지됩니다.

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: Optional[str] = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```

## 필수 쿼리 매개 변수

경로가 아닌 매개 변수의 기본값을 선언 할 때(지금은 쿼리 매개 변수 만 참조)만 사용하지 않습니다.

특정 값을 추가하지 않고 옵션으로 만 설정하려면 기본값을 없음으로 설정하십시오.

그러나 쿼리 매개 변수가 필요한 경우 기본값을 선언하지 않을 수 있습니다.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):
    item = {"item_id": item_id, "needy": needy}
    return item
```

여기서 `needy` 쿼리 매개 변수는 `str` 유형의 필수 쿼리 매개 변수입니다.

브라우저에서 다음과 같은 URL을 열면
```text
http://127.0.0.1:8000/items/foo-item
```

... 필요한 매개 변수를 추가하지 않으면 다음과 같은 오류가 표시됩니다.

```json
{
    "detail": [
        {
            "loc": [
                "query",
                "needy"
            ],
            "msg": "field required",
            "type": "value_error.missing"
        }
    ]
}
```

`needy`는 필수 매개 변수이므로 URL에서 설정해야합니다.
```text
http://127.0.0.1:8000/items/foo-item?needy=sooooneedy
```
이것은 아래와 깉이 작동합니다.
```json
{
    "item_id": "foo-item",
    "needy": "sooooneedy"
}
```

물론 일부 매개 변수는 필요에 따라 정의 할 수 있으며 일부는 기본값을 갖고 일부는 완전히 선택적으로 정의 할 수 있습니다.

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: Optional[int] = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```
이 경우 3 개의 쿼리 매개 변수가 있습니다.
- `needy`는 `str`을 필수 매개 변수이다.
- `skip`는 `int`형이며 기본 값이 `0`이다.
- `limit`는 `int`형이며 옵션 값이다.

### 팁
경로 매개 변수와 같은 방식으로 열거 형을 사용할 수도 있습니다.
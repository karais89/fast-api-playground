# fast-api-playground

환경
- Windows 10
- Pycharm 2020.3
- MySQL
- 공식 튜토리얼의 경우 별도의 브랜치로 연습
- https://fastapi.tiangolo.com/

## 세팅

```sh
pip install fastapi
pip install uvicorn[standard]
```

pycharm 세팅은 생략

## 모바일 웹 서버 프레임 워크 개발
- 소규모 모바일 게임 개발 (프로젝트 웬디 참고)
- https://github.com/totuworld/Wendy

### 제공할 기능

- [ ] 기기정보 관리
- [ ] 사용자 정보 관리
- [ ] 통화 관리
- [ ] 아이템 관리
- [ ] 지급 관리
- [ ] 쿠폰
- [ ] 인앱 영수증 검증

## 인증
- https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/

### 사용자 비밀번호 암호화

### access token & JWT
```shell
pip install PyJWT
```

### 데코레이터를 사용하여 모든 엔드포인트에 인증 절차 삽입



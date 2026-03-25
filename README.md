# Postman Auth API Test Project

## 프로젝트 소개
이 프로젝트는 Automation Exercise의 인증(Auth) 관련 API를 대상으로 Postman을 활용해 테스트한 QA 실습 프로젝트입니다.

회원가입과 로그인 API를 중심으로 정상/비정상 케이스를 설계하고
각 요청에 대해 응답 코드와 메시지를 검증하는 자동 테스트 스크립트도 함께 작성했습니다.

---

## 테스트 대상
- 회원가입(Create Account)
- 로그인 검증(Verify Login)

---

## 테스트 환경
- Tool: Postman
- API Test Type: Manual + Automated Validation
- Body Type: x-www-form-urlencoded
- Variables Used:
  - `{{baseUrl}}`
  - `{{testEmail}}`
  - `{{testPassword}}`
  - `{{testName}}`

---

## 테스트 케이스 구성

### 1. Create Account - Success
- 목적: 정상 회원가입 요청 시 계정 생성 여부 확인
- 기대 결과:
  - `responseCode = 201`
  - `message = "User created!"`

### 2. Verify Login - Success
- 목적: 정상 이메일/비밀번호 입력 시 로그인 검증 성공 확인
- 기대 결과:
  - `responseCode = 200`
  - `message = "User exists!"`

### 3. Verify Login - Wrong Password
- 목적: 잘못된 비밀번호 입력 시 예외 응답 확인
- 기대 결과:
  - `responseCode = 404`
  - `message = "User not found!"`

### 4. Verify Login - Missing Email
- 목적: 필수 파라미터(email) 누락 시 예외 처리 확인
- 기대 결과:
  - `responseCode = 400`
  - `message`에 `missing` 포함

### 5. Verify Login - Invalid Method
- 목적: 지원하지 않는 HTTP Method 요청 시 예외 처리 확인
- 기대 결과:
  - `responseCode = 405`
  - `message`에 `not supported` 포함

---

## 자동 검증(Post-response Script)
각 요청에 대해 아래 항목을 Postman Script로 자동 검증했습니다.

- HTTP 상태코드 확인
- JSON 응답 파싱
- `responseCode` 값 확인
- `message` 값 또는 포함 문자열 확인

예시:
```javascript
pm.test("responseCode는 200이다", function () {
    const res = pm.response.json();
    pm.expect(res.responseCode).to.eql(200);
});
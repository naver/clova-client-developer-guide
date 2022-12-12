# 클라이언트 인증 API

이 문서는 CLOVA 클라이언트 인증용 REST API 레퍼런스입니다. 클라이언트가 CIC에 연결하려면 [CLOVA access token을 생성](/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken)해야 합니다. 클라이언트 인증 서버는 CLOVA access token 획득 및 관리에 필요한 클라이언트 인증 API를 제공하고 있으며, 여기에서는 클라이언트 인증 API에 대해 설명합니다.

## Base URI

이 API의 base URI는 다음과 같습니다.

```
https://auth.clova.ai/
```

## Authroization code 발급 {#IssueAuthorizationCode}

{{ book.ServiceEnv.OrientedService }} 계정 access token 및 [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)와 같은 것을 파라미터로 전달해 authorization code를 요청합니다. Authorization code는 CLOVA access token을 발급받을 때 사용됩니다.
일반적으로 사용자 인증을 받기 위해 클라이언트 기기와 페어링(Pairing)된 앱에서 인증을 처리합니다. 다만, 페어링된 앱에서 클라이언트 쪽으로 CLOVA access token을 전송하는 것은 보안상 이슈가 있기 때문에 이 코드를 대신 클라이언트로 보냅니다. 클라이언트는 전달받은 authorization code를 다시 클라이언트 인증 서버로 전달하여 [CLOVA access token을 요청](#IssueCLOVAAccessToken)해야 합니다.

### Endpoint

```
GET /authorize
```

### Parameter

| 입력 타입 | 필드 이름     | 자료형  | 필드 설명                   | 필수 여부 |
|-----------|---------------|---------|-----------------------------|:---------:|
| Header    | `Authorization` | string | [획득한 {{ book.ServiceEnv.OrientedService }} access token](/Develop/Guides/Interact_with_CIC.md#ObtainCLOVAAccessToken)을 입력합니다. | 필수 |
| Query     | `client_id` | string | 클라이언트 ID ([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `device_id` | string | 클라이언트 기기의 MAC 주소나 생성한 UUID | 필수 |
| Query     | `model_id` | string | 클라이언트 기기의 모델 ID | 필수 |
| Query     | `request_vu` | string | {{ book.ServiceEnv.OrientedService }} 계정 인증 없이 <a id="GuestMode"></a>guest 모드 형태의 서비스를 제공할 때 사용하는 파라미터. 이 파라미터를 사용하려면 Authroization 헤더를 생략하고 이 파라미터의 값을 `'Y'`로 설정하십시오.<div class="tip"><p><strong>Tip!</strong></p><p><code>request_vu</code>의 기본 값은 <code>N</code>이며, {{ book.ServiceEnv.OrientedService }} 계정을 인증하여 쓰는 것이 기본 방침입니다.</p></div> | 선택 |
| Query     | `response_type` | string | 응답 유형. 현재 `"code"`만 지원합니다. | 필수 |
| Query     | `state` | string | 요청 위조(cross-site request forgery) 공격을 방지하기 위해 클라이언트에서 사용하는 상태 token 값(URI 인코딩 적용)	 | 필수 |

### Request example

```http
GET https://auth.clova.ai/authorize?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model&response_type=code&state=FKjaJfMlakjdfTVbES5ccZ HTTP/1.1
Host: auth.clova.ai
Accept: application/json
Authorization: Bearer [{{ book.ServiceEnv.OrientedService }} access token]

```

```shell
curl -X GET https://auth.clova.ai/authorize?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model&response_type=code&state=FKjaJfMlakjdfTVbES5ccZ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer [{{ book.ServiceEnv.OrientedService }} access token]'
```

### Response

| 상태 코드 | 설명                                          | 응답 객체            |
|-----------|-----------------------------------------------|----------------------|
| 200       | 요청 처리 성공 시 받는 응답 | [authorizeResult](#schemaauthorizeresult) |
| 400       | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 받는 응답 | [authorizeResult](#schemaauthorizeresult) |
| 403       | 헤더에 포함된 {{ book.ServiceEnv.OrientedService }} access token이 유효하지 않을 때 받는 응답. 사용자가 CLOVA 서비스를 탈퇴하면 사용자는 1 개월 동안 해당 계정으로 CLOVA 서비스를 사용할 수 없습니다. <div class="note"><p><strong>Note!</strong></p><p>클라이언트가 <code>423 Locked</code> 상태 코드를 받게 되면 "CLOVA 서비스를 탈퇴한 계정입니다. 탈퇴한 후 한 달 뒤에 해당 계정으로 다시 로그인하면 CLOVA 서비스를 사용하실 수 있습니다."라고 안내 문구를 제공해야 합니다.</p></div> | [authorizeResult](#schemaauthorizeresult) |
| 423       | CLOVA를 탈퇴한지 한 달이 안된 사용자의 {{ book.ServiceEnv.OrientedService }} 계정으로 인증을 시도했을 때 받는 응답. 사용자가 CLOVA 서비스를 탈퇴하면 사용자는 1 개월 동안 해당 계정으로 CLOVA 서비스를 사용할 수 없습니다. | [authorizeResult](#schemaauthorizeresult) |
| 451       | 사용자가 이용 약관을 동의하지 않았을 때 받는 응답. 클라이언트는 이 응답을 받으면 `redirect_uri` 필드에 있는 주소로 이동하여 웹 페이지를 표시해야 합니다. 해당 URI는 사용자에게 서비스 이용 약관에 대해 동의를 받는 페이지입니다. | [authorizeResult](#schemaauthorizeresult) |
| 500       | 서버 내부 오류로 인한 authorization code 발급 실패 시 받는 응답 | [authorizeResult](#schemaauthorizeresult) |

### Response example

```json
// 200 Response
{
  "summary": "HTTP 응답 메시지가 200 OK 상태 코드를 가지는 예",
  "value": {
    "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
  }
}

// 400 Response
{
  "code": "string",
  "redirect_uri": "string",
  "state": "string"
}

// 451 Response
{
  "summary": "HTTP 응답 메시지가 451 Unavailable For Legal Reasons 상태 코드를 가지는 예",
  "value": {
    "code": "4mrklvwoC_KNgDlvmslka",
    "redirect_uri": "https://example.net/clova/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
  }
}
```

## CLOVA access token 발급 {#IssueCLOVAAccessToken}

[발급받은 authorization code](#IssueAuthorizationCode)를 사용하여 클라이언트 인증 서버에 CLOVA access token을 요청합니다.

### Endpoint

```
GET /token?grant_type=authorization_code
```

### Parameter

| 입력 타입 | 필드 이름     | 자료형  | 필드 설명                   | 필수 여부 |
|-----------|---------------|---------|-----------------------------|:---------:|
| Query     | `client_id` | string | 클라이언트 ID ([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `client_secret` | string | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `code` | string | [발급받은 authorization code](#IssueAuthorizationCode) | 필수 |
| Query     | `device_id` | string | 클라이언트 기기의 MAC 주소나 생성한 UUID | 필수 |
| Query     | `model_id` | string | 클라이언트 기기의 모델 ID | 필수 |

### Request example

```http
GET https://auth.clova.ai/token?grant_type=authorization_code?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&code=cnl__eCSTdsdlkjfweyuxXvnlA&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model HTTP/1.1
Host: auth.clova.ai
Accept: application/json

```

```shell
curl -X GET https://auth.clova.ai/token?grant_type=authorization_code?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&code=cnl__eCSTdsdlkjfweyuxXvnlA&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model \
  -H 'Accept: application/json'
```

### Response

| 상태 코드 | 설명                                          | 응답 객체            |
|-----------|-----------------------------------------------|----------------------|
| 200       | 요청 처리 성공 시 받는 응답 | [tokenResult](#tokenResult) |
| 400       | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 받는 응답 | [tokenResult](#tokenResult) |
| 500       | 서버 내부 오류로 인한 authorization code 발급 실패 시 받는 응답 | [tokenResult](#tokenResult) |

### Response example

```json
// 200 Response
{
  "value": {
    "access_token": "XHapQasdfsdfFsdfasdflQQ7w",
    "expires_in": 332000,
    "refresh_token": "GW-Ipsdfasdfdfs3IbHFBA",
    "token_type": "Bearer"
  }
}

// 400 Response
{
  "access_token": "string",
  "expires_in": 0,
  "refresh_token": "string",
  "token_type": "string"
}
```

## CLOVA access token 갱신 {#RefreshCLOVAAccessToken}

발급받은 refresh token을 사용하여 CLOVA access token을 갱신합니다.

### Endpoint

```
GET /token?grant_type=refresh_token
```

### Parameter

| 입력 타입 | 필드 이름     | 자료형  | 필드 설명                   | 필수 여부 |
|-----------|---------------|---------|-----------------------------|:---------:|
| Query     | `client_id` | string | 클라이언트 ID ([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `client_secret` | string | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `device_id` | string | 클라이언트 기기의 MAC 주소나 생성한 UUID | 선택 |
| Query     | `model_id` | string | 클라이언트 기기의 모델 ID | 필수 |
| Query     | `refresh_token` | string | 인증 성공 후 발급받은 refresh token | 필수 |

### Request example

```http
GET https://auth.clova.ai/token?grant_type=refresh_token?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&model_id=test_model&refresh_token=GW-Ipsdfasdfdfs3IbHFBA HTTP/1.1
Host: auth.clova.ai
Accept: application/json

```

```shell
# You can also use wget
curl -X GET https://auth.clova.ai/token?grant_type=refresh_token?client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&model_id=test_model&refresh_token=GW-Ipsdfasdfdfs3IbHFBA \
  -H 'Accept: application/json'
```

### Response

| 상태 코드 | 설명                                          | 응답 객체            |
|-----------|-----------------------------------------------|----------------------|
| 200       | 요청 처리 성공 시 받는 응답 | [tokenResult](#tokenResult) |
| 400       | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 받는 응답 | [tokenResult](#tokenResult) |
| 500       | 서버 내부 오류로 인한 authorization code 발급 실패 시 받는 응답 | [tokenResult](#tokenResult) |

### Response example

```json
// 200 Response
{
  "value": {
    "access_token": "xFcH08vYQcahQWouqIzWOw",
    "expires_in": 12960000,
    "refresh_token": "drJK-soIQI6vqEukqsLU2g",
    "token_type": "Bearer"
  }
}

// 400 Response
{
  "access_token": "string",
  "expires_in": 0,
  "refresh_token": "string",
  "token_type": "string"
}
```

## CLOVA access token 삭제 {#DeleteCLOVAAccessToken}

[발급받은 CLOVA access token](#IssueCLOVAAccessToken)을 삭제합니다. 응답으로 삭제한 CLOVA access token에 대한 정보가 반환됩니다.

### Endpoint

```
GET /token?grant_type=delete
```

### Parameter

| 입력 타입 | 필드 이름     | 자료형  | 필드 설명                   | 필수 여부 |
|-----------|---------------|---------|-----------------------------|:---------:|
| Query     | `access_token` | string | 인증 성공 후 발급받은 CLOVA access token | 필수 |
| Query     | `client_id` | string | 클라이언트 ID ([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `client_secret` | string | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조) | 필수 |
| Query     | `device_id` | string | 클라이언트 기기의 MAC 주소나 생성한 UUID | 필수 |
| Query     | `model_id` | string | 클라이언트 기기의 모델 ID | 필수 |

### Request example

```http
GET https://auth.clova.ai/token?grant_type=delete?access_token=xFcH08vYQcahQWouqIzWOw&client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model HTTP/1.1
Host: auth.clova.ai
Accept: application/json

```

```shell
# You can also use wget
curl -X GET https://auth.clova.ai/token?grant_type=delete?access_token=xFcH08vYQcahQWouqIzWOw&client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ&client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D&device_id=aa123123d6-d900-48a1-b73b-aa6c156353206&model_id=test_model \
  -H 'Accept: application/json'
```

### Response

| 상태 코드 | 설명                                          | 응답 객체            |
|-----------|-----------------------------------------------|----------------------|
| 200       | 요청 처리 성공 시 받는 응답 | [tokenResult](#tokenResult) |
| 400       | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 받는 응답 | [tokenResult](#tokenResult) |
| 500       | 서버 내부 오류로 인한 authorization code 발급 실패 시 받는 응답 | [tokenResult](#tokenResult) |

### Response example

```json
// 200 Response
{
  "value": {
    "access_token": "xFcH08vYQcahQWouqIzWOw",
    "expires_in": 12960000,
    "refresh_token": "drJK-soIQI6vqEukqsLU2g",
    "token_type": "Bearer"
  }
}

// 400 Response
{
  "access_token": "string",
  "expires_in": 0,
  "refresh_token": "string",
  "token_type": "string"
}
```

## 공유 객체 {#SharedObjects}

여기서는 요청이나 응답 시 사용되는 공유 객체에 대해 설명합니다.

### authorizeResult {#authorizeResult}

[`/authorize`](#IssueAuthorizationCode)에 대한 결과를 가지는 응답 객체

```json
{
  "code": "string",
  "redirect_uri": "string",
  "state": "string"
}
```

다음과 같은 필드를 가집니다.


| 필드 이름     | 자료형  | 필드 설명                   | 필수/포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `code` | string | 인증 서버로부터 발급받은 authorization code. HTTP 응답 메시지가 `200`이나 `451`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다. | 필수/항상 |
| `redirect_uri` | string | 서비스 이용 약관과 관련된 내용을 제공하는 페이지 URI. HTTP 응답 메시지가 `451 Unavailable For Legal Reasons`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다. 클라이언트는 이 필드에 포함된 URI로 이동하여 페이지를 표시해야 합니다. 사용자가 이용 약관에 동의하면 클라이언트는 `302 Found`(URL redirection) 상태 코드를 가진 응답을 다음 URL과 함께 수신하게 됩니다. <ul><li><code>clova://agreement-success</code>: 사용자가 이용 약관 동의를 완료함. 클라이언트는 CLOVA access token 발급을 위해 다음 단계를 계속 진행할 수 있습니다.</li><li><code>clova://agreement-failure?error=[reason]</code>: 사용자가 이용 약관에 동의하지 않았거나 서버 오류로 이용 약관 동의에 실패함. 클라이언트는 적절한 예외 처리를 해야 합니다. `error` 파라미터의 값으로 다음과 같은 값이 전달될 수 있습니다.<ul><li><code>"server_error"</code>: 내부 서버 오류</li><li><code>"terms_not_agreed"</code>: 필수 약관 동의가 수행되지 않음</li><li><code>"user-disagreement"</code>: 사용자가 약관에 동의하지 않음</li></ul><div class="tip"><p><strong>Tip!</strong></p><p>이 필드에 명시된 URI(약관 동의 페이지)로 페이지를 이동(redirect)할 때 <code>redirect_uri</code> 파라미터를 추가할 수 있습니다. <code>redirect_uri</code> 추가하면 이용 약관 페이지의 응답이 아래와 같은 형태를 가지게 됩니다.</p><ul><li><code>https://[redirect_uri]?code=[code]&state=[state]</code>: 사용자가 이용 약관 동의를 완료함.</li><li><code>https://[redirect_uri]?code=[code]&state=[state]&error=[reason]</code>: 사용자가 이용 약관에 동의하지 않았거나 서버 오류로 이용 약관 동의에 실패함.</li></ul></div> | 선택/조건부 |
| `state` | string | 요청 위조(cross-site request forgery) 공격을 방지하기 위해 클라이언트에서 전달받은 상태 token을 복호화한 값(URL 디코딩 적용). HTTP 응답 메시지가 `200`이나 `451`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다. | 필수/항상 |

### tokenResult {#tokenResult}

[`/token?grant_type=authorization_code`](#IssueCLOVAAccessToken)와 [`/token?grant_type=refresh_token`](#RefreshCLOVAAccessToken)에 대한 결과를 가지는 응답 객체

```json
{
  "access_token": "string",
  "expires_in": 0,
  "refresh_token": "string",
  "token_type": "string"
}
```

다음과 같은 필드를 가집니다.


| 필드 이름     | 자료형  | 필드 설명                   | 필수/포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token` | string | CLOVA access token | 필수/항상 |
| `expires_in` | number | CLOVA access token의 유효 기간. 초 단위이며, `0` 이상의 정수 값을 가집니다.) | 필수/항상 |
| `refresh_token` | string | CLOVA access token을 갱신하기 위한 refresh token | 필수/항상 |
| `token_type` | string | CLOVA access token의 타입. `"Bearer"`로 고정 반환됩니다. | 필수/항상 |

### tokenDeletionResult {#tokenDeletionResult}

[`/token?grant_type=delete`](#DeleteCLOVAAccessToken)에 대한 결과를 가지는 응답 객체

```json
{
  "access_token": "string",
  "client_id": "string",
  "expires_in": 0
}
```

다음과 같은 필드를 가집니다.


| 필드 이름     | 자료형  | 필드 설명                   | 필수/포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token` | string | 삭제한 CLOVA access token | 필수/항상 |
| `client_id` | string | 클라이언트 ID | 필수/항상 |
| `expires_in` | number | 삭제한 CLOVA access token이 가졌던 유효 기간. 초 단위이며, `0` 이상의 정수 값을 가집니다. | 필수/항상 |


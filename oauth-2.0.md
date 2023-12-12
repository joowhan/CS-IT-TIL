# Oauth 2.0에 대해

간단 요약: Oauth는 사용자가 비밀번호를 제시하지 않고 다른 웹 사이트에서 현재 사이트의 접근 권한을 위임 받는 것을 의미한다. 관련 URL: https://developers.payco.com/guide 분야: 네트워크 생성 일시: 2023년 11월 7일 오후 11:32 작성: 김주환

## OAuth란?

* 우리는 흔하게 카카오나 구글 소셜 로그인으로 다른 사이트에서 쉽게 로그인, 회원가입을 한다. 연동되는 외부 웹 어플리케이션에서 이런 카카오나 구글이 제공하는 기능을 간편하게 사용할 수 있게 되는데 이때 사용되는 **프로토콜이 OAuth**다.
* OAuth는 사용자들이 비밀번호를 제공하지 않고, 다른 웹 사이트에서 자신들의 정보에 대해 접근 권한을 부여할 수 있는 공통적인 수단이며, 접근 위임을 위한 개방형 표준이다. 다른 플랫폼의 사용자의 데이터에 접근하기 위해 Client(현재 서비스)가 사용자의 접근 권한 위임을 받을 수 있는 표준 프로토콜이다.

### OAuth 구성 요소

| 구분                   | 설명                                                                                                                  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Resource Owner       | 사용자. 개인 정보를 소유하고 있는 주체이며, 여기서 Resource를 개인정보라고 생각하면 된다.                                                             |
| Client               | 자사, 애플리케이션 서버                                                                                                       |
| Authorization Server | 권한을 부여해주는 서버이다. 사용자는 이 서버로 ID, PW를 넘겨 Authorization Code를 발급받고, Client는 Authorization Code를 이 서버로 넘겨 Token을 발급 받는다. |
| Resource Server      | 구글이나 카카오처럼 사용자의 개인 정보를 가지고 있는 서버이다. Client는 Token을 이 서버로 넘겨서 개인정보 응답을 받을 수 있다.                                      |
| Access Token         | 자원에 대한 접근 권한을 Resource Owner가 인가했다는 자격 증명이다.                                                                        |
| Refresh Token        | Access Token의 유효기간을 짧아 시간이 지나면 만료되어 다시 로그인을 해야 한다. 이때 Refresh Token이 있다면 재발급을 받아 재 로그인을 할 필요가 없다.                   |

* 위에 payco의 예시처럼 사용자가 직접 로그인을 하는 프로세스가 아니다. 현재 우리가 쓰고 있는 ‘서비스(Client)’가 사용자를 대신해 로그인 하는데 Resource Server에서 필요한 정보를 얻어 유효성을 판단한다.
* Client는 사용자의 동의를 얻어 개인 정보를 활용하고, Resource Server는 Client를 구분하는 값을 전달하게 된다.

### 승인 과정

1. 사용자는 Client(서비스)를 이용하기 위해 로그인 페이지에 접근한다.
2. 사용자가 구글 소셜 로그인 버튼을 누른다면 특정한 url이 구글 서버로 보내진다.
3. 그럼 Client의 서비스 정보와 서버에 등록된 서비스 정보를 비교한 후 확인이 완료되면 전용 로그인 페이지로 이동하여 사용자에게 보여준다.
4. 사용자가 ID/PW를 입력해서 로그인 하면 Client는 사용하려는 기능들에 대해 사용자에게 동의를 요청한다.
5. 사용자가 동의하면 사용자가 권한을 위임했다는 승인이 Resource server로 보내진다.
6. Resource server도 Client에게 권한 승인을 하기 위해 Authorization code를 사용자에게 보내면 사용자는 그대로 다시 Client에게 보낸다.
7. Client가 Resource Server에게 url(Client ID, PW, Authorization Code)을 보낸다.
8. Resource Server는 Client가 준 정보와 비교해서 일치한다면 Access Token을 발급하고 필요 없어진 Authorization Code를 지운다.
9. 토큰을 받은 Client는 로그인이 완료되었다고 응답한다.
10. Client는 Resource Server의 api를 요청해 사용자의 ID, 프로필 정보를 사용할 수 있다.

### Firebase에서 소셜 로그인 동작 원리

* Firebase Authentication이 위에서 Authorization Server 역할을 한다. 구글, 이메일, Facebook 등의 인증 방식을 자신의 서비스에 쉽게 붙일 수 있도록 도와주는 SaaS 서비스이다.
* 구글 로그인의 경우, SHA1, SHA256 인증서 지문을 등록해주어야 한다.

> **Reference**

[PAYCO 로그인 및 PAYCO 바로가입 개발 가이드](https://developers.payco.com/guide)

[OAuth 2.0 개념과 동작원리](https://hudi.blog/oauth-2.0/)

[🌐 OAuth 2.0 개념 - 그림으로 이해하기 쉽게 설명](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-OAuth-20-%EA%B0%9C%EB%85%90-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)

---
layout: page
title: Keycloak
permalink: /authz/keycloak
category: authz
author: wonchang
---
Keycloak의 페이지를 입맛에 맞게 변경하는 방법을 공부 중… ← Apps/Authentication 을 참조할 것.

https://www.baeldung.com/spring-boot-keycloak  ← 보던 중




KeyCloak은 오픈소스(redhat 자회사인 jboss 의해서 유지보수된다고 한다)

 * 비슷한 다른 서비스
   + Auth0 (Okta)
   + ForgeRock
   + Amazon Cognito


빅테크(구글, 페이스북) 기업들은 자체적으로 개발해서 사용함.


# keycloak 관련해서 확인하던 글들
* https://to-moneyking.tistory.com/m/102  <= 이 글 보던 중
* https://velog.io/@ddkds66/SSO-%EA%B5%AC%EC%B6%95%EA%B8%B0-Authorization-Server-%EC%84%A0%EC%A0%95%EA%B3%BC-%ED%94%84%EB%A1%9C%ED%95%84-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95



# Resources
 * https://www.baeldung.com/spring-boot-keycloak
   + 그래도 최신버전(v20)을 다루고 있음.
 * [AWS 에서 Keycloak 기반 서비스 인증 체계 구축 하기 (1편) - 키클록(Keycloak) 이란?](https://devocean.sk.com/blog/techBoardDetail.do?ID=165131)
   + => 이거 꽤 볼만할 수 있다.


# Udemy Spring Security 강의에서 나온 간단한 시나리오

![](/images/authz/OpenID-introduce03-udemy-eazyBank.png)

Github이나 Facebook, 구글 인증서버같은 경우는 다른 organization에서 소유하고 있기 떄문에,
제한이 있다. eazyBank라는 서비스가 웹서비스, 모바일앱, 마이크로서비스와 같이 발전될 때를 대비해서 자체적으로 인증서를 구축하는 시나리오를 설명하고 있다.

API서비스간의 통신에서, 예를 들면 서드파트 어플리케이션이 eazyBank의 리소스서버(카드, 대출)에 대해서 Rest API 를 호출하는 경우에서는 Client Credentials Grant Type을 이용해야 한다.

end user가 연관되는 유스케이스에서는 Oathorization Code Grant Type을 이용하게 된다.

* 강의 정보
  * https://www.udemy.com/course/spring-security-zero-to-master/learn
  * https://github.com/eazybytes/spring-security <= 강의 소스
  * https://github.com/eazybytes/springsecurity6 <= Spring Boot 3.x (Spring Security 6)

---
# 인증서버가 필요한 이유
패밀리타운 프로젝트에서
타사와의 제휴시에 고정된 인증코드값을 가지고(전달시는 쿼리파라미터로; 전달시는 외부브라우저를 사용함), JWT 토큰을 발급해서 인증처리를 한 적이 있다. 고정된 인증코드값으로 인해 유출시 보안에 취약한 문제가 있고, 이것을 개선하는데 정해진 시간에만 유효하도록 수정하려고 하였으나, 추가적으로 문제점이 있었다.

고정된 인증코드이기 때문에, 복수의 프로젝트에서 인증코드를 처리하는(복호화해서, 복호화한 데이터를 사용하는...) 로직이 들어가게 된 것이다.

그래서 위에서 발견된 보안 취약점을 개선하려고 하면, 복수의 프로젝트에서 중복된 코드를 수정해야되는 상황이 되었다.

중복을 피하기 위해서는 "결국에는" 인증서버가 필요하게 된 것이다.

하지만, 만약에 엔터프라이즈급의 기능을 제공해주는 인증서버가 있다면(오픈소스 or Saas), 그것을 이용하는 것도 효용성이 좋겠다는 생각이 든 계기였다.


---
# 설치

 * KeyCloak 에 대한 기본적인 튜토리얼
   + https://league-cat.tistory.com/397
   + https://jsonobject.tistory.com/445
   + 설치와 기본적인 인증처리 내용


```
bin/kc.sh start-dev --http-port 8180

```



### Using a reverse proxy
https://www.keycloak.org/server/reverseproxy



# KeyCloak 동작 확인

PostMan으로 테스트시에는 PostMan같은 테스트 프로그램을 사용하는 경우가 있는데, 이 경우에는 2개의 다른 어플리케이션이 인증시에 사용하는 Client Credentials Grant Type방식을 이용하게 된다. PostMan에서 이 grant type은 "Service accounts roles"라는 용어로 나오는데, 이것은 2개의 다른 서비스가 인증한다는 의미를 내포하고 있다.

KeyCloak의 관리자페이지에서 사용자를 수동으로 추가할 수 있으나

실제 개발시에는 프론트 페이지의 유저 가입같은 폼에서 KeyCloak 의 Rest API를 호출하는 형식으로 개발하게 됨.



### URL 정보를 확인
* http://localhost:8180/realms/eazybankdev/.well-known/openid-configuration

 * "token_endpoint"
   + URL이 토큰을 발급받는 URL정보
 * "jwks_uri"
   + 리소스 서버가 인증서를 다운받는 URL정보
 * "authorization_endpoint"
   + ahotrization code를 발급해주고, client로 리다이렉트 처리해주는 인증 서버의 URL
   + Authorization Code Grant Type에서 이용되었다.




**Access Token 발급**

PostMan에서 토큰을 발급받기 위해서는 token_endpoint로 x-www-form-urlencoded 형식으로
```
client_id {설정값}
client_secret {설정값}
scope openid email profile address
grant_type client_credentials
```
이 설정정보에 CSRF 토큰이 없는데, 이것은 브라우저를 이용하지 않기 때문이다.(브라우저를 사용하지 않는 경우는 CSRF 공격을 염려할 필요가 없다.)



**Authorizaition Code 발급**

PostMan에서 Authorization Code를 발급받기 위해서는 authorization_endpoint로 param으로
```
client_id {설정값}
response_type code
scope openid
redirect_uri {client의 설정값}
state
```

여기서 state는 CSRF토큰으로 동작하게됨

Authorization Code를 발급받는 처리를 테스트하기 위해서는 리다이렉트 처리가 들어가기 때문에, 브라우저에서 직접 테스트하는게 편하다.

브라우저에서 PostMan이 생성해준 URL을 param포함해서 입력하면, (인증되지 않았기 떄문에) 로그인 페이지로 이동하고, 사용자 정보를 입력하면, redirect_uri로 이동하게 된다.

여기서 Authorization Code를 발급하고 나서 Access Token을 발급받기 위해서는 "token_endpoint"를 이용해야 한다.


PostMan에서 토큰을 발급받기 위해서는 token_endpoint로 x-www-form-urlencoded 형식으로
```
client_id {설정값}
client_secret {설정값}
grant_type authorization_code
code {발급된Authorization Code값}
redirect_uri {client의 설정값}
scope openid
```
위에서 Client Credentials Grant Type 이용시와 다른 점은 grant type이 authorization_code라는 점이다.

# Custom Theme
KeyCloak 가이드의 Server Developer" > "Themes" 를 참조할 것.

가이드에서 테마 생성방법에 대해서 설명해주고 있어서 도움이 된다.

* Keycloak 로그인 페이지 커스텀(Dooray의 문서, jar로 빌드됨, dooray-keycloak-login-page.jar 참조)
  + https://helpdesk.dooray.com/share/pages/9wWo-xwiR66BO5LGshgVTg/2939987857775996519


### 참고할 만한 소스
github.com 에서 "keycloak theme"로 검색할 것.
* https://github.com/keycloakify/keycloakify <= 가장 활성화 되어 있는 얘
* https://github.com/MAXIMUS-DeltaWare/material-keycloak-theme <= 샘플예
* https://github.com/alxrodav/keycloak-theme <= 도커파일 작성예


# Tips

### ssl termination
가끔 tls termination으도 사용됨.

ssl/tls termination이용시는 proxy mode는 "edge" 로 사용할 수 있다.

 * 참조   
   + https://www.keycloak.org/server/reverseproxy

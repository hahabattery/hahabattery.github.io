---
layout: page
title: OAuth
permalink: /authz/
---

# OAuth Authorization Server



# 구축예 - 1
이 구축예에서는 Spring 프레임워크의 Spring Authorization Server와 Redhat SSO의 업스트림 프로젝트로 사용되고 있는 key cloak중에서 안정성과 기능이 다양한 Key Cloack을 사용해서 구축하였다.

* [SSO 구축기 - SSO Model 설계와 SSO, OAuth 2.0, OpenID Connect Protocol의 이해](https://velog.io/@ddkds66/SSO-%EA%B5%AC%EC%B6%95%EA%B8%B0-SSO-Model-%EC%84%A4%EA%B3%84%EC%99%80-SSO-OAuth-2.0-OpenID-Connect-Protocol%EC%9D%98-%EC%9D%B4%ED%95%B4)
* [SSO 구축기 - Authorization Server 선정과 프로필 서버 구축](https://velog.io/@ddkds66/SSO-%EA%B5%AC%EC%B6%95%EA%B8%B0-Authorization-Server-%EC%84%A0%EC%A0%95%EA%B3%BC-%ED%94%84%EB%A1%9C%ED%95%84-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95)


# Spring Authorization Server 를 잉요한 개발

* Simple Single Sign-On with Spring Security OAuth2
  + https://www.baeldung.com/sso-spring-security-oauth2

* Spring 기반 OAuth 2.1 Authorization Server 개발 찍먹해보기
  + https://tech.kakaopay.com/post/spring-oauth2-authorization-server-practice/

* 미디움 Building SSO based on Spring Authorization Server (Part 1 of 3)
  + https://medium.com/@d.snezhinskiy/building-sso-based-on-spring-authorization-server-part-1-of-3-68b3dda053fd


# Legacy Stack
### Spring Security OAuth와 Spring Boot를 이용한 SSO구현 방법 (Legacy stack)
* https://kimseungjae.tistory.com/15
  + OAuth2.0을 이용하고 [@EnableOAuth2Sso](https://docs.spring.io/spring-boot/docs/1.5.7.RELEASE/api/org/springframework/boot/autoconfigure/security/oauth2/client/EnableOAuth2Sso.html) 어노테이션을 이용
  + WebSecurityconfigurerAdapter를 확장하고, 확장한 클래스에 @EnableOAuth2Sso를 붙여두면, 해당 경로에 대대허서는 SSO로써(OAuth로 인증받았으면, 다른 인증이 필요없는) 동작하는 듯하다. 하지만, 문서버전을 보면, 예전 방식인 듯 하다.
  + @EnableOAuth2Sso 는 유용한 거 같으면서도 보안상 위험 할 수도 있을거 같다.

### Spring Security OAuth (legacy stack)
* [Simple Single Sign-On with Spring Security OAuth2 (legacy stack)](https://www.baeldung.com/sso-spring-security-oauth2-legacy)

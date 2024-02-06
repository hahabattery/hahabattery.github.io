---
layout: page
title: IntelliJ
permalink: /tools/editors/intellij
author: wonchang
category: tools
---


 * Cmd + Shift + B 
   + 정의로 이동 (dependency injection된 객체를 생성한 곳으로 이동할 때 유용)

 * Shift + Ctrl + A 나 ⇧⌘A
   * Find Action 처리할 수 있는 액션을 검색

 * Code - Surround With... 메뉴 클릭 (Crtl + Alt + T / ⌥ ⌘ T)
   + 이렇게 해서 try catch를 넣을 수 있다.

---
# 표시 윈도우 이동
 * Command + K
   + Commit side bar
 * Command + 1
   + Project side bar

---
# IntelliJ 가 제공하는 특수한 기능
### VM option
intelliJ에서 VM Option 추가시에 아래형태대로 옵션까지 입력해주어야 한다.
```
-Dspring.profiles.active=dev
```

### 플러그인

 * JPA buddy
 * Key Promoter X <- 단축키를 알려줌
 * GitToolBox <- 라인별로 수정사항 알려줌
 * Maven Helper <- maven dependeny 리스트 표시할 때 유용함.


---
# 자동완성

### 액션 처리
 * 편집기에서 Alt + Enter를 입력하면, 처리 가능한 액션을 실행할 수 있다.
   * 예를 들어서 메소드 정의 자동 생성(만들고 싶은 메소드 작성후 Alt + Enter )
 * Shift + Alt + Enter 를 입력하면, 첫번째 제안을 적용함.

### 메소드 인자 추가
 * 메소드의 인자를 추가시에
   * Ctrl + Space 로 입력이 가능했다.

### code snippet
 * 사용가능한 Code Snippet 확인
   * Ctrl+Alt+S (Setting 메뉴)  >  template 검색  >
   * 필요하면 새로 생성해야 한다.

### Generate
 * Generate
   * Alt + Ins
   * Constructor Injection이나 Setter Injection만들 때도 사용 가능함.

 * 폴더나 패키지 디렉토리 선택후 Alt + Ins 눌러서 생성도 가능하다.

 * Complete Current Statement (작성중인 코드 완성)
   * Ctrl + Shift + Enter
   * ;나 if문의 대괄호 만들 떄 유용

### 테스트

 * 테스트코드 작성
   * Ctrl + Shift + T

 * 테스트코드 <===> 실제 코드 사이에 전환
   * Ctrl + Shift + T

### Override Methods
 * Ctrl + O

### 자주 사용하는 template
 * tdd 테스트케이스
 * soutv println
 * psvm  main 메소드
 * iter  for each 구문
 * fori  for (int i = 0; i < ; i ++)


 * tdd 설정
```
@Test
public void $methodName$() throws Exception {
    //given
    $END$
    //when

    //then
}
```

---
# 코드 이동

 * 함수 코드 순서 이동
   * 함수 상단을 선택후에 Ctrl+Shift+ArrowDown 혹은 ArrowUp


 * Class찾기
   * Ctrl + N
 * 파일 찾기
   * Ctrl + Shift + N or Command + Shift + N

 * Navigate Back / Forward (소스 편집하면서 이동하기 편함)
   * (Windows) Ctrl + Alt + 좌우화살표
   * (Mac) Command + Option + 좌우화살표

 * 최근 파일로 이동시
   * (Windows) Ctrl + E
   * (Mac) Command + E


 * 어떤 항목이 사용된 위치를 검색
   * Alt + F7

### 이동
 * 다음 에러로 이동
   * F2

 * 정의(Declaration)로 이동
   * Ctrl + B

### 괄호로 이동
 * Ctrl + [ or ]
 * Alt + Command + [ or ]

---
# Refactoring

 * 기본적으로 Ctrl + Alt + Shift + T (refactor this)를 누르면 리팩토링 메뉴가 표시된다.

### Inline Variables

 * 변수를 줄여서 코드를 간략히 표현(inline variable)
   * 생략할 변수를 선택하고 Ctrl+Alt+N 으로 간단히 표현 가능.

### Extract (추출)
 * 변수 추출
   * 메서드선택후 Ctrl(Cmd) + Alt + V 하면 리턴타입이 설정됨.
   * 하드코딩된 문자열을 선택후 Ctrl(Cmd) + Alt + V 하면 외부 변수로 분리

 * 메소드의 아규먼트 추출
   * 메소드의 아규먼트 선택후 Ctrl(Cmd) + Alt + V

 * 파라미터 추출하기 (Extract -> Parameter)
   * 파라미터로 추출할 값을 선택후 Ctrl(Cmd) + Alt + p

 * 메서드 추출
   * 함수로 추출할 코드라인을 선택후 Ctrl(Cmd) + Alt + M

 * 이너클래스 추출
   * F6

### Lambda, Stream, etc

 * Lambda => Method Reference
   * Alt + Shift + Enter

---
# 선택
 * 다중선택
   * Alt + Shift 누른 상태에서 마우스 클릭으로 선택

 * Column Selection Mode
   * Alt+Shift+Ins 눌러서 Coumn Selection Mode로 변경후
   * Alt+Shift 누른 상태로 선택후 Ctrl을 누른 상태로 이동,
   * 선택하는 경우에는 Ctrl + Shift 누른채로 이동
   * 인덴트 정리하는 경우에는 Ctrl + BackSapce 로 이동도 가능

---
# 편집

### 복제
 * 해당 라인을 아래에 복사
   * Ctrl+D

### 수정
 * 메소드등 rename
   * Shift+F6 빨간색으로 표시되는 곳에서 수정하면 참조하는 곳도 같이 수정된다.
   * 클래스 java를 잘못 생성한 경우에도 소스에서 Shift+F6로 변경하면 파일 이름까지 같이 변경되었다.
   * 변수명 변경시에 Shift+F6으로 동일한 변수명을 모두 수정가능.

### 대문자로 변경
 * 변경한 부분을 선택한 상태에서 Ctrl + Shift + U



### 코드 가독성
 * reformat
   * Shift+Ctrl(Cmd)+Alt+L

 * 상수에 대해서 접근할 때, static import를 통해서 가독성을 높인다.
   * Alt + Enter Static Import 선택

 * inline variable
   * Ctrl(Cmd) + Alt + N 로 불필요한 변수를 삭제


# 찾기

 * 모든 항목 검색
   * Shift 두번 입력

### 검색

- 모든파일에서 단어 찾기
  - Ctrl + shift + F 기본적으로 프로젝트 파일에서 찾는 것이 디폴트이다.

- 모든파일에서 단어 변경 (replace all)
  - Ctrl + shift + R

- 테스트케이스 작성
  - 클래스나 메소드 위에서 alt + enter > create Test Case

 * 최근에 열었던 리소스 파일 목록
   * Ctrl+E


# 실행

 * 서버 재시작
   * Ctrl + F5

 * Run context configuration from editor (에디터에 열려있는 프로그램이 실행됨)
   * Ctrl + Shift + F10

 * 재실행 (마지막에 실행한 것을 다시 실행)
   * Shift + F10

 * spring devtools가 설치되있는 상태에서
   * Ctrl + Shift + F9 로 현재 파일만 컴파일해서 서버 재시작
   * 이 기능은 java파을은 안되고, html파일같은 경우만 되는듯...
   * 이미 프로그램이 실행중인 상태에서만 의도한 대로 동작하는 듯.

 * 디버그 모드로 실행하기?
   * Shift + F9

### 실행 옵션

 * 프로그램 상단 Edit Configuration 을 눌러서 설정창을 표시하고,
 * VM Options에 -Dspring.profiles.active="dev" 형식으로 입력하면 된다.

### gradlew 명령 사용

 * 프로젝트 디렉토리로 이동해서 gradlew <option> 으로 명령 실행이 가능함.
 * 넣을 수 있는 옵션은 intelliJ 오른편 gradle로 들어가면 확인 가능함.

# 화면 전환

### Hide All Windows

 * 다른 창 숨기기 (코드 창 크게 볼 떄 유용)
   * Ctrl + Shift + F12

 * 창 표시 (특정 윈도우 표시할 때 유용)
   * Alt + 1  (화면에 해당하는 번호, 보통 1번이 화면)


# 빌드

 * spring-boot-devtools가 설치되있는 상태에서 Ctrl+Shift+F9로 해당 html파일만 컴파일 할 수 있다.

# 자주하는 설정
 * 설정 혹은 Preference라고 하기도 함.
   * Ctrl+Alt+S
 * Project Structure
   * Shift+Ctrl+Alt+S


### Gradle 실행시에 느린 문제

 * 설정(Ctrl+Alt+S) > Build,Execution,Deployment > Gralde Projects
   * Build and Rund에서 Gradle로 설정되어있는 것을 IntelliJ로 변경

### lombok 설정

 * start.spring.io에서 롬복 설정후에 IntelliJ에서 따로 설정해야 한다.
   * 설정(Ctrl+Alt+S) > "Annotation Processors" 로 검색 > Enable annotation processing 체크



# 사용 버전의 기능

#### JPA 편의 기능?
 * File > Project Structure > facets > JPA선택
   * 연관관꼐 설정할 때 도움이 되는 듯.


# 도움말

### View parameter hints in a popup
 * Ctrl + F9

### parameter정보
 * Ctrl + P



# Trouble Shooting

### error: unmappable character for encoding MS949

- File > Setting 에서 Encoding으로 검색해서 모두 UTF-8로 변경

- Edit Run Configuration 에서 VM Option 설정에 -Dfile.encoding=UTF-8를 추가

- idea.exe 와 idea64.exe 파일 마지막에 -Dfile.encoding=UTF-8를 추가

- 위에서부터 순서대로 수정

---
layout: page
title: Visual Studio Code
permalink: /tools/editors/vscode
author: wonchang
category: tools
---

# VSCode

 * 파일을 더블 클릭하면, 탭이 고정됨. 한번 클릭하면 클릭때마다 바뀜.

 * ctrl + Enter
   * 아래에 행추가

 * Shift + Alt + Up / Down
   * 현재라인을 위 / 아래에 복사

 * Alt + Up / Down
   * 현재 라인을 위 / 아래로 이동시킴

# 검색
* Cmd + Shift + F
  * 전체 검색
* Cmd + p
  * 파일명 검색

## 수정

### multi line 수정

- Ctrl + Alt 누르고 up down 키로 선택하고, 수정

### multi cursor 수정

- Alt 누르고 마우스로 선택후에 수정

### 같은 문자열을 수정

- Ctrl + F 로 검색후
- Alt + Enter 로 수정

### 같은 문자열을 수정
 * 수정하고자 하는 단어에 커서를 올려놓고 ctrl + f2를 해주면 동일한 이름을 가진 변수들에 커서가 생성됨

### 삭제
 * 한단어 삭제
   * Ctrl + back space


## 선택
 * 다중선택
   * Alt 누른 상태에서 마우스 클릭으로 선택

 * Ctrl + D
   * 커서를 선택하고 Ctrl D 하면 커서위치의 단어와 동일한 내용이 표시됨


## Navigation

 * ctrl+p	빠른 열기, 파일로 이동
   * 파일 검색시 유용

 * CTRL + e 파일 이름의 일부만 입력하여 파일 검색

## 자동완성

## 복사

 * Shift Option Up/Ddown
   + 행을 복사함.

### HTML
 * ! -> tab
   * HTML snippet 생성

 * .box*3
   * 아래 형식의 태그 생성
   ```
   <div class="box"></div>
   <div class="box"></div>
   <div class="box"></div>
   ```
 * li{$}*4
   * 아래 형식의 태그 생성
   ```
   <li>1</li>
   <li>2</li>
   <li>3</li>
   <li>4</li>
   ```

 * .column*2
   * 아래 형식의 태그 생성
   ```
   <div class="row">
   <div class="row">
   ```

 * h1{Welcom To My Course}*3
   * 아래 형식의 태그 생성
   ```
   <h1>Welcom To My Course</h1>
   <h1>Welcom To My Course</h1>
   <h1>Welcom To My Course</h1>
   ```

 * .box.box-$*2
   * 아래 형식의 태그 생성
   ```
   <div class="box box-1"></div>
   <div class="box box-2"></div>
   ```

 * input:checkbox
   * 아래 형식의 태그 생성
   ```
	<input type="checkbox" name="" id="">
   ```





## Code Snippet

 * File -> Preferences -> User Snippets

### Plugins

##### Live Server

 * 기동시키기 Alt + L -> Alt + O


## 설정

 * 좌측 하단의 설정 아이콘 클릭으로 설정이 가능함.
 * tab size


## 유용한 Extension

### live server
 * local에서 server를 돌리도록 도와줌. 자동으로 로딩해주는 기능이 있음.

### bracket pair colorizing
 * bracket의 pair를 맞춰줌

### JavaScript ES6 Snippet

### prettier

 * extension에서 prettier 설치후, setting에서 format on save를 활성화하면 적용됨

##### prettier 설정

- prettier.endOfLine
  - Windows(CRLF)와 Unix(LF)의 개행문자의 차이를 제어하도록 해주는 옵션.
  - 서버 개발시에 windows에서 작성하고 unix에서 사용하는 경우에도 유용하게 사용가능.

# 자주 사용하는 기능

### Formatter

 * 전체
   + Shift + Alt + F  <= 포매팅
   + 혹시, 설정이 안되있으면, extension에서 Prettier Code Formatter를 설치후, File > Preference > Formatter에서 설정

 * 선택한 부분
   + Command + K + F


### Side Bar
 * Ctrl + B 사이드바 보이기

---
layout: page
title: Eclipse
permalink: /tools/editors/eclipse
author: wonchang
category: tools
---

# Eclipse

### 검색

- 현재 소스에서 검색
  - Ctrl + f
- 전체 프로젝트 검색시는 Tab메뉴에서 Search > File 로 검색

- Shift + Ctrl + R (Navigate > Open Resource)
  - Search all resource files (including Java files)
  - 파일명 검색
  
- Shift + Ctrl + T
  - Search all Java classes in classpath

- File Structure Navigate (함수 검색에 편했음)
  - Ctrl + F12
 
- 괄호 쌍으로 이동하기
  - Ctrl + Shift + p 

### Navigation

 * 인터페이스 함수를 선택하고 오른쪽 클릭 > Open Type Hierarchy 를 선택하면, 구현 클래스가 표시됨

 * Ctrl + T 
   * 인터페이스를 구현한 클래스나 함수 찾을 때 사용

### Selection

- alt + shift + A
  - Toggle block selection
  - 여러 라인 선택

### Split view
 * Window>Editor>Toggle Split Editor 에서 설정가능

 * 단축키는 
   * Ctrl + _ for split horizontally, and
   * Ctrl + { for split vertically

### 에러 메세지

- eclipse unmappable character for encoding MS949
- [Window] -> Preferences -> General -> Workspace
  - 아래 하단에 Text file encoding을 Other UTF-8로변경


### Trouble Shooting

##### xml파일에 언어 인코딩 문제

- preference설정에서 인코딩 관련 설정을 UTF-8로 변경

- Runtime Configuration에서 argument설정에 file.encoding=UTF-8 추가

- eclipse.ini 파일에 -Dfile.encoding=UTF-8를 추가



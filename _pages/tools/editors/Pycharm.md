---
layout: page
title: PyCharm
permalink: /tools/editors/pycharm
author: wonchang
category: tools
---

# PyCharm 설정 내용

### Python Interpreter 설정

1. Settings 에서 Python Interpreter를 선택하는 데서 "show all"을 선택하고
2. 좌측 상단 + 버튼을 클릭해서 "Add Local Interpreter"를 선택하고
3. "Pipenv Environment"를 선택하고 로컬에 설치된 "pipenv" 바이너리를 선택해준다.(이때 pipenv가 설치되지 않은 경우는 "brew install pipenv" 같은 명령으로 설치해주어야 한다)
4. 터미널을 실행해서 "pipenv shell" 명령을 실행해서 가상환경을 세팅한다.
5. "pipenv install {패키지명}" 명령으로 패키지를 설치할 수 있다.

###

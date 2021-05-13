# Vue CLI

## 실행 환경

NodeJS와 npm을 설치한다.

npm 모듈 [링크](https://www.npmjs.com/)

### npm 명령어

npm init : 새로운 프로젝트나 패키지 만들 때 사용. package.json이 생성된다.

npm install package : 생성되는 위치에서만 사용가능한 패키지로 설치

npm install -g package : 글로벌 패키지에 추가.



## 모듈 설치

https://www.npmjs.com/ 에서 검색한다. prompty-sync라는 모듈은 다음 명령어를 통해 설치한다.

 ` > npm I prompt-sync`



## @vue/cli

Vue.js에서 공식으로 제공하는 CLI. Vue 프로젝트를 빠르게 구성할 수 있는 스캐폴딩을 제공한다 like framework. 

Vue와 관련된 오픈소스들 대부분이 CLI로 구성이 가능하도록 구현돼있다.

### 실행

설치 : `npm install –g @vue/cli`

확인 : `vue –V or vue --version`

프로젝트 생성 : `vue create <project-name>`

실행 : `npm run serve`


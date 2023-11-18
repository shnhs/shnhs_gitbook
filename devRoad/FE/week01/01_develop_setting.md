# 개발환경 세팅

## 개발환경

모든 개발의 시작은 개발환경 세팅일 것이다.  
개발 환경 세팅이 어려운 이유는 기술 트렌드가 빠르게 변화하기 때문이다.  
기술 트렌드가 빠르게 바뀌고 그에 맞게 도구가 새롭게 여러가지 나오고 그에 따라 유행과 흐름이 바뀐다.  
요즘 유행하는 기술트렌드를 잘 찾아보고 필요에 맞게 바꿀수 있는 능력도 개발자에게 필요할 것이다.

## Node.js

자바스크립트 개발환경 세팅은 Node.js 부터 시작한다. 필요한 버전에 맞게 설치를 진행한다.  
정말 완전 최신의 기능을 필요로하는 것이 아니라면 LTS(Long Term Support) 버전을 사용하는것을 추천한다.  

필요에 의해 여러버전의 Node를 사용할 경우를 위해 fnm과 같은 도구를 사용하여 버전을 관리하자.  

```bash
brew install fnm # Mac OS 기준 Homebrew를 사용하는 환경 기준으로 터미널 작성

fnm install --lts # LTS 버전 node 설치

fnm list # 설치된 node 리스트 확인

node -v # 현재 설정된 node 버전 확인
```

## 프로젝트 초기 설정

npm 명령어를 사용하여 초기 프로젝트를 생성하자.

```bash
npm init -y # 중간에 필요한 질문을 아묻따 yes처리
```

프로젝트를 생성하고 까먹기 전에 `gitignore`를 세팅하자.  
기본적으로 `node_modules` 와 `dist`는 일단 추가한다.

혹은 깃허브에서 제공하는 템플릿을 사용하여 쉽게 설정할수도 있다.  
[https://github.com/github/gitignore]

## TypeScript

생존코스를 진행하면서 타입스크립트를 사용한다.  
타입스크립트는 자바스크립트를 기반으로하여 정적타입 문법을 추가한 자바스크립트이다.  
타입을 명시하여 좀 더 나은 코드 퀄리티와 에러를 사전에 방지할 수 있는 이점이 있다.  

마찬가지로 npm을 사용하여 설치한다.

```bash
npm i -D typescript

npx tsc --init # tsconfig 파일을 만들어준다.
```

여기서는 타입스크립트 패키지를 설치하면서 -D 옵션을 사용하였다.  
해당 옵션의 의미는 개발환경을 위해서만 해당 디펜던시를 추가하겠다는 옵션이다.  
`package.json`을 살펴보면 `devDependencies` 에 추가되는 것을 볼 수 있다.  

타입스크립트로 작성한 코드는 나중에 어쨋든 자바스크립트로 컴파일 되어야 한다.  
이때 컴파일 옵션을 지정해주는 tsconfig.json 파일이 필요하다.  
리액트를 사용할것이므로 jsx 옵션을 'react-jsx'로 설정을 바꿔주자

## ESLint

ESLint는 정적 코드 분석기이다.  
쉽게 말해서 자동으로 코드 패턴을 검사해주고 컴파일 할 때 문제가 없도록 수정해주는 도구이다.  
ESLint를 통해서 스타일 통일 + 잠재적 문제 발견 + 최신 코드 트렌드 반영 등의 이점을 누릴 수 있다.

Prettier와는 사용목적이 다른 것 같다.  
ESLint도 물론 코딩컨벤션 유지에 도움이 되겠지만, 그래도 주 목적은 컴파일 에러 방지를 위한 코드 패턴 유지라면  
Prettier는 줄바꿈, 들여쓰기, 공백 등 일관된 코딩 컨벤션을 위한 도구인 것 같다.

```bash
npm i -D eslint

npx eslint --init # ESLint 초기 세팅. 이것저것 설정해줘가면서 설치 필요
```

eslintignore도 gitignore와 유사하게 설정이 필요하다.  
귀찮다면 똑같이 복붙해도 큰 문제는 없다.

.eslintrc.js에 미리 env파트에 `jest: true`를 잡아두면 좋다.  
React 17버전 이후로 `Import React from 'react';` 없이도 문제없이 코드를 작성할 수 있다.  
ESLint는 별도의 설정없이는 이를 에러로 판단하기 때문에 거슬릴 수 있다.  
해결하기 위해 .eslintrc.js에 rule을 추가한다.

```javascript
{
    rules: {'react/react-in-jsx-scope': 'off'}
}
```

만약 vscode에서 저장할 때 자동으로 eslint를 통한 수정을 하고싶다면,  
setting.json에 아래 내용을 추가한다.

```json
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

## React

개발환경 세팅관련 정리이기 때문에 리액트 관련 개념은 일단 생략한다.

```bash
npm i react react-dom

npm i -D @types/react @types/react-dom
```

2번째 설치 명령어를 보면 앞에 @types 가 붙은 것을 볼 수 있다.  
자바스크립트를 베이스로 만들어진 라이브러리를 타입스크립트로 추론할 수 있도록 만들어진 보조라이브러리를 의미한다.  
때문에 실제 빌드 및 배포 후에는 필요하지 않기 때문에 -D 옵션을 지정하여 설치한다.  

## Jest 설치

Jest는 테스팅 도구이다.  
마찬가지로 세팅관련 정리이기 때문에 개념은 일단 생략한다.  
기본은 아니지만 jest가 swc를 사용하도록 설치를 진행한다.
> swc란? swc는 타입스크립트 컴파일러이다. Rust로 만들어졌으며 속도가 매우빠르다.

```bash
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom@5.16.4
```

그리고 Jest는 기본적으로 타입스크립트를 사용하지 않으므로,
`jest.config.js`에서 타입스크립트를 사용하도록 설정을 잡아준다.

```javascript
module.exports = {
 testEnvironment: 'jsdom',
 setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
 transform: {
  '^.+\\.(t|j)sx?$': [
   '@swc/jest',
   {
    jsc: {
     parser: {
      syntax: 'typescript',
      jsx: true,
      decorators: true,
     },
     transform: {
      react: {
       runtime: 'automatic',
      },
     },
    },
   },
  ],
 },
 testPathIgnorePatterns: ['<rootDir>/node_modules/', '<rootDir>/dist/'],
};
```

## Parcel

Parcel은 모듈번들러이다. 혹은 빌드 툴 이라고 생각할 수도 있다.  
번들러(bundler)란 dependency가 있는 자바스크립트 파일들을  최적화, 압축하여  
하나 혹은 여러개의 static 파일로 빌드해주는 컴파일러이다.  
그리고 그러한 행위를 빌드 라고 한다.

왜 이런게 필요하냐면 쉽게말해서 이전버전 브라우저 및 자바스크립트와  
현대버전의 자바스크립트 사이 호환이 필요하기 때문에 번들가 필요하고 사용한다.

```bash
npm i -D parcel

npm install -D parcel-reporter-static-files-copy # 정적파일 서빙을 위한 추가 패키지
```

Parcel로 빌드하고 정적 서버를 실행시킬것이기 때문에 약간의 세팅이 필요하다.

```json
"source": "./index.html", // package.json의 main을 source로 변경
```

정적 리소스를 위해 .parcelrc 파일을 생성하고 설정을 추가한다.  
아래 설정을 추가하여 static 폴더내 리소스를 정적파일로 사용할 수 있다.

```json
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

## 스크립트 수정

`package.json` 에서 터미널로 편하게 실행할 수 있는 몇가지 스크립트를 추가한다.

```json
{
  ...
  "scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
  ...
}
```

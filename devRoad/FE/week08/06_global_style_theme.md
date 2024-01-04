# 1

## Reset CSS

웹 브라우저 마다 고유한 default 스타일이 적용되어있는 경우가 있다.  
때문에 내가 설정한 스타일이 의도한 대로 보여지지 않을 수가 있다.  

어느 브라우저에서나 동일한, 그리고 내가 의도한 스타일을 보여주기 위해서는  
default값을 초기화하는 작업이 필요하고, 이때 사용할 수 있는게 Reset CSS 이다.

## `box-sizing` 속성

box-sizing CSS 속성은 요소의 너비와 높이를 계산하는 방법을 지정한다.  
요소의 너비나 높이를 줄이거나 늘리거나 혹은 padding을 추가하는 등  
변경이 필요할 때 어떻게 설정되는지를 의미한다.

box-sizing 속성은 대표적으로 아래 두 속성 중 하나를 선택하여 사용한다.  

- `content-box`  
설정하지 않았을 경우 디폴트 값이다. 너비와 높이 속성이 콘텐츠 영역만 포함하고,  
안팎 여백과 테두리는 포함하지 않는다.

- `border-box`  
너비와 높이 속성이 안쪽 여백과 테두리는 포함하고, 바깥쪽 여백은 포함하지 않는다.

## `word-break` 속성  

텍스트가 콘텐츠 박스 밖으로 넘어갈 경우 줄 바꿈 여부를 설정한다.  

- normal  
기본 줄바꿈 규칙을 사용.

- break-all  
강제로 줄바꿈을 한다. 아시아 언어(CJK)에서는 normal과 동일하다.

- keep-all  
문자 쌍 사이에서는 줄바꿈이 되지 않는다. CJK 이외의 언어에서는 normal과 동일하다.  

## Theme

Theme 이란 공통으로 사용한 스타일을 지정하는 방식이다.  
하나의 객체에 스타일을 정의해두는 것이다.

공통으로 사용하는 스타일이 있다면, 각각의 컴포넌트에 정의하기보다는  
하나의 객체를 만들고 해당 파일에서 스타일을 불러가서 사용하는 방식이다.  
추후에 변경이 필요할 때 공통 스타일을 관리하는 한 곳에서만 작업을 하면 되므로 유용하다.

대표적인 예로 다크테마/라이트테마 를 대응하는데 많이 사용된다.

## ThemeProvider

styled-component에서는 ThemeProvider라는 헬퍼 컴포넌트를 사용하여 Theme를 하위에 props로 내려준다.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { ThemeProvider } from "styled-components";
import App from "./App";

const darkTheme = {
  textColor: "whiteSmoke",
  backgroundColor: "#111",
}

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whiteSmoke",
}

ReactDOM.render(
    <React.StrictMode>
        <ThemeProvider theme={darkTheme}>
            <App />
        </ThemeProvider>
    </React.StrictMode>,
    document.getElementById("root")
);
```

props로 내려준 theme을 접근하여 사용할 수 있다.  

```jsx
const Title = styled.h1`
  color: ${props => props.theme.textColor};
`

const Wrapper = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background-color: ${props => props.theme.backgroundColor};
`

function App() {
  return (
    <Wrapper>
      <Title>Hello</Title>
    </Wrapper>
  );
}
```

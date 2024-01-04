# 5

## props

props로 넘겨준 속성값에 따라 원하는 스타일을 잡아 줄 수 있다.

```jsx
import styled from 'styled-components';
 
export const Title = styled.div<{ color?: string }>`
  line-height: 48px;
  color: ${(props) => props.color || '#a2a2a2'};
  font-weight: 600;
  font-size: 1.5rem;
`;

---

<Title color="white">제목</Title>
```

## attrs

말 그대로 attributes를 삽입하기 위한 메서드이며 1개의 객체타입의 arg를 받는다.  
기본 속성을 추가할 수 있다. 불필요하게 반복되는 속성을 처리할 때 유용하다.  
특히 버튼 등을 만들 때 적극 활용할 수 있다.

attrs로 지정한 속성들은 inline-style로 지정되어 들어간다.

```jsx
import styled from 'styled-components';

const Button = styled.button.attrs({
 type: 'button',
})`
 border: 1px solid #888;
 background: transparent;
 cursor: pointer;
`;

export default Button;
```

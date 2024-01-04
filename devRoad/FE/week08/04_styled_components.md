# 4

## styled-componets

가장 대표적인 CSS in JS 라이브러리이다. ES6와 CSS를 사용하여 리액트 컴포넌트를 스타일링하기 위해 사용된다.  
컴포넌트와 스타일 간의 매핑을 제거하고, 스타일을 정의하여 실제로 적용된 리액트 컴포넌트를 생성한다.  

기본적으로 styled-components는 컴포넌트 기반의 리액트에서 CSS 스타일링을 편하게 관리하기 위해서 만들어졌다.  
아래는 공식문서에서 소개하는 장점들이다.

- Automatic critical CSS  
화면에 렌더링 되는 컴포넌트만 추적하여 해당되는 스타일만 자동으로 스타일을 입힐 수 있다.  
최소한의 코드만 로딩하도록 관리할 수 있다.

- No class name bugs  
지정한 스타일에 따라 고유한 클래스 이름을 생성해주므로  
클래스 이름 중복, 오타 등을 걱정하지 않아도 된다.  

- Easier deletion of CSS  
css를 별도로 관리하게되면 해당 css가 쓰이는지 아닌지를 명확하게 관리하기 힘들다.  
스타일이 특정 컴포넌트에 연결되어 있으므로 해당 스타일을 컴포넌트 단위로 사용 혹은 삭제를 관리할 수 있다.

- Simple dynamic styling  
수많은 class를 정의하지 않아도, props를 사용하거나 global theme을 사용하여  
직관적으로 컴포넌트를 유연하게 스타일링 할 수 있다.

- Painless maintenance  
컴포넌트에 영향을 주는 css를 찾기 수월하여, 유지보수가 용이하다.  

- Automatic vendor prefixing  
vendor prefix란, CSS의 실험적인 기능(권고안에 없거나, 있지만 아직 잘 제정되지 않은 기능 등)을 사용할 때,  
각 브라우저에서 사용하는 접두사를 붙여 실험적인 기능임을 알리는 것이다.  
styled-components는 vendor prefix를 알아서 관리해주기 때문에 편리하게 사용할 수 있다.

기본적인 styled component 사용방법은 아래와 같다.

```jsx
import styled from 'styled-components';

const Paragraph = styled.p`
 color: #00F;
`;

export default function Greeting() {
 return (
  <Paragraph>
   Hello, world!
  </Paragraph>
 );
}
```

HTML 요소가 아니라 직접 생성한 styled component를 기반으로 스타일을 추가로 적용할 수 있다.  
. 이 아니라 ( )를 사용하여 작성한다.

```jsx

import styled from 'styled-components';

const Paragraph = styled.p`
 color: #00F;
`;

const BigParagraph = styled(Paragraph)`
 font-size: 5rem;
`;

export default function Greeting() {
 return (
  <BigParagraph>
   Hello, world!
  </BigParagraph>
 );
}
```

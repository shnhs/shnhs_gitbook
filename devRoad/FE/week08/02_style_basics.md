# 2

## className

html에서는 div, span 등의 요소에 class 이름을 붙여서 원하는 스타일을 적용할 수 있다.  
JSX 문법에서는 동일한 기능을 위해 className 속성을 사용한다.  

CSS 클래스를 만들고, 원하는 요소에 className을 지정하여 스타일 적용을 할 수 있다.  

```js
<style>
 .greeting {
  color: #00F;
 }
</style>

---
export default function Greeting() {
 return (
  <p className="greeting">
   Hello, world!
  </p>
 );
}
```

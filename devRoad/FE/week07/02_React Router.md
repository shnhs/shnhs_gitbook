# 2

## 라우터란?

네트워크 장비로써의 라우터는 네트워크 사이의 경로를 결정하고 가장 최적의 경로를 찾아주는 장비를 의미한다.

웹 개발에서의 라우터도 비슷한 역할을 한다.  
클라이언트의 요청 URL을 보고 해당하는 곳으로 전달해주는 역할을 한다.

여러개의 페이지가 있거나, 클라이언트 입력에 따라 페이지를 동적으로 변경해야 하는 경우에  
라우터를 사용하여 적절한 페이지로 이동시킬 수 있다.

## React Router

라우팅 관련 라이브러리 중 가장 많이 사용하는 라이브러리이다.

내부적?으로 “클라이언트 사이트 라우팅” 을 활성화 한다.  
기존 웹사이트에서는 새로운 화면을 보여주려먼 서버에 새롭게 문서를 요청해서 렌더링 해야한다.  
그러나 클라이언트 사이드 라우팅을 사용하면 다시 요청을 보내지 않고,  
링크 클릭등의 형태로 URL을 변경하여 페이지를 업데이트 할 수 있다.

컴포넌트들을 미리 가져와 렌더링 해놓고,  
라우팅된 URL에 맞는 컴포넌트만 다시 렌더링해서 화면에 보여주는 느낌

### Browser Router

HTML5의 history API를 활용하여 라우팅. 최상단 컴포넌트에 지정해야 함

```jsx
import * as React from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';

const root = createRoot(document.getElementById('root'));

root.render(
  <BrowserRouter>{/* The rest of your app goes here */}</BrowserRouter>
);
```

### Route

라우터 생성 함수에 전달되는 객체

path 속성으로 URL을 설정하고, element 속성으로 화면에 보여줄 컴포넌트를 지정할 수 있다.

```jsx
import { Routes, Route } from 'react-router-dom';

function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/about" element={<AboutPage />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}
```

### Memory Router

내부에서 배열을 통해 Location을 저장한다.  
외부 소스와 연결되어 동작하지 않기 때문에 테스트코드 같은 곳에서 사용할 수 있다.

기본값은 [”/”] 이다.

```jsx
describe('App', () => {
  function renderApp(path: string) {
    render(
      <MemoryRouter initialEntries={[path]}>
        <App />
      </MemoryRouter>
    );
  }

  context('when the current path is “/”', () => {
    it('renders the home page', () => {
      renderApp('/');

      screen.getByText(/Hello/);
    });
  });
});
```

### RouterProvider

React Router 6.4부터는 라우터 객체를 만들어서 사용하는 방법을 지원한다.  
라우팅 정보만 별도의 파일로 관리할 수 있다는 장점이 있다.

```jsx
import Layout from '../Layout';
import Main from '../pages/Main';
import PageA from '../pages/PageA';
import PageB from '../pages/PageB';

export const routes = [
  {
    path: '/',
    element: <Layout />,
    children: [
      {
        index: true,
        element: <Main />,
        label: 'main',
      },
      {
        path: '/pageA',
        element: <PageA />,
        label: 'A',
      },
      {
        path: '/pageB',
        element: <PageB />,
        label: 'B',
      },
    ],
  },
];
```

기존 방식처럼 `BrowserRouter` 로 최상위 컴포넌트를 감싸는 방식이 아니라  
`createBrowserRouter` 메서드를 통해 라우터를 생성하고,  
`RouteProvider` 를 이용해 라우터 정보를 전달하고 활성화 시킨다.

```jsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import routes from './routes';

const router = createBrowserRouter(routes);

root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

테스트 환경에서는 `createMemoryRouter` 로 라우터를 만들어서 테스트를 작성할 수 있다.

```jsx
describe('routes', () => {
  function renderRouter(path: string) {
    const router = createMemoryRouter(routes, { initialEntries: [path] });
    render(<RouterProvider router={router} />);
  }

  context('when the current path is “/”', () => {
    it('renders the home page', () => {
      renderRouter('/');

      screen.getByText(/Hello/);
    });
  });
});
```

### Link

클릭하여 다른 주소로 이동 시킬 수 있는 컴포넌트

\<a> 태그는 기본적으로 페이지를 이동하면서 아예 새로운 요청으로 페이지를 불러온다.  
Link 컴포넌트는 내부에서 History API를 사용하여 주소만 변경할 뿐 새로운 요청을 하지는 않는다.

```jsx
function Header() {
  return (
    <header>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

### NavLink

Link 컴포넌트의 특수 버전. 특정 링크에 스타일을 넣어 줄 수 있다.  
`isActive` 속성을 전달 받아서 현재 active된 링크를 선택하여 원하는 스타일을 지정할 수 있다.

### Navigate

렌더링 될 때 현재 위치를 변경하는 컴포넌트

```jsx
import { Navigate } from 'react-router-dom';

export default function LoginPage() {
  return <Navigate to="/" />;
}
```

### useNavigate

양식이 제출되거나 특정 이벤트가 발생했을 경우 특정 페이지로 이동 시킬 수 있는 훅이다.

```jsx
import { useNavigate } from 'react-router-dom';

export default function Footer() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/about'); // /about 으로 현재 위치를 이동
  };

  return (
    <footer>
      <button type="button" onClick={handleClick}>
        Press
      </button>
    </footer>
  );
}
```

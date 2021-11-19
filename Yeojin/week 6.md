# 5장. 리액트 라우터
## 01. 프로젝트 준비 및 기본적인 사용법
```
$ npx create-react-app router-tutorial

$ cd router-tutorial
$ yarn add react-router-dom
```

### 프로젝트에 라우터 적용
src/index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom'; // * BrowserRouter 불러오기
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

// * App 을 BrowserRouter 로 감싸기
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);

serviceWorker.unregister();
```

### 페이지 만들기
웹 사이트에 가장 처음 들어왔을 때 보여줄 Home 컴포넌트와, 웹사이트의 소개를 보여주는 About 페이지
sre/Home.js
```js
import React from 'react';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>이곳은 홈이에요. 가장 먼저 보여지는 페이지죠.</p>
    </div>
  );
};

export default Home;
```
src/About.js
```js
import React from 'react';

const About = () => {
  return (
    <div>
      <h1>소개</h1>
      <p>이 프로젝트는 리액트 라우터 기초를 실습해보는 예제 프로젝트랍니다.</p>
    </div>
  );
};

export default About;
```

### Route: 특정 주소에 컴포넌트 연결하기
사용방식
```
<Route path="주소규칙" component={보여주고싶은 컴포넌트}>
```
src/App.js
```js
import React from 'react';
import { Route } from 'react-router-dom';
import About from './About';
import Home from './Home';

const App = () => {
  return (
    <div>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};

export default App;
```
/about 경로가 / 규칙과도 일치하기 때문에 홈과 소개가 같이 나옴.
이를 고치기 위해선 Home 을 위한 라우트에 exact 라는 props를 true로 설정하면 됨.
src/App.js
```js
import React from 'react';
import { Route } from 'react-router-dom';
import About from './About';
import Home from './Home';

const App = () => {
  return (
    <div>
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};

export default App;
```
이렇게 하면 경로가 완전히 똑같을 때만 컴포넌트를 보여주게 됨

## Link: 누르면 다른 주소로 이동시키기
Link 컴포넌트는 클릭하면 다른 주소로 이동시키는 컴포넌트
리액트 라우터를 사용할 땐 일반 <ahref="...
">...</a> 태그를 사용하면 안됨
만약에 사용한다면 onClick에 e.preventDfault()를 호출하고 따로 자바스크립트로 주소를 변환시켜야 함
대신에 Link라는 컴포넌트를 사용
이유는 a 태그의 기본적인 속성은 페이지를 이동시키면서, 페이지를 아예 새로 불러오게 됨.
그렇게 되면서 우리 리액트 앱이 지니고 있는 상태들도 초기화 되고, 렌더링 된 컴포넌트도 모두 사라지고 새로 렌더링 하게 됨.
Link 컴포넌트는 브라우저의 주소만 바꿀 뿐 페이지를 새로 불러오지는 않음
src/App.js
```js
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};

export default App;
```

## 02. 파라미터와 쿼리
페이지 주소를 정의 할 때, 유동적인 값을 전달해야 할 때도 있음. 이는 파라미터와 쿼리로 나뉘어질 수 있는데
파라미터 : /profiles/velopert
쿼리 : /about?details=true
이것을 사용하는 것에 대하여 무조건 따라야 하는 규칙은 없지만, 일반적으로 파라미터는 특정 id나 이름을 가지고 조회를 할 때 사용하고 쿼리는 어떤 키워드를 검색하거나 요청할 때 필요한 옵션을 전달할 때 사용.

### URL Params
/profile/velopert 이런시긍로 뒷부분에 username 을 넣어줄 때 해당 값을 파라미터로 받아오기
src/Profile.js
```js
import React from 'react';

// 프로필에서 사용 할 데이터
const profileData = {
  velopert: {
    name: '김민준',
    description:
      'Frontend Engineer @ Laftel Inc. 재밌는 것만 골라서 하는 개발자'
  },
  gildong: {
    name: '홍길동',
    description: '전래동화의 주인공'
  }
};

const Profile = ({ match }) => {
  // 파라미터를 받아올 땐 match 안에 들어있는 params 값을 참조합니다.
  const { username } = match.params;
  const profile = profileData[username];
  if (!profile) {
    return <div>존재하지 않는 유저입니다.</div>;
  }
  return (
    <div>
      <h3>
        {username}({profile.name})
      </h3>
      <p>{profile.description}</p>
    </div>
  );
};

export default Profile;
```
파라미터를 바아올 땐 match 안에 들어있는 params 값을 참조
match 객체 안에는 현재의 주소가 Route 컴포넌트에서 정한 규칙과 어떻게 일치하는지에 대한 정보가 들어있음

Profile을 App에서 적용
path 규칙에는 /profiles/:username 이라고 넣어주면 username에 해당하는 값을 파라미터로 넣어주어서 Profile 컴포넌트에서 match props를 통하여 전달받을 수 있게 됨.
src/App.js
```js
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profile from './Profile';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
      <Route path="/profiles/:username" component={Profile} />
    </div>
  );
};

export default App;
```
/profiles/velopert 경로로 직접 입력하여 들어가면 사전에 입력한 데이터들이 나타남

### Query
About 페이지에서 쿼리 받아오기
쿼리는 라우트 컴포넌트에게 props 전달되는 location 객체에 있는 search 값에서 읽어올 수 있음.
location 객체는 현재 앱이 갖고 있는 주소에 대한 정보를 지니고 있음.
```js
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```
여기서 search 값을 확인해야하는데, 이 값은 문자열 형태로 되어있어서 객체 형태로 변환하는건 우리가 직접 해야함

qs 라는 라이브러리를 사용하여 쉽게 할 수 있음
```
$ yarn add qs
```

About 컴포넌트에서 search 값에 있는 detail 값을 받아와서, 해당 값이 true 일 때 추가 정보를 보여주도록 구현
src/About.js
```js
import React from 'react';
import qs from 'qs';

const About = ({ location }) => {
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true
  });
  const detail = query.detail === 'true'; // 쿼리의 파싱결과값은 문자열입니다.

  return (
    <div>
      <h1>소개</h1>
      <p>이 프로젝트는 리액트 라우터 기초를 실습해보는 예제 프로젝트랍니다.</p>
      {detail && <p>추가적인 정보가 어쩌고 저쩌고..</p>}
    </div>
  );
};

export default About;
```
/about?detail=true 경로로 들어가면 추가 정보가 나타남.

## 03. 서브라우트
서브라우트는 라우트 내부의 라우트를 만드는 것을 의미
컴포넌트를 만들어서 그 안에 또 Route 컴포넌트를 렌더링 하면 됨

### 서브 라우트 만들어보기
Profiles라는 컴포넌트를 만들어서, 그 안에 각 유저들의 프로필 링크들과 프로필 라우트를 함께 렌더링
src/Profiles.js
```js
import React from 'react';
import { Link, Route } from 'react-router-dom';
import Profile from './Profile';

const Profiles = () => {
  return (
    <div>
      <h3>유저 목록:</h3>
      <ul>
        <li>
          <Link to="/profiles/velopert">velopert</Link>
        </li>
        <li>
          <Link to="/profiles/gildong">gildong</Link>
        </li>
      </ul>

      <Route
        path="/profiles"
        exact
        render={() => <div>유저를 선택해주세요.</div>}
      />
      <Route path="/profiles/:username" component={Profile} />
    </div>
  );
};

export default Profiles;
```
첫번째 Route 컴포넌트에서는 component 대신에 render가 사용되었는데, 여기서는 컴포넌트가 아니라 JSX 자체를 렌더링 가능
JSX를 렌더링하는 것이기에 상위 영역에서 props나 기타 값들을 필요하면 전달 해 줄 수 있음

App에서 Profiles를 위한 링트와 라우트 생성
```js
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profiles from './Profiles';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profiles">프로필 목록</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
      <Route path="/profiles" component={Profiles} />
    </div>
  );
};

export default App;
```

만약 만들게되는 서비스에서 특정 라우트 내에 탭 같은 것을 만들게 된다면, 단순히 state로 관리하는 것보다 서브 라우트로 관리하는 것을 권장. 
왜냐하면, setState 같은 것을 할 필요도 없고, 링크를 통하여 다른 곳에서 쉽게 진입 시킬 수도 있고, 나중에 검색엔진 크롤링까지 고려한다면, 검색엔진 봇이 더욱 다양한 데이터를 수집해 갈 수 있음

## 04. 리액트 라우터 부가기능
### history 객체
history 객체는 라우트로 사용된 컴포넌트에게 match, location 과 함께 전달되는 props중 하나.
이 객체를 통하여, 우리가 컴포넌트 내에 구현하는 메소드에서 라우터에 직접 접근할 수 있음.
-뒤로가기, 특정 경로로 이동, 이탈 방지등..
src/HistorySample.js
```js
import React, { useEffect } from 'react';

function HistorySample({ history }) {
  const goBack = () => {
    history.goBack();
  };

  const goHome = () => {
    history.push('/');
  };

  useEffect(() => {
    console.log(history);
    const unblock = history.block('정말 떠나실건가요?');
    return () => {
      unblock();
    };
  }, [history]);

  return (
    <div>
      <button onClick={goBack}>뒤로가기</button>
      <button onClick={goHome}>홈으로</button>
    </div>
  );
}

export default HistorySample;
```
src/App.js
```js
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profiles from './Profiles';
import HistorySample from './HistorySample';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profiles">프로필 목록</Link>
        </li>
        <li>
          <Link to="/history">예제</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
      <Route path="/profiles" component={Profiles} />
      <Route path="/history" component={HistorySample} />
    </div>
  );
};

export default App;
```
조건부로 다른 곳으로 이동도 가능하고, 이탈을 메시지 박스를 통하여 막을 수도 있음


### withRouter HoC
라우트 컴포넌트가 아닌 곳에서 match/location/history를 사용해야할 때 쓰면 됨
src/WithRouterSample.js
```js
import React from 'react';
import { withRouter } from 'react-router-dom';
const WithRouterSample = ({ location, match, history }) => {
  return (
    <div>
      <h4>location</h4>
      <textarea value={JSON.stringify(location, null, 2)} readOnly />
      <h4>match</h4>
      <textarea value={JSON.stringify(match, null, 2)} readOnly />
      <button onClick={() => history.push('/')}>홈으로</button>
    </div>
  );
};

export default withRouter(WithRouterSample);
```
src/Profiles.js
```js
import React from 'react';
import { Link, Route } from 'react-router-dom';
import Profile from './Profile';
import WithRouterSample from './WithRouterSample';

const Profiles = () => {
  return (
    <div>
      <h3>유저 목록:</h3>
      <ul>
        <li>
          <Link to="/profiles/velopert">velopert</Link>
        </li>
        <li>
          <Link to="/profiles/gildong">gildong</Link>
        </li>
      </ul>

      <Route
        path="/profiles"
        exact
        render={() => <div>유저를 선택해주세요.</div>}
      />
      <Route path="/profiles/:username" component={Profile} />
      <WithRouterSample />
    </div>
  );
};

export default Profiles;
```
withRouter 를 사용하면, 자신의 부모 컴포넌트 기준의 match 값이 저장됨
현재 gildong 이라는 URL Parms 가 있는 상황임에도 불고하고 params 쪽이 { } 이렇게 비어있음
WithFouterSample은 Profiles에서 렌더링 되었고, 해당 컴포넌트는 /profile 규칙에 일치하기 때문에 그런 결과가 나타남

### Switch
여러 Route들을 감싸서 그 중 규칙이 일치하는 라우트 단 하나만을 렌더링 시킴
Swtich 를 사용하면, 아무것도 일치하지 않았을 때 보여줄 Not Found 페이지를 구현할 수도 있음
App.js
```js
import React from 'react';
import { Switch, Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profiles from './Profiles';
import HistorySample from './HistorySample';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profiles">프로필 목록</Link>
        </li>
        <li>
          <Link to="/history">예제</Link>
        </li>
      </ul>
      <hr />
      <Switch>
        <Route path="/" exact={true} component={Home} />
        <Route path="/about" component={About} />
        <Route path="/profiles" component={Profiles} />
        <Route path="/history" component={HistorySample} />
        <Route
          // path 를 따로 정의하지 않으면 모든 상황에 렌더링됨
          render={({ location }) => (
            <div>
              <h2>이 페이지는 존재하지 않습니다:</h2>
              <p>{location.pathname}</p>
            </div>
          )}
        />
      </Switch>
    </div>
  );
};

export default App;
```

### NavLink
Link(누르면 다른 주소로 이동) 랑 비슷한데, 만약 현재 경로와 Link 에서 시작하는 경로가 일치하는 경우 특정 스타일 혹은 클래스를 적용할 수 있는 컴포넌트
Profiles 에서 사용하는 컴포넌트에서 Link 대신 NavLink 사용
src/Profiles.js
```js
import React from 'react';
import { Route, NavLink } from 'react-router-dom';
import Profile from './Profile';
import WithRouterSample from './WithRouterSample';

const Profiles = () => {
  return (
    <div>
      <h3>유저 목록:</h3>
      <ul>
        <li>
          <NavLink
            to="/profiles/velopert"
            activeStyle={{ background: 'black', color: 'white' }}
          >
            velopert
          </NavLink>
        </li>
        <li>
          <NavLink
            to="/profiles/gildong"
            activeStyle={{ background: 'black', color: 'white' }}
          >
            gildong
          </NavLink>
        </li>
      </ul>

      <Route
        path="/profiles"
        exact
        render={() => <div>유저를 선택해주세요.</div>}
      />
      <Route path="/profiles/:username" component={Profile} />
      <WithRouterSample />
    </div>
  );
};

export default Profiles;
```
만약에 스타일이 아니라 CSS 클래스를 적용하고 싶다면 activeStyle 대신 activeClassName 을 사용하면 됨

### 기타
-Redirect : 페이지를 리디렉트 하는 컴포넌트
-Prompt : 이전에 사용했던 history.block 의 컴포넌트 버전
-Route Config : JSX 형태로 라우트를 선언하는 것이 아닌 Angular 나 Vue 처럼 배열/ 객체를 사용하여 라우트 정의
-Memory Router : 실제로 주소는 존재하지 않는 라우터. 리액트 네이티브나, 임베디드 웹앱에서 사용하면 유용

## useReactRouter Hook 사용하기
withRouter 를 사용하는 대신에 Hook을 사용해서 구현할 수도 있음. 아직은 리액트 라우터에서 공식적인 Hook 지원은 되고 있지 않음
그 전까지는, 다른 라이브러리를 설치해서 Hook 을 사용하역 구현할 수 있음. 
use-react-router 라이브러리 설치
RouterHookSample.js
```js
import useReactRouter from 'use-react-router';

function RouterHookSample() {
  const { history, location, match } = useReactRouter();
  console.log({ history, location, match });
  return null;
}

export default RouterHookSample;
```
Profiles.js
```js
import React from 'react';
import { Route, NavLink } from 'react-router-dom';
import Profile from './Profile';
import WithRouterSample from './WithRouterSample';
import RouterHookSample from './RouterHookSample';

const Profiles = () => {
  return (
    <div>
      <h3>유저 목록:</h3>
      <ul>
        <li>
          <NavLink
            to="/profiles/velopert"
            activeStyle={{ background: 'black', color: 'white' }}
          >
            velopert
          </NavLink>
        </li>
        <li>
          <NavLink
            to="/profiles/gildong"
            activeStyle={{ background: 'black', color: 'white' }}
          >
            gildong
          </NavLink>
        </li>
      </ul>

      <Route
        path="/profiles"
        exact
        render={() => <div>유저를 선택해주세요.</div>}
      />
      <Route path="/profiles/:username" component={Profile} />
      <WithRouterSample />
      <RouterHookSample />
    </div>
  );
};

export default Profiles;
```
하단에 렌더링

콘솔을 확인하면 프로필 목록 페이지를 열었을 때 location, match, history 객체들을 조회할 수 있음.
이 Hook 이 정식 릴리즈는 아니기 때문에 만약 withRouter가 너무 불편하다고 느낄 경우에만 사용하는 것 권장
사용 한다고 해서 나쁠 것은 없지만, 나중에 정식 릴리즈가 나오게 되면 해당 라이브러리를 제거하고 코드를 수정하는 일이 발생할 것.
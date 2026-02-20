---
layout: post
title:  "컨텍스트 API를 사용하여 전역 상태 관리"
date:   2024-02-18 17:08:54 +0900
categories: React
---

## 컨텍스트 API란?

React에서는 상위 컴포넌트에서 하위 컴포넌트로 state을 전달하기 위해
props를 사용해서 하위 컴포넌트로 전달해야 합니다.
props를 사용하지 않고 전역적으로 state를 공유할 수 있게 만들어 주는
기능이 컨텍스트 API입니다.

일반적인 React 상태관리

![일반
상태관리](https://velog.velcdn.com/images/hitzza/post/ae9bcd43-eb6a-45e4-8cd0-75888a14801e/image.png)

컨텍스트를 사용한 전역 상태관리

![컨텍스트
상태관리](https://velog.velcdn.com/images/hitzza/post/3fa0b695-db4b-46a4-befa-ce3f9284d918/image.png)

------------------------------------------------------------------------

## 컨텍스트 API 사용법

컨텍스트를 사용할 때 주로 App 파일에서 컨텍스트 Provider를 설정합니다.

``` js
import React, { createContext } from "react";

const Context = createContext("context");

function App() {
  return (
    <Context.Provider value="Hello World!">
      <Component />
    </Context.Provider>
  );
}

export default App;
```

------------------------------------------------------------------------

## App 파일보다 상위 파일(index)에서 적용하기

App보다 상위 파일인 index에서 컨텍스트를 적용하려면 어떻게 해야 할까?

리액트에서는 값이 변경된 걸 인지하려면 훅을 사용해서 state를 변경해야
합니다.\
훅은 함수형 컴포넌트 내부에서만 사용할 수 있습니다.

App 파일은 함수형 컴포넌트라 문제가 없지만, index 파일에서는 훅을 사용할
수 없습니다.

``` js
// index
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(<App />);
```

그래서 컨텍스트 Provider를 커스텀해서 사용합니다.

------------------------------------------------------------------------

## Context Provider 제작

전역적으로 사용하기 좋은 예시로 로그인 정보를 담는 Provider를
제작합니다.

### UserInfoProvider.tsx

``` js
import { useReducer } from "react";
import { INITUSERINFO } from "../constant/UserInfoInitialState";
import reducer from "./Reducer";
import { 컨텍스트, 컨텍스트_디스패치 } from "./UserInfoContext";

const UserInfoProvider = ({ children }: { children: React.ReactNode }) => {
  const [state, dispatch] = useReducer(reducer, INITUSERINFO);

  return (
    <컨텍스트.Provider value={state}>
      <컨텍스트_디스패치.Provider value={dispatch}>
        {children}
      </컨텍스트_디스패치.Provider>
    </컨텍스트.Provider>
  );
};

export default UserInfoProvider;
```

------------------------------------------------------------------------

## 초기 상태 정의

### UserInfoInitialState.ts

``` js
export const INITUSERINFO = {
  accessToken: "",
  checkIsSignIn: false,
  id: 0,
  image: "",
  introduction: "",
  nickName: "",
};
```

------------------------------------------------------------------------

## 타입 정의

``` js
export type UserInfo = {
  accessToken: string,
  checkIsSignIn: boolean,
  id: number,
  image: string,
  introduction: string,
  nickName: string,
}

export type ActionType =
  | { type: "AUTH", accessToken: string }
  | {
      type: "SET_USERINFO",
      id: number,
      image: string,
      introduction: string,
      nickName: string,
    };
```

------------------------------------------------------------------------

## Context 생성

### UserInfoContext.ts

``` js
import React, { createContext } from "react";
import { ActionType, UserInfo } from "../types/contextUserInfo";

type UserInfoDispatch = React.Dispatch<ActionType>;

export const userInfoStateContext = createContext<UserInfo | undefined>(undefined);
export const userInfoDispatchContext = createContext<UserInfoDispatch | undefined>(undefined);
```

------------------------------------------------------------------------

## Reducer 작성

``` js
import { UserInfo, ActionType } from "../types/contextUserInfo";

const reducer = (state: UserInfo, action: ActionType): UserInfo => {
  switch (action.type) {
    case "AUTH":
      return {
        ...state,
        accessToken: action.accessToken,
        checkIsSignIn: true,
      };
    case "SET_USERINFO":
      return {
        ...state,
        id: action.id,
        image: action.image,
        introduction: action.introduction,
        nickName: action.nickName,
      };
    default:
      return state;
  }
};

export default reducer;
```

------------------------------------------------------------------------

## 최종 Provider

``` js
import { useReducer } from "react";
import { INITUSERINFO } from "../constant/UserInfoInitialState";
import reducer from "./Reducer";
import {
  userInfoStateContext,
  userInfoDispatchContext,
} from "./UserInfoContext";

const UserInfoProvider = ({ children }: { children: React.ReactNode }) => {
  const [state, dispatch] = useReducer(reducer, INITUSERINFO);

  return (
    <userInfoStateContext.Provider value={state}>
      <userInfoDispatchContext.Provider value={dispatch}>
        {children}
      </userInfoDispatchContext.Provider>
    </userInfoStateContext.Provider>
  );
};

export default UserInfoProvider;
```

------------------------------------------------------------------------

이렇게 완성된 Context Provider를 index에서 import 해서 사용하면 전역
상태 관리가 가능합니다.

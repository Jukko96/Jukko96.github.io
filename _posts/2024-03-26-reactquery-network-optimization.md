---
layout: post
title:  "ReactQuery를 사용한 네트워크 요청 수 줄이기"
date:   2024-03-26 17:08:54 +0900
categories: ReactQuery 
---



이번 포스트에서는 ReactQuery를 사용하여 네트워크 요청 수를
줄여보겠습니다. 개발 환경은 Next.js에서 Axios를 사용하여 개발하였습니다.

------------------------------------------------------------------------

## ReactQuery란?

ReactQuery는 React에서 데이터를 관리하기 위한 라이브러리입니다.\
주로 API 호출 결과나 캐시된 데이터와 같은 비동기 데이터를 처리하는 데
사용됩니다.

### 장점

-   API 호출 결과 자동 캐싱
-   이전 데이터 재사용 가능
-   Mutation을 통한 생성/수정/삭제 처리 용이

------------------------------------------------------------------------

## 설치 방법

``` bash
npm i @tanstack/react-query
# or
pnpm add @tanstack/react-query
# or
yarn add @tanstack/react-query
# or
bun add @tanstack/react-query
```

------------------------------------------------------------------------

## Provider 설정

### React (App.jsx)

``` javascript
import {
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'

const queryClient = new QueryClient()

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <AnyComponents />
    </QueryClientProvider>
  )
}
```

------------------------------------------------------------------------

## 프로젝트 적용 사례

### 문제 상황

![network-before](https://velog.velcdn.com/images/hitzza/post/59d01de4-084d-44af-bfc4-c710b8e7b947/image.gif)

------------------------------------------------------------------------

### 수정 결과

![network-after](https://velog.velcdn.com/images/hitzza/post/34d581e0-6d72-4e8b-bf3b-9c05f4d5dd76/image.gif)

------------------------------------------------------------------------

## 정리

✔ useQuery는 기본적으로 stale\
✔ staleTime을 설정해야 캐싱 효과 발생\
✔ 비동기 state 업데이트는 useEffect에서 처리

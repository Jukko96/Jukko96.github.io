---
layout: post
title:  "[와글와글 프로젝트]Lighthouse를 사용하여 웹사이트 최적화(2)"
date:   2024-03-21 17:08:54 +0900
categories: 와글와글 Lighthouse
---


첫 Lighthouse를 사용해서 나온 결과입니다.\
![lighthouse-result](https://velog.velcdn.com/images/hitzza/post/6fe2abee-2535-41e4-8e7a-ccee9cb69a2d/image.png)

Performance 점수가 너무 낮게 나와서 가장 큰 문제가 어떤 것인지 확인해
보았습니다.\
![performance-detail](https://velog.velcdn.com/images/hitzza/post/aa920436-e346-4301-b19c-3dd46fa0af3b/image.png)

## 주요 문제점

1.  **Minimize main thread work 3.9s**\
    메인 스레드의 블로킹 시간이 3.9초 발생

2.  **Largest Contentful Paint element 24,590ms**\
    가장 큰 요소를 그리는 데 약 24초 소요

3.  **Reduce JavaScript execution time 2.5s**\
    자바스크립트 실행에 2.5초 소요 (2초 이상 시 경고)

이번 글에서는 **Largest Contentful Paint(LCP)** 를 우선적으로
개선합니다.

------------------------------------------------------------------------

# LCP를 줄이는 방법

LCP 측정 기준:\
https://developer.chrome.com/docs/lighthouse/performance/lighthouse-largest-contentful-paint/

![lcp-guide](https://velog.velcdn.com/images/hitzza/post/16e9a30b-9188-436f-b121-8efbc2e69cea/image.png)

저의 LCP 결과:

![lcp-result](https://velog.velcdn.com/images/hitzza/post/373bf1c3-b051-4a1d-a9ff-c9a093c6d95b/image.png)

Render Delay가 비정상적으로 높았습니다.\
이는 **LCP 리소스 로드 완료 시점부터 실제 렌더링까지의 지연
시간**입니다.

------------------------------------------------------------------------

## 원인 분석

해당 컴포넌트는 서버 컴포넌트였습니다.

``` javascript
export default async function Component{
  const fetchData = async () => {
    try {
      const {data} = await api.get('url')
      return data
    }
  }
  const postData = await fetchData()
}

return <Post data={postData}/>
```

### React vs Next.js 차이

-   **React (CSR)**\
    초기 렌더링 → 비동기 데이터 수신 → setState → 리렌더링

-   **Next.js (SSR)**\
    서버에서 async 데이터 fetching 완료까지 기다린 뒤\
    완전한 HTML 생성 → 클라이언트 전송

따라서 데이터 fetching 동안 서버가 대기하면서 Render Delay가 크게
발생했습니다.

------------------------------------------------------------------------

# 해결 방법: Lazy Loading

Lazy Loading은 초기 로딩 시 중요하지 않은 리소스를 나중에 불러오는
방식입니다.

Next.js에서는 `next/dynamic`을 사용합니다.

``` javascript
import Component from './Component'
import dynamic from 'next/dynamic'

export default function TestComponent(){
  const LazyComponent = dynamic(() => import('./LazyComponent'))

  return (
    <>
      <Component/>
      <LazyComponent/>
    </>
  )
}
```

------------------------------------------------------------------------

## 결과

![render-delay-improved](https://velog.velcdn.com/images/hitzza/post/c8940cca-aa1d-4bd0-8862-58bd76c6fdaa/image.png)

**42,250ms → 2,570ms**

Render Delay가 크게 감소했습니다.

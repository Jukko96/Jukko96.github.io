---
layout: post
title:  "[와글와글 프로젝트]Lighthouse를 사용하여 웹사이트 최적화(1)"
date:   2024-03-21 17:08:54 +0900
categories: 와글와글 Lighthouse
---


[1. Lighthouse란?](#1-lighthouse란)\
[2. Lighthouse 설치 방법](#2-lighthouse-설치-방법)\
[3. Lighthouse의 성능 측정 기준](#3-lighthouse의-성능-측정-기준)\
[4. Performance 평가 항목](#4-performance-평가-항목)\
[5. 총 성능 점수](#5-총-성능-점수)

------------------------------------------------------------------------

현재 포스팅은 Lighthouse에 대해서 다루고 있습니다. 최적화 작업은 2번째
글부터 읽어주세요!

------------------------------------------------------------------------

## 1. Lighthouse란?

Lighthouse란 구글에서 개발한 웹 페이지의 성능, 품질 및 정확성을 모범사례
준수 여부를 평가하기 위한 오픈소스 형태의 자동화 도구입니다.\
Lighthouse를 사용하여 여러 가지 측정 항목에 대한 점수를 부여해서 현재
사이트의 상태를 판단할 수 있습니다.

------------------------------------------------------------------------

## 2. Lighthouse 설치 방법

Lighthouse는 구글 크롬의 extension으로 확장 프로그램만 설치하면 간단하게
사용하실 수 있습니다.

Lighthouse 다운로드 페이지에서 Lighthouse 확장 프로그램을 설치하신 후,
개발자 도구로 이동하셔서 Lighthouse를 클릭하시면 바로 사용이 가능합니다.

단축키\
- mac: command + option + i\
- windows: F12

### Lighthouse 탭

![lighthouse-tab](https://velog.velcdn.com/images/hitzza/post/f4a82d2f-cc4d-49ed-a94a-95e59ca772bd/image.png)

### Analyze page load

![analyze](https://velog.velcdn.com/images/hitzza/post/408a0002-082e-4d5e-86b0-36333b53506e/image.png)

------------------------------------------------------------------------

## 3. Lighthouse의 성능 측정 기준

1.  성능 (Performance)\
    페이지의 로딩 속도, 렌더링 시간 및 다른 성능 관련 지표를 평가합니다.

2.  접근성 (Accessibility)\
    웹 페이지가 장애인이나 기술적 제한을 가진 사용자들에게 적합한지를
    평가합니다.

3.  최적화 (Best Practices)\
    웹 페이지의 코드 품질, 보안 및 모범 사례 준수 여부를 확인합니다.

4.  SEO (Search Engine Optimization)\
    검색 엔진 최적화를 위한 관련 지표를 제공합니다.

------------------------------------------------------------------------

## 4. Performance 평가 항목

### 1) Largest Contentful Paint (LCP)

LCP는 사용자가 웹 페이지를 처음 방문했을 때 화면에 가장 큰 이미지나
텍스트 블록이 렌더링 되는 시간입니다.\
LCP는 2.5초 이내가 권장됩니다.

LCP에서 고려하는 요소\
- `<img>` 요소\
- `<svg>` 내부 `<image>` 요소\
- `<video>` 요소\
- background-image 요소\
- 텍스트 블록 요소

------------------------------------------------------------------------

### 2) Total Blocking Time (TBT)

TBT는 자바스크립트 메인 스레드 작업으로 인해 다른 작업이 차단되는 시간을
의미합니다.\
기준은 50ms이며 Lighthouse는 70ms 작업을 감지하면 20ms 차단으로
계산합니다.

------------------------------------------------------------------------

### 3) Cumulative Layout Shift (CLS)

CLS는 예상치 못한 레이아웃 이동으로 인한 사용자 경험 문제를 점수화한
지표입니다.

------------------------------------------------------------------------

### 4) First Contentful Paint (FCP)

FCP는 브라우저가 DOM 콘텐츠의 첫 번째 요소를 렌더링하는 시간을
측정합니다.

------------------------------------------------------------------------

### 5) Speed Index (SI)

SI는 페이지 로드 중 콘텐츠가 시각적으로 표시되는 속도를 측정합니다.

#### 프로젝트 SI 검사 결과

![si](https://velog.velcdn.com/images/hitzza/post/639df99b-eab7-463f-b8c9-64cf93eab171/image.png)

------------------------------------------------------------------------

## 5. 총 성능 점수

Lighthouse v10부터는 Time to Interactive Metric이 제거되고 Metric
가중치가 변경되었습니다.

![metric](https://velog.velcdn.com/images/hitzza/post/eeb5733e-3ec8-40b8-94b4-61f51d76a88b/image.png)

5가지 Metric 점수의 가중 평균으로 총 점수가 계산됩니다.

------------------------------------------------------------------------

와글와글 프로젝트 메인 페이지 성능 측정 결과

![score](https://velog.velcdn.com/images/hitzza/post/fbb134cd-d8c7-442b-827f-08500ddff152/image.png)

생각보다 낮은 점수가 나왔습니다.

다음 포스팅에서는 Performance 점수를 높이는 방법을 다룰 예정입니다.

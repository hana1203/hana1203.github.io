---
title: "express 서버 query parameter vs path parameter | req.query vs req.params"
layout: post
categories: node.js
---

## Node.js Express 프레임워크로 StatesAirlines 서버 구현하기

#### \* 과제 목표

클라이언트 요청에 따라 항공편, 예약 데이터를 CRUD (조회, 생성, 수정, 삭제)하는 기능을 만든다.

#### \* 구조

http://localhost:3001 로 들어가면 제대로 들어온게 맞다는 welcome! 페이지가 띄어짐

이미 주어진 과제 파일에 라우터로 설정이 되었기 때문에 웹사이트는 각 3부분으로 분기가 된다.

> /airport

> /flight

> /book

url endpoint로 쳐서 들어가면 각각 공항 정보, 항공편 정보, 부킹 정보를 볼 수가 있는 것

공항정보를 가져오는 기능은 모두 구현이 되어있었지만, 나머지 flight와 book 에 대한 서버 기능을 만드는게 이번 과제였다.

#### \* 어려웠던 점

query가 뭔지 감으로만 대충 알고 request, response 가 정확히 무엇인지.. express 공식문서를 봐도 개념이 잘 정리가 되지않아 과제를 하는데 매우 헤맸다. @\_@

혼자 복습을 하면서 콘솔에 모두 찍어보면서 확인을 하니 이제 조금은 알 것 같다!

---

### Airport 공항이름 자동완성

Request

> GET /airport?query={query}

C가 들어있는 공항 코드 검색

- `/airport?query=c`

```
console.log(req.query);
//{ query: 'c' }
```

```
console.log(req.query.query);
//c
```

```
console.log(list); //res.json()으로 보내는 데이터
[
    { name: '제주', code: 'CJU' },
    { name: '인천', code: 'ICN' },
    { name: '세부', code: 'CEB' }
]
```

### Flight 항공편 조회

Request

> GET /flight

출발시간이 oo이고 도착시간이 oo 으로 검색

출발지 oo 도착지 oo 으로 검색

- `/flight?departure_times=2021-12-02T12:00:00&arrival_times=2021-12-03T12:00:00`

- `/flight?departure=ICN&destination=CJU`

```
console.log(req.query);
//{  departure: 'ICN', destination: 'CJU' }
```

### Flight uuid로 항공편 조회

Request

> GET /flight/{:id}

uuid값을 가진 모든 항공편 조회

- `/flight/af6fa55c-da65-47dd-af23-578fdba40bed`

```
console.log(req.params);
//{ id: 'af6fa55c-da65-47dd-af23-578fdba40bed' }
```

```
console.log(req.body);
//{}
```

---

## 정리

## query parameter

/airport? /flight? 처럼 endpoint에 물음표 이후에 나오는 값을 query string이라고 한다.

`/flight?departure=ICN`

보통 필터링을 해서 값을 가져오는 경우 query parameter 사용이 적합하다.

`req.query`

이때 uri 의 query string을 가져오려면 req.query를 사용한다.

## path parameter

`/flight/af6fa55c-da65-47dd-af23-578fdba40bed`

id와 같이 유일한 값을 가진 데이터는 path parameter로 물음표 없이, 주소 / 이후에 바로 넣어서 사용한다.

`req.params`

입력된 path parameter의 값을 가져오고 싶으면 req.params를 사용한다

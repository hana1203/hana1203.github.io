---
title: "express 서버 post, put 요청하기 | POSTMAN 활용"
layout: post
categories: node.js
---

GET 요청을 할땐 웹 브라우저 주소창에서 직접 치면서 바로 어떤 요청이가고 어떤 응답이 오는지 데이터를 직접 눈으로 확인이 되었는데..

<img width="694" alt="image" src="https://user-images.githubusercontent.com/92300124/175957894-9fac2961-8e35-4b83-b440-f6fb01cafed9.png">

POST, PUT 을 요청하려니까 문제가 생겼다.

POST는 실제로 데이터를 요청해서 req.body 내용을 저장해야되는데 도대체 이걸 어디서 요청을 보내고 어떻게 확인한다는 거지?

웹 브라우저 주소창에서 할 수 있는건 GET 요청밖에 할수가 없었다. 아무리 주소창에 백날 POST 요청 uri를 쳐도 아무일도 일어나지 않았다..

정답은 Postman 활용이었다. 불과 몇주 전에 기본 실습해보고 그냥 그런앤가보다 했는데 이럴때 쓰는 거였다!

POSTMAN은 API 테스트 도구로 HTTP 요청을 쉽게 눈으로 볼수 있게 도와주는 그런 아이다.

## POST 요청

<img width="1067" alt="image" src="https://user-images.githubusercontent.com/92300124/175963604-a12d1be8-fb5c-4c5c-bcd0-06ee157bbc31.png">

여기서 주의할 점은, 실제로 post 요청하는 데이터를 body에 json 형식으로 손수 넣어준다!

> [POST] /book

{% highlight javascript %}
let booking = [];
...
create: (req, res) => {
// TODO:
const bookData = req.body;
booking.push(bookData);
return res.status(201).json(bookData);
},
{% endhighlight javascript %}

```
console.log(bookData);
//{ flight_uuid: 'KE1909', name: '에밀리', phone: '010-1111-2222' }
```

### PUT 요청

<img width="1074" alt="image" src="https://user-images.githubusercontent.com/92300124/175959855-aa788207-935b-4484-85ad-08d7af585155.png">

마찬가지로 put 요청하는 데이터를 body에 제이슨 형식으로 넣는다.

> [PUT] /flight/:id

요청된 id 값과 동일한 uuid값을 가진 객체를 찾아서 그 객체의 데이터를, 요청된 body의 데이터로 수정한다.

즉, id값을 uri에 넣고, body에 수정할 내용을 넣은 다음에 put 요청을 날리면 이제 그 데이터는 body에서 요청하는 데이터에 맞게 덮어쓰기가 된다.

```
   console.log(updatedFlight); //uuid 찝어서 req.body로 보내는 데이터
    // {
    //   uuid: 'af6fa55c-da65-47dd-af23-578fdba99bed',
    //   departure: 'ICN',
    //   destination: 'CJU',
    //   departure_times: '2021-12-02T11:00:00',
    //   arrival_times: '2021-12-04T15:00:00'
    // }
```

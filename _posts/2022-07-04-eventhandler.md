---
title: "react 이벤트 핸들러 함수 등록시 parameter 전달하기 "
layout: post
categories: react
---

### Event handler?

이벤트 핸들러 event handler는 이벤트가 발생했을 때 자신은 호출을 안하고 브라우저한테 호출을 떠넘긴다(위임한다).

### 이벤트 핸들러 등록

이벤트 발생시 브라우저에게 이벤트 핸들러 호출을 떠넘기는 걸 이벤트 핸들러를 등록한다고 한다.

중요한 점은 이벤트 핸들러 함수를 등록할때는 함수를 호출하는 호출문이 아닌 함수 자체를 등록해야 된다. 함수 자체!!!

왜냐면 콜백 함수와 마찬가지로 함수 참조를 등록해야지 브라우저가 이벤트 핸들러를 호출할 수 있다고 한다.

만약 함수 참조가 아닌 함수 호출문 자체를 등록해버리면 함수 호출문의 평가 결과가 이벤트 핸들러로 등록된다고 한다. (모던 자바스크립트 딥다이브 758pg)

이번 react custom component 과제를 하면서 이벤트 핸들러 함수를 등록할때 함수에 parameter가 있을 때와 없을 때의 사용법이 달랐다.

모든 경우의 수를 넣어가면서 ㅋㅋㅋ 작동하는지 확인해봤는데 너무너무 헷갈렸으므로 정리하는 시간을 가져야겠다.

## 1. No Parameter 매개변수 X

{% highlight javascript %}
const openModalHandler = () => {
// TODO : isOpen의 상태를 변경하는 메소드를 구현합니다.
setIsOpen(!isOpen);
};
return (
...
<ModalBtn onClick={openModalHandler}
// TODO : 클릭하면 Modal이 열린 상태(isOpen)를 boolean 타입으로 변경하는 메소드가 실행되어야 합니다. >
</ModalBtn>
)
{% endhighlight javascript %}

## 2. parameter가 그냥 변수

{% highlight javascript %}
const selectMenuHandler = (index) => {
// TIP: parameter로 현재 선택한 인덱스 값을 전달해야 하며, 이벤트 객체(event)는 쓰지 않습니다
// TODO : 해당 함수가 실행되면 현재 선택된 Tab Menu 가 갱신되도록 함수를 완성하세요.
setCurrentTab(index);
};
return (
...
{menuArr.map((menu, index) => (

<li onClick={() => selectMenuHandler(index)}>
</li>
 ))}
 )
{% endhighlight javascript %}

## 3. parameter가 이벤트 객체

{% highlight javascript %}
const addTags = (event) => {
// TODO : tags 배열 에 새로운 태그를 추가하는 메소드를 완성하세요.
// 이 메소드는 태그 추가 외에도 아래 3 가지 기능을 수행할 수 있어야 합니다.
// - 이미 입력되어 있는 태그인지 검사하여 이미 있는 태그라면 추가하지 말기
// - 아무것도 입력하지 않은 채 Enter 키 입력시 메소드 실행하지 말기
// - 태그가 추가되면 input 창 비우기
if (tags.includes(event.target.value)) {
//이미 입력되어있는 태그이면 추가안하기
return null;
} else if (event.target.value.length === 0) {
setTags(tags); //아무런 입력 없으면, 빈 문자열이면 아무것도 하지마!
} else if (event.key === "Enter") {
setTags([...tags, event.target.value]);
event.target.value = ""; //태그 추가되면 input창 비우기
}
};

return (
...
<input onKeyUp={(event) => addTags(event)} />
)
{% endhighlight javascript %}

---
title: "array.splice()로 배열 안의 특정 요소 제거 및 변경"
layout: post
categories: javascript
---

# Array.prototyple.splice()

배열에서 어떤 요소를 제거하고 싶을때 나한테 늘 처음 떠오르는 아이디어는 array.pop() 인데, pop() 메서드는 맨 마지막 요소를 제거한다.

그렇다면 특정 요소만 없애고 싶은데 그럴땐 무슨 메서드를 써야되지? filter 쓰는 것 말고.

같은 문제에 직면했을때 늘 js array remove specific element 을 구글링하는 내 자신 발견 🤣

splice()를 자유자재로 쓰지 못하고 있던 것 같아 이번 기회에 splice()를 정리하는 타임~~

## splice(start, [deleteCount], [items])

### 첫번째 인자만 사용해서 start 부터 이후 요소까지 모두 삭제

{% highlight javascript %}
const arr = [1, 2, 3, 4];
const result = arr.splice(1);
console.log(arr); //[1] //인덱스 1부터 모두 제거하고 변경된 원본 배열
console.log(result); //[2, 3, 4] //제거된 요소
{% endhighlight javascript %}

### start에 삭제를 원하는 배열 인덱스 넣고 deleteCount에는 몇 개 삭제할 건지 넣기

{% highlight javascript %}
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 1);
console.log(arr); //[1, 3, 4] //인덱스 자리 1에서 1개 제거한 이후의 배열
console.log(result); //[2] //제거된 요소
{% endhighlight javascript %}

### 세번째 인자에 items 까지 적어주면 새로운 요소를 삽입해주어 배열 변경 가능

{% highlight javascript %}
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 1, 100);
console.log(arr); //[1, 100, 3, 4] //인덱스 1에서 1개를 제거한 다음에 새로운 요소를 넣는다.
console.log(result); //[2] //제거된 요소
{% endhighlight javascript %}

- 원본 배열이 직접 변경된다. Mutable하다.
- 두번째 인자로 제거할 요소의 개수만큼 원본 배열에서 요소를 제거할 수 있다.
- 세번째 인자로 새로운 요소도 얼마든지 넣어서 원본 배열을 변경할 수 있다.

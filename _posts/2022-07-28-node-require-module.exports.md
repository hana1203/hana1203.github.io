---
title: "node.js require(), module.exports로 다른파일에 있는 데이터 가져오기 | feat.구조분해할당"
layout: post
categories: node.js
---

## 나의 시도와 error ⚠️

data.js 파일에는 discussions 라는 배열 타입의 데이터가 있고, 이 데이터를 main.js 에 가져와서 쓰고 싶었다.

{% highlight javascript %}
//data.js
const discussions = [{id:"",title:"",url:""},{},{},...];
...
module.exports = discussions
{% endhighlight javascript %}

{% highlight javascript %}
//main.js
const { discussions } = require('./data.js')
{% endhighlight javascript %}

그런데 이렇게해서는 main.js에서 referenceError: document is not defined 에러가 계속 발생했다.
main.js에서 data.js를 통해 불러오는 변수한테 계속 레퍼런스 에러가 뜬 것이다.
당연히 data.js에 있는 데이터들이 불러와지지 않았다. (이유를 몰랐던 어제만해도 도대체 왜 안불러와지는지 몰랐지만 🥲)

오늘 무심코썼던 require() 함수와 module.exports에 대해 파헤쳐보았고 오랜만에 골똘히 생각하는 시간을 가졌던 것 같다.


## module.exports 로 모듈 내보내기

> The default value of module.exports object is {} (empty object). 

module.exports의 기본값은 빈 객체이다. 


## require() 함수로 다른 파일에 있는 모듈 가져오기

> The builtin require function is the easiest way to include modules that exist in separate files. The basic functionality of require is that it reads a JavaScript file, executes the file, and then proceeds to return the exports object.
<https://nodejs.org/en/knowledge/getting-started/what-is-require/>

노드 공식문서에 따르면 다른 파일에 있는 모듈을 가져오기 위해서는 require() 함수를 사용하고, require() 함수는 exports 객체를 반환한다. 

---

## 해결

## 변수에 대입해놓은 모듈을 내보내고 가져오기

내 에러코드에서의 module.exports를 콘솔에 찍어보면, [{},{}...] 배열 데이터 자체가 찍힌다. 

{% highlight javascript %}
//data.js
const discussions = [{id:"",title:"",url:""},{},{},...]
...
module.exports = discussions
console.log(module.exports)
//[{id:"",title:"",url:""},{},{},...]
{% endhighlight javascript %}

discussions라는 배열 데이터 자체를 module.exports 에 그대로 대입해서 할당했고, 이를 내보냈기 때문.
그래서 이 상태에서 main.js에서 require()로 가져오려면...

{% highlight javascript %}
//main.js
const discussions = require('./data.js')
{% endhighlight javascript %}

객체에 담긴 { discussions } 이 아닌, discussions 변수 자체를 가져오면 잘 작동한다.
curly braces 하나 차이였다니...!!! 사소한 차이지만 원리를 모르면 역시 컴퓨터는 제대로 돌아가지 않는다는 걸 다시 한번 느낌..


## 구조분해할당으로 require() 모듈 가져오기

그렇다면.. 객체에 담긴 const { discussions } = require('./data.js') 는 언제 쓰는 것인지?
그냥 변수로 불러올 때와 차이점은 무엇인지 매우 헷갈리기 시작했다. 

이렇게 구조분해할당을 사용해서 {} = require() 으로 가져와야 되는 경우에는,
module.exports로 내보낼때 바로 module.exports.변수 (여기선 discussions)로 사용하는 경우이다.

{% highlight javascript %}
//data.js
module.exports.discussions = [{id:"",title:"",url:""},{},{},...]
console.log(module.exports)
//{discussions: [{id:"",title:"",url:""},{},{},...]}
{% endhighlight javascript %}

이렇게 되면, module.exports의 기본값은 원래 빈 객체였는데, key에 변수명인 discussions이 담기고, 해당하는 데이터 내용이 value에 담겨있는 객체로 변경된 것이다.

{% highlight javascript %}
//main.js
const { discussions } = require('./data.js')
{% endhighlight javascript %}

따라서 이 모듈을 require()로 가져와야할때, module.exports 객체 {discussions: [{id:"",title:"",url:""},{},{},...]} 에서 내가 원하는건 discussions라는 key 이름을 가진 value 값을 가져오고 싶은거니깐 이때 구조분해할당을 활용해서 { discussions } 라고 적어주면, 비로소 해당하는 value 값에 있는 데이터를 불러올 수 가 있는 것이다. 


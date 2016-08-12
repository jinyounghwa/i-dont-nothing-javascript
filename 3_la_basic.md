##### 이 책은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
3.1 문법
식별자
식별자 란 변수나 함수, 프로퍼티, 함수 매개변수의 이름입니다. 식별자는 다음 형식에 따라 한 개 이상의 문자로 표기합니다.
+ 첫번째 문서는 항상 반드시 및줄 ( _ )이나 달러($) 기호 중 하나여야 합니다.
+ 다른 문자에는 글자나 및줄, 달러기호, 숫자를 자유롭게 쓸 수 있습니다.

ECMAscript 에서는 관습적으로 카멜 케이스로 씁니다.  
첫번째 글자는 소문자로 쓰고, 단어가 바뀔 때는 바꾸니 단어의 첫번째 글자를 대문자로 씁니다.  
+ firstScend
+ myCar
+ doSomethingImportant

주석
// 와 / ** / 사용합니다.

스트릭트 모드
"use strict"; 로 엄격모드를 지원합니다. 전역으로 사용하거나 함수 단위로 사용 할 수 있습니다.
<pre>
<code>function doSomething () {
  "use strict";
  //함수 본문
}</code>
</pre>

문장  
자바스크립트는 각 문장을 세미콜론으로 종료합니다.
<pre>
<code>
var sum = a + b // 유효하지만 권장안합니다.
var diff = a- b; // 이렇게 쓰세요
</code>
</pre>
코드 블록
<pre><code>
if (test) {
  test = false;
  alert(test)
} // 이렇게 쓰세요
</code></pre>
###### 여담이지만 세미콜론을 사용하지 않을때는 for문 이나 if문 같은 부분에는 사용하지 않습니다.

키워드와 예약어
키워드들은 다음과 같습니다.   
<table>
    <tr>
    <td>break</td>
    <td>do</td>
    <td>instanceof</td>
    <td>typeof</td>

  </tr>
  <tr>
  <td>case</td>
  <td>else</td>
  <td>new</td>
  <td>var</td>
</tr>
<tr>
<td>catch</td>
<td>finally</td>
<td>return</td>
<td>void</td>
</tr>
<tr>
<td>continue</td>
<td>for</td>
<td>switch</td>
<td>while</td>
</tr>
<tr>
<td>debugger</td>
<td>function</td>
<td>this</td>
<td>with</td>
</tr>
<tr>
<td>in</td>
<td>try</td>
<td>-</td>
<td>-</td>
</tr>
</table>
예약어들은 다음과 같습니다.
<table>
    <tr>
    <td>abstract</td>
    <td>enum</td>
    <td>int</td>
    <td>short</td>
  </tr>
  <tr>
  <td>boolean</td>
  <td>export</td>
  <td>interface</td>
  <td>static</td>
</tr>
<tr>
<td>byte</td>
<td>extends</td>
<td>long</td>
<td>super</td>
</tr>
<tr>
<td>char</td>
<td>final</td>
<td>native</td>
<td>synchornized</td>
</tr>
<tr>
<td>class</td>
<td>goto</td>
<td>private</td>
<td>transient</td>
</tr>
<tr>
<td>debugger</td>
<td>implemments</td>
<td>protected</td>
<td>volatile</td>
</tr>
<tr>
<td>doble</td>
<td>import</td>
<td>public</td>
<td>-</td>
</tr>
  </table>
5판에서는 예약어 규칙이 바뀌어서 모드에 따라 예약어가 다릅니다. 다음 목록은 일반 모드에서 예약어로 사용하는 단어입니다.
<table>
<tr>
  <td>class</td>
  <td>enum</td>
  <td>extends</td>
  <td>super</td>
</tr>
<tr>
  <td>const</td>
  <td>export</td>
  <td>import</td>
  <td> - </td>
</tr>
</table>
스트릭트 모드에는 다음 예약어가 추가 됩니다.
<table>
<tr>
<td>implemments</td>
<td>packge</td>
<td>public</td>
<td>interface</td>
</tr>
<tr>
<td>private</td>
<td>static</td>
<td>let</td>
<td>protected</td>
</tr>
<tr>
<td>yield</td>
<td>-</td>
<td>-</td>
<td>-</td>
</tr>
</table>
ECMA-262 5판에는 키워드와 예약어가 바뀐것 이외에도 eval과 arguments에 제한이 추가되었습니다.

변수
ECMAscript 는 느슨한 변수타입을 사용하는데, 이 말은 변수에 어떤 타입의 데이터라도 저장할 수 있다는 의미
var 는 키워드이며 변수 이름은 식별자 입니다.
<pre>
var message;
</pre>
이 코드는 message라는 이름의 변수를 의미하며 해당 변수에는 어던 값이든 할당 할 수 있다. 초기화 하지 않으면 undefined할당됨
<pre>
var message = "hi";
</pre>
위 코드는 message변수가 문자열 값 hi를 저장한 것으로 정의함 (문자열 타입을 지정한 것은 아님)
<pre>
var message = "hi";
message = 100; // 유효하지만 권장안함
</pre>
아래는 함수를 종료하는 즉시 파괴되는 예
<pre>
function test() {
  var message = "hi"; //전역변수
}
test();
alert(message); // 에러
</pre>
var 연산자를 생략하면 전역이 된다.
<pre>
function test() {
  message = "hi";
}
test();
alert(message);
</pre>
변수를 여러 개 선언하려면 쉼표로 구분하여 한 문장으로 선언 할 수 있습니다.
<pre>
var message = "hi",
    found = false,
    age = 29;
</pre>
데이터 타입
원시 데이터 타입
+ undefined
+ null
+ boolean
+ 숫자
+ 문자열

##### typeof연산자: 이 연산자를 적용하면 다음 문자열 중 하나를 반환합니다.
+ 정의되지 않는 변수 : "undefined"
+ 불리언 : "boolean"
+ 문자열 : "string"
+ 숫자 : "number"
+ 함수를 제외한 객체 또는 null : "object"
+ 함수 : "function"
기술적으로 함수는 객체로 간주되고 함수에는 다른 객체에 없는 특별한 프로퍼티가 존재하므로
typeof 연산자가 함수를 다른 객체와 구별해 반환하는것이 합리적이라고 함
<pre>
var message = "some string";
alert(typeof message); // "string"
alert(typeof (message)); // "string"
alert(typeof 95); //"number"
</pre>

undefined타입
var 변수를 사용하여 정의하였지만 초기화하지 않으면 undefined가 할당됨
<pre>
var message;
alert (message == undefined); //true
//변수는 실행되었지만 초기화 되지는 않음

var message = undefined;
alert(message == undefined); // true
// 기본적으로 이렇게 항상 초기화 되기 때문에 이렇게 할 필요는 없음
</pre>

<pre>
var message ; // 변수는 선언되었지만 값은 undefined

// var age - 정의하지 않음
alert(message); //undefined
alert(age); // 에러
</pre>
정의하지 않은 변수에 유의미한 조작은 typeof 뿐임
<pre>
var message;  // 변수는 선언되었지만 값은 undefined
// var age - 정의하지 않음

alert(typeof message); // undefined
alert(typeof age); // undefined
</pre>

null 타입
typeof 를 호출하면 object를 반환하는게 특징
<pre>
alert(null == undefined); // true
</pre>
이렇게 나오는 이유는 == 연산자가 피 연산자를 비교할 때 암묵적으로 타입변환을 하기 때문
변수의 값에 명시적으로 undefined는 할당하면 안돼지만 null은 다름 객체를 이용할 수 없을때
null 을 오게 할 수도 있음

불리언 타입
ECMAscript에서는 모든 타입을 불리언 값으로 표현 할 수 있다.
<pre>
var message = "Hello world";
var messageAsBoolean = Boolean(message);
</pre>
이 예제에서는 문저일 message가 불리언 값으로 변환되어 messageAsBoolean에 저장된다.
Boolean() 함수는 어떤 타입의 데이터에서도 호출 할 수 있고 항상 불리언 값을 반환한다.
<table>
<tr>
<th>데이터 타입</th>
<th>true로 반환하는 값</th>
<th>false로 반환하는 값</th>
</tr>
<tr>
<td>불리언</td>
<td>true</td>
<td>true</td>
</tr>
<tr>
<td>문자열</td>
<td>비어있지않은 문자열 모두</td>
<td>""(빈 문자열)</td>
</tr>
<tr>
<td>숫자</td>
<td>0이 아닌 숫자, 무한대 포함</td>
<td>0,NaN</td>
</tr>
<tr>
<td>객체</td>
<td>모든객체</td>
<td>null</td>
</tr>
<tr>
<td>undefined</td>
<td>해당없음</td>
<td>undefined</td>
</tr>
</table>
if 문 같은 제어문은 다음과 같이 타입을 자동으로 불리언으로 바꾸므로 위 표에서 설명한 변환 규칙을 이해할것

<pre>
var message = "Hello world";
if (message) {
  alert("Value is true");
}
</pre>
이 예제에서는 alet가 표시되는데 message에 저장된 문자열이 자동으로 불리언 true로 바뀌기 때문
불리언을 써야하는데 실수로 객체로 쓰면 항상 true값이 되기때문에 주의해야 함

숫자타입
숫자를 계산 할 때 항상 10진수로 변환하여 계산
가능하면 정수로 계산하는것을 권장
평가할 때 소수점으로 평가하지 않음
<pre>
if(a+b == 0.3){ // 이렇게 하지 말라는이야기
  alert("you got 0.3")
}
</pre>
범위를 벗어나는 숫자  -infinity / infinity

NaN
숫자를 반환한 값으로 의도한 조작이 실패하였을 때 반환하는 값
에러를 반환하는 것이 아님
isNaN() 은 매개 변수를 하나 받으며 숫자가 아닌 값인지 검사한다.
<pre>
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false - 10 은 숫자입니다.
alert(isNaN("10")); // false - 숫자 10으로 바꿀 수 없다 "10" 은 문자로 인식되지만 숫자로 바꿀 수 있기 때문
alert(isNaN("blue")); // true - 문자열이기에 숫자로 바꿀 수 없다.
alert(isNaN(true)); // false - 숫자 1로 바꿀 수 있다.
</pre>

숫자로 바꿀 수 없는 것이나 NaN 자체인 경우 true / 숫자로 바꿀수 있는 경우 false를 반환

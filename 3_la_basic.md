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

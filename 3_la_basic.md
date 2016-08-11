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
| break | do | instanceof | typeof | 
| case | else | new | var | 
| catch | finally | return | void | 
| continue | for | switch | while |
| debugger | function | this | with | 
| default |if | throw | delete | 
| in|try | - | - | 
예약어들은 다음과 같습니다.
| abstract | enum | int | short | 
| boolean | export | interface | static | 
| byte | extends | long | super | 
| char | final | native | synchornized | 
| class | goto | private | transient | 
| debugger | implemments | protected | volatile | 
| doble | import | public | - | 

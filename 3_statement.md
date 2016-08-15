##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
if 문  
if 문은 프로그래밍 언어 대부분에서 가장 많이 쓰이는 제어문이다.
<pre>
if (condition) statement1 else statement2
</pre>

condition(조건)에는 어떤 표현식이든 쓸 수 있으며, 심지어 불리언값으로 평가되지 않는 표현식이라도 상관없다.
ECMAscript에는 자동으로 해당 표현식의 결과에 Boolean() 함수를 호출하여 불리언 값으로바꾼다.
condition이 true로 평가되면 statement1을 실행하고 false로 평가되면 statement2를 실행한다.
<pre>
if (i < 25) alert("Greater than 25"); // 한줄 문장
else{
  alert("Less than or equal 25") // 두 줄 문장
}
</pre>

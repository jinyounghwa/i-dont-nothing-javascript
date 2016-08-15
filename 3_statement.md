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

do-while 문  
do-while문은 평가 전 루프이다. 평가전 루프라는 말은 루프의 종로 조건을 평가하기 전에 루프 본문을 실행한다.
즉, 루프 본문은 최소 한번은 반드시 실행된다.  
<pre>
do {
  statement
} while (expression);
//  실제 사용은 다음과 같다.
var i = 0;
do {
  i += 2;
} while (i < 10);
</pre>
이 예제에서는 i가 10보다 작은 한 루프를 계속 실행한다. 변수 i는 0에서 시작하여 루프를 실행하는 동안 매회 2씩 늘어난다.  

while 문  
while문은 평가 후 루프이다. 평가 후 루프라는 말은 본문을 실행하기 전에 종료 조건을 평가한다.
루프 본문을 단 한번도 실행하지 않을 수 있다.  
<pre>
 while(expression) statement
 //실제 사용은 다음과 같다.
 var i = 0;
 while (i < 10) {
   i += 2;
 }
</pre>

for 문  
for 문 역시 평가 후 루프이며 루프에 들어가기 전 변수를 초기화 할 수 있다.
<pre>
for (initialization; expression ; post-loop-expression) statement
//실제 사용은 다음과 같다.
var count = 10;
for (var i = 0; i < count ;i++){
  alert(i);
}
</pre>  

이 코드는 변수 i를 정의하고 0으로 초기화 한다. for루프는 조건식  i < count 가 참으로 평가될 때만 실행하므로
while 문과 마찬가지로 루프 본문의 코드를 실행하지 않을 때도 있다. 루프 본문을 실행하면 루프 코드 역시 실행되어
변수 i의 값을 바꾼다. while문으로 바꾸어 보면 다음과 같다.  
<pre>
var count = 10;
var i =0;
while (i < count) {
  alert(i);
  i++;
}
// while루프가 할 수 없는것은 for 루프로도 불가능하다.
var count = 10;//for 루프 밖에서 초기화
var i;
for (i = 0; i < count;i++){
  alert(i);
}
//루프 밖에서 접근
var count = 10;
for (var i =0; i < count; i++) {
  alert(i);
}
alert(i); // 10 루프 밖에서 정의되어도 동작함
for (;;) { // 무한루프(조건식,조건표현식,루프 후 코드는 모두 옵션이며 다음과같이 모든 옵션이 생략하면 무한루프가 된다.)
  doSomething();
}
//조건 하나만 명시하면 for루프가 while루프처럼 동작 할 수 있음
var count = 10;
var i = 0;
for (;i < count;){
  alert(i);
  i++;
}
</pre>


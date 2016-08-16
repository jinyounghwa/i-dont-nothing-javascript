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

for-in 문   
for-in문은 엄격한 반복문으로 객체의 프로퍼티를 나열하는데 사용한다.
<pre>
for(property in expression) statement
// 실제 사용은 다음과 같다.
for (var propName in window) {
  document.write(propName);
}
</pre>
여기에서 for-in문은 BOM window객체의 모든 프로퍼티를 표시한다. 루프를 샐행할 때마다 propName변수에 window객체의 프로퍼티
이름이 저장된다. 이 과정은 객체의 프로퍼티를 모두 나열할 때까지 계속된다 for 문과 마찬기자로 for-in문 역시 제어부에서 반드시
var 키워드를 써야 하는것은 아니지만, 지역변수를 이용하게 하려면 var 키워드를 권장한다.  나열할 수 있는 프로퍼티는 모두 반환되지만
그 순서는 브라우저에 따라 다를 수 있다. null/undefined를 나열 할 경우 에러를 내고  ES5부터는 루프본문을 실행하지 않게 변경되었다.
for-in루프를 실행하기 전에 객체를 가리키는 변수의 값이 null/undefined인지 확인하는것을 권장한다.

문장 레이블
다음 문법에 따라 문장에 레이블을 붙였다가 나중에 사용할 수 있다.
<pre>
label : statement
//실제 사용은 다음과 같다.
start : for (var i=0; i < count; i++) {
  alert(i);
}
//이 예제에서 사용한 레이블 start를 나중에 break나 continue문에서 참조할 수 있다. 문장레이블은 중첩된 루프에서 일반적으로 사용한다.
</pre>

break문과 continue문
break문과 continue문을 통해 루프 내부의 코드 실행을 세밀히 조절 할 수 있다. break문은 즉시 루프에서 빠져나가 루프 다음 문장을 실행한다.
continue문은 루프를 즉시 빠져나가기는 하지만 루프 실행은 계속된다.
<pre>
var num = 0;
for (var i=1; i < 10; i++) {
  if ( i % 5 == 0) {
    break;
  }
  num++
}
alert(num); //4
</pre>
이 코드에서 for루프는 변수 i를 1부터 10까지 증가시킨다. 루프 본문에서 if문 으로 i 값이 5의 배수인지 확인한다. 5의 배수라면 break문이
실행되고 루프에서 빠져나온다. num변수는 0에서 시작되었으며 루프를 몇번 실행했는지 나타내는 값을 저장한다.
break문 다음에 실행할 코드, 즉 루프 다음줄의 alert에서 4를 표시한다. i가 5가 되었을 때 break문이 num을 증가시키기 전에
르프를 빠져나가기 때문에 루프는 4번 실행된다. break를 continue로 바꾸면 결과가 달라진다.
<pre>
var num = 0;
for (var i=1; i < 10; i++) {
  if ( i % 5 == 0) {
    continue;
  }
  num++
}
alert(num); //8
</pre>
이 코드에서는 루프가 8회 실행된다. i의 값이 5가 되면 루프에서 빠져나오며 num의 값은 6이다. 루프는 i가 10이 될때까지 돈다.
num의 마지막 값은 8인데 continue문 때문에 한번 빠져 나갔기 때문에 9가 아니라 한번 더 적은 8이다.
break문과 continue문을 문장 레이블과 함께 사용하면 코드의 특정 위치로 이동 할 수 있다.
<pre>
var num = 0;
outermost:
for (var i=0;i < 10;i++) {
  for (var j=0; j < 10 ; j++) {
    if (i == 5 && j == 5) {
      break outermost;
    }
    num++;
  }
}
alert(num); // 55
</pre>
이 예제에서 outermost레이블은 첫번째 for문을 가리킨다. 각 루프는 10회씩 실행되고 num++문장은 100회 실행된다.
따라서 실행이 끝나면 num은 100 이 된다. 그러나 이 코드의 break문은 매개변수로 레이블을 받았다.
i와 j가 모두 5인 시점에서 실행이 멈춰버리므로 num의 값은 55가 된다.
<pre>
var num = 0;
outermost :
for (var i = 0; i < 10; i++) {
  for (var j = 0; j < 10; j++) {
    if (i == 5 && j == 5){
      continue outermost;
    }
    num++
  }
}
alert(num); // 95
</pre>
이 코드에서 continue문은 실행을 강제하는데 내부 루프가 아니라 외부 루프의 실행을 강제한다. j가 5가 되면 continue문이 실행되고
내부루프는 다섯번의 실행을 건너뛰므로 num은 100이 아니라 95가 된다.

switch문
switch문도 다른언어에서 가져왔으며 if문과 관련이 깊다.
<pre>
switch (expression) {
  case value : statement
  break;
  case value : statement
  break;
  case value : statement
  break;
  case value : statement
  break;
  default : statement
}
</pre>
switch문의 각 case는 표현식이 value와 일치하면 statement문을 실행하라는 의미이며 break키워드는 코드 실행을 멈추고 문을 빠져나가라는 뜻이다.
break키워드를 쓰지 않으면 다음 case를 계속 평가한다. default키워드는 case중 value로 평가되는 것이 없을 때 실행할 문장이다.
<pre>
if ( i == 25 ) {
  alert("25");
}else  if ( i == 35){
  alert("35");
}else if ( i == 45) {
  alert("45");
}else {
  alert("Other")
}
// 다음과 같이 변경할 수 있다.
switch (i) {
  case : 25;
  alert("25");
  case : 35;
  alert("35");
  case : 45;
  alert("45");
  default:
  alert("Other")
}
</pre>
case문마다 주석을 달아서 진행을 알려주는것이 좋다.
switch문은 다른언에서 차용하긴 했지만 ECMAscript만의 고유한 특징이 있다. 모든 데이터 타입에서 동작하므로 문자열과 객체에서 사용할 수 있다.
대부분의 언어에스는 숫자타입에만 사용할 수 있다. 또한 둘째 값은 상수일 필요가 없고 변수나 표현식도 쓸 수있다.
<pre>
switch ("Hello world") {
  case "Hello" + "world":
  alert("Greeting was found");
  break;
  case "goodbye":
  alert("Closing was found");
  break;
  default:
  alert("Unexpected message was found");
}
</pre>
이 예제에서는 switch문에 문자열 값을 썼다. 표현식을 사용한 다음과 같은 코드도 사용 할 수 있다.
<pre>
var num = 25;
switch (true) {
  case num < 0:
  alert("Less then 0");
  break;
  case num >= 0 && num <=10:
  alert("Between 0 and 10.");
  break;
  case num >= 10 && num <= 20:
  alert("Between 10 and 20");
  break;
  default:
  alert("More than 20.");
}
</pre>
이 코드에서 변수 num은  switch문 밖에서 정의되었다. 각 case문은 불리언 값을 반환하므로 switch문에 매개변수로 true를 넘겼다.
각 case문을 순서대로 평가하면서 true인 것을 찾거나 default문을 만날 때까지 계속된다.
또한 switch문은 일치 연산자로 값을 비교하므로 타입 변환은 일어나지 않는다. 문자열 "10"과 숫자 10은 같은것으로 간주한다. 

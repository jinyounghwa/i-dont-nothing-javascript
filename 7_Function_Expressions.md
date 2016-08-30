##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
함수 표현식
이 장에서 다루는 내용
+ 함수 표현식의 특징
+ 함수와 재귀
+ 클로저를 이용한 고유 변수  

함수 표현식은 자바스크립트에서 가장 강력하면서도 혼란스러운 부분 중 하나이다. 함수를 정의하는 방법은 함수 선언과 함수 표현식 두 가지이다. 첫번째 함수 선언은 다음과 같이 function키워드 다음에 함수 이름을 쓰는 형태이다.  
<pre>
function functionName(arg0,arg1,agr2) {
  //함수 본문
}
</pre>
대부분의 브라우저에는 모두 함수에 비표준 프로퍼티인 name를 지원한다 이 값은 function키워드 다음에 쓴 함수 이름과 항상 일치한다.  
<pre>
alert(function.name); // "functionName"
</pre>
함수 선언에서 가장 뚜렷한 특징은 함수 선언 끌어올림(hoisting)이다. 이말은 함수 선언부를 다른 코드보다 먼저 읽고 실행한다는 뜻이다. 다음과 같이 함수를 호출하는 코드 다음에 함수를 선언해도 동작에는 문제가 없다.  
<pre>
sayHi();
function sayHi(){
  alert("hi");
}
</pre>
함수 선언부를 함수 호출보다 먼저 일고 실행하므로 이 예제에서는 에러가 일어나지 않는다. 함수를 정의하는 두번째 방법은 함수 표현식이다. 함수표현식에는 여러가지 형태가 있는데 많이 쓰이는 형태는 다음과 같다.  
<pre>
var functionName = function (arg0,arg1,agr2){
  // function body
}
</pre>
함수 표현식 패턴은 일반적인 변수 할당과 거의 비슷하다. 함수를 생성하고 이를 functionName이란 변수에 할당했다. 이렇게 생성된 함수는 function키워드 다음에 함수 이름이 없으므로 익명 함수로 간주한다.따라서 이러한 함수의 name프로퍼티는 빈 문자열이다. 함수 표현식은 다른 표현식과 마찬가지이므로 반드시 호출하기 전에 할당해야 한다. 다음코드는 에러를 일으킨다.  
<pre>
sayHi(); //에러 - 함수가 아직 존재하지 않는다.
var sayHi = function() {
  alert("Hi");
}
</pre>
함수 끌어올림을 이해해야 함수 선언과 함수 표현식의 차이를 이해할 수 있다. 다음코드를 실행해 보면 매우 의외의 결과를 보게 될 것이다.  
<pre>
//절대 이렇게 하지 말 것
if (condition) {
  function sayHi(){
    alert("hi");
  }else{
    function sayHi(){
      alert("hi");
    }
  }
}
</pre>
이 코드는 마치 condition이 true일 때와 false일 때 sayHi()함수를 서로 다르게 정의하는 것처럼 보니다. 하지만 이는 ECMAScript에서 허용하지 않는 문법이므로 자바스크립트 엔진에서는 에러를 수정하려고 한다. 문제는 브라우저마다 에러를 수정하는 방법이 다르기 때문에 결코 사용하면 안됀다. 다음코드와 같이 함수 표현식을 이용하는 예제는 안전하다.  
<pre>
var sayHi;
if (condition) {
  sayHi = function() {
    alert("Hi");
  };
}else{
  sayHi = function() {
    alert("Yo!")
  };
}
</pre>
함수 표현식을 사용하면 다른 함수에서 사용할 수 있도록 함수를 반환하는 형태도 가능하다. 이전 5장 에서 다음과 같은 함수를 만들었었다.  
<pre>
function createComparisonFunction(propertyName) {
  return function(Object1, Object2){
    var value1 = object1[propertyName];
    var value2 = object2[propertyName];
    if(value1 < value2){
      return -1;
    }else if (value1 > value2){
      return 1;
    }else{
      return 0;
    }
  }
}
</pre>
createComparisonFunction()는 익명함수를 반환한다. 반환된 함수는 아마 변수에 할당하거나 어떤 식으로든 호출하겠지만, createComparisonFunction()내부에서는 익명이다. 이렇게 값처럼 쓰이는 함수를 함수 표현식이라고 부른다. 물론 함수 표현식을 이런 용도로만 쓰지는 않고, 다른 쓰임은 이 챕터 전반에 걸쳐 설명한다.  

재귀  
'재귀함수'는 일반적으로 다음예제처럼 함수가 자기 자신을 이름으로 호출하는 형태이다.  
<pre>
function factorial(name) {
  if (num <= 1) {
    return 1;
  }else{
    return num * factorial(num -1);
  }
}
</pre>
이 예제는 재귀의 전형적 예제인 계승 함수이다 이 코드는 잘 동작하지만 함수 바로 다음에 아래 코드를 추가하면 잘 동작하지 않는다.  
<pre>
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4)); //에러
</pre>
여기에서 factorial()함수는 anotherFactorial이라는 변수에 저장되었다. 그리고 factorial변수에 null을 할당하였기 때문에 원래 함수에 대한 참조는 anotherFactorial만 남았다. anotherFactorial()을 호출하면 에러가 발생하는데, 더이상 함수가 아닌 factorial()을 실행하려 하기 때문이다. argument.callee를 쓰면 이 문제를 예방 할 수 있다.   
argument.callee는 현재 실행중인 함수를 가리키는 포인트 이므로 다음과 같이 재귀 호출에 쓸 수 있다.  
<pre>
function factorial(num) {
  if (num <= 1) {
    return 1;
  }else{
    return num * argument.callee(num-1);
  }
}
</pre>
함수 이름을 argument.callee로 바꾸면 함수를 어떻게 호출하든 올바르게 동작한다. 재귀 함수를 사용할 때는 항상 함수 이름대신 argument.callee를 쓰는것을 권장한다.  
스트릭트모드에서는 argument.callee의 값에 접근 할 수 없으며 대신 이름붙은 함수 표현식을 써서 같은 결과를 낼 수 있다. 다음 코드를 확인하라.  
<pre>
var factorial = (function f(num){
  if (num <= 1) {
    return 1;
  }else{
    return num * f(num-1);
  }
  })
</pre>
이 코드에서는 이름 붙은 함수 표현식 f()를 생성해 변수 factorial에 할당하였다. f라는 이름은 함수를 다른변수에 할당하더라도 그대로 유지되므로 재귀 호출은 정확히 실행된다.  

클로저  

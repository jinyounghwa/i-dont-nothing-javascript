##### 이 글은 자바스크립트를 말하다의 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
함수  
함수는 호출 할 수 있는 값이다. 함수를 정의하는 한 가지 방법은 함수선언이다, 예를들어 다음 코드는 단 하나의 매개변수 x를 받는 함수 id 를 정의한다.
<pre>
function id(x) {
  return x;
}
</pre>
return문은 id 값인 x를 반환한다. 함수를 호출할 때는 이름을 쓰고 괄호안에 매개변수를 쓴다.  
<pre>
id('hello')
'hello'
</pre>
함수에서 반환하는 값이 없으면 묵시적으로 undefined가 반환된다.  
<pre>
function f() {}
f();
// undefined
</pre>
이 장에서는 함수를 정의하고 호출하는 방법 중한가지만 설명하고 다른 방법은 나중에 설명하도록 하겠다.  

자바스크립트에서 함수의 세가지 역할  
자바스크립트에서 정의된 함수는 다음과 같이 3가지 함수로도 동작한다.  
+ 메서드 아닌 함수 ('일반적인 함수')  
이런 함수는 직접 호출 할 수도 있고 일반적인 함수로도 동작한다.  
<pre>id('hello')</pre>
+ 생성자  
함수를 new연산자로 호출 할 수 있다. (ES6의 arrow함수에서 사용불가) 이런 함수는 객체 팩토리인 생성자가 된다.  
<pre>new Date()</pre>  
생성자 이름은 대문자로 시작하는것이 관례이다.  
+ 메서드  
함수를 객체 프로퍼티에 저장할 수 있다. 이런함수를 메서드라고 부르며 그 객체를 통해 호출한다.  
<pre>obj.method()</pre>
매서드 이름은 소문자로 시작하는것이 관례이다.  

'Parameter' vs 'Argument'
Parameter와 Argument는 보통 콘텍스트에서 의도한 의미를 명확히 할 수 있으므로 종종 구분하지 않고 쓰기는 한다. 두 용어는 다음규칙으로 구별한다.  
+ Parameter는 함수를 정의할 때 사용하며 formal parameter, formal argument 라고 불리기도 한다.  
<pre>
function foo(param1, param2) {
  ...
}
</pre>
argument는 함수를 호출 할 때 사용하며 actual parameter, actual argument라고 부르기도 하다. 아래 예제의 3과 7은 argument이다.  
<pre>
foo(3,7);
</pre>

함수 정의  
다음 세가지에 대해 설명하도록 한다.  
+ 함수 표현식
+ 함수 선언
+ function() 생성자  
모든 함수는 객체이며, Function의 인스턴스이다.
<pre>
function id(x) {
  return x;
}
console.log(id instanceof function); // true
</pre>
따라서 함수는 Function.prototype에서 메서드를 가저온다.  

함수 표현식  
함수 표현식은 값, 즉 함수 객체를 생성한다. 다음 예제를 보자.  
<pre>
var add = function (x,y) {return x + y};
console.log(add(2,3)); //5
</pre>
위 코드는 함수 표현식의 결과를 add에 할당하고 그 변수를 통해 호출한다. 함수 표현식의 값은 위 예제처럼 변수에 할당하거나 다른 함수의ㅏ 매개변수로 쓰거나 그 밖에 다른 용도로도 쓸 수 있다. 일반적인 함수 표현식에서는 이름이 없으므로 '익명 함수 표현식'이리고 부르기도 한다.  

이름붙은 함수 표현식  
함수 표현식에서도 이름을 붙일 수 있다. 이름붙은 함수 표현식은 함수 표현식에서 자기 자신을 참조 할 수 있으므로 재귀구조에 유용하다.  
<pre>
var fac = function me (n) {
  if( n >0 ) {
    return n * me(n-1);
  }else{
    return 1;
  }
};
console.log(fac(3)); //6
//이름 붙은 함수 표현식의 이름은 함수 표현식 내부에서만 접근 할 수 있다.
var repeat = function me (n, str) {
  return n > 0 ? str + me(n-1, str) : '';
};
console.log(repeat(3,'Yeah')) // YeahYeahYeah
console.log(me); // ReferenceError : me is not defined
</pre>

함수 선언  
함수선언이란 새 함수를 선언하고 함수 객체를 만들어 그 변수에 할당하는 것을 말한다.  
<pre>
function add(x,y) {
  return x + y;
}
// 위 코드는 함수 표현식처럼 보이지만 문이다. 다음 코드와 어느정도 비슷하다.
var add = function(x,y) {
  return x + y;
}
</pre>

Function생성자  
Function()생성자는 문자열로 저장된 자바스크립트 코드를 평가한다. 예를들어 다음 코든 앞의 예제와 동등하다.
<pre>
var add = new function('x' + 'y', 'return x + y');
</pre>
그러나 함수를 위와 같이 정의하면 속도가 느릴 뿐아니라 코드가 문자열에 접근되므로 도구에서 접근 할 수 없기 때문에 가능하면 함수 표현식이나 함수선언을 사용하는게 좋다.  

끌어 올림  
끌어올림이란 '스코프 맨 앞으로 이동'이라는 뜻이다 함수선언은 완벽하게 끌어 올려지며 변수 선언은 부분적으로 끌어 올려진다.  
함수선언은 완벽히 끌어올려지므로 함수를 선언하기 전에 호출 할 수 있다.  
<pre>
foo();
function foo() {
  //이 함수는 끌어올려진다.
}
</pre>
var 선언 역시 끌어올려지지만 할당은 끌어올려지지 않으므로 var 선언과 함수 표현식을 아래와 같이 쓰면 에러가 발생한다.  

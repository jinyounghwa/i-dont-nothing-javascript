#8장 함수
함수는 한번정의하면 몇번이고 실행 할 수 있는 코드블록입니다.  
자바스크립트 함수는 매개변수를 가지고 있다.   
(문에서는 전달인자라고 하며, 함수 안으로 들어왔을 때는 매개변수 또는 파라미터라고 부름)  
어떤 객체의 프로퍼티로 할당된 함수를 해당객체의 매서드라고 한다.  
객체를 대상으로 해서 호출하면 이 객체는 해당 함수의 호출 컨텍스트(this)값이 된다.  
중첩된 함수는 해당함수가 정의된 유효범위 안에서 어떤 변수에도 접근할 수 있다. (클로저)  
#함수 정의하기
함수 - 함수 이름 식별자 - 매개변수 -  중괄호(실행되는 본문)
<pre>
function name (e) { 실행되는 머시기 와 return 같은 종료하고 반환되는 머시기(없음 undefined) };   
</pre>
함수 이름에 대한 부분은 본문내용을 참조한다.
함수선언문이 실제로 하는 일 : 어떤 변수를 정의하고 함수 객체를 그 변수에 할당하는 것이다.  
함수 대부분은 return을 포함하고 있으며, 다음에 오는 표현식의 값을 호출자에게 반환한다.
리턴값이 없는 함수를 프로시저(procedure)라고 부르기도 한다.
#중첩함수
다음 예제처럼 함수 중첩가능
<pre>
function hypotenuse(a,b) {
  function square(x) { return X*X;}
  return Math.sqrt(square(a) + square(b));
}
</pre>
중첩된 함수는 해당 함수가 속한 함수 (혹은 함수 들)의 매개변수와 변수에 접근할 수 있다.
#함수 호출하기
+ 일반적인 함수 형태
+ 매서드 형태
+ 생성자
+ call() / apply()  매서드  
일반적인 함수 호출에서 표현식의 값은 그 함수의 반환 값이다. 만약 인터프리터가 return 문을 만나지 못해 함수 끝에 다다르면  
그 반환값은 undefined  

<strong>매서드 호출</strong>
o.m = f;
o.m ();
o.m(x,y);
o['m'](x,y);
a[0](z)

복잡한 프로퍼티 접근 표현식을 포함 할 수 있다.
f().m()
customer.surrname.toUpperCase()

+ 매서드 체이닝
매서드가 객체를 반환하면 매서드의 반환 값을 후속 호출의 일부로 사용
<pre>
// 모든 hadder를 갖고
// 헤더의 id 에 대한 map 함수 결과를 배열로 얻고 get() 정렬 sort() 한다.
$(':hadder').map(function(){return this.id}).get().sort();
</pre>

+ this는 키워드이고 변수나 프로퍼티 이름이 아니기 때문에 this에 값을 할당 할 수 없다.
+ 변수와 달리 this키워드는 유효범위가 없고 중첩 함수는 호출자의 this값을 상속하지 않는다.
+ 만약 중첩 함수가 매서드 형태로 호출되면, 그 함수의 this 값은 그 함수의 호출 대상 객체다.

# 생성자 호출
함수나 매서도 호출 앞에 new키워드가 있다면 생성자 호출
생성자 호출에서 괄호 안에 전달인자 목록이 포함되어 있다면, 우선 전달인자 표현식이 평가되고 함수와
메서드 호출에 경우와 마찬가지로 평가된 전달인자가 함수에 전달된다.

+ 생성자 함수는 객체를 초기화하고, 새로 생성된 이 객체는 생성자 함수의 컨텍스트로 사용된다.
+ 생성자 함수는 this 키워드로 접근 할 수 있다.
+ 생성자 함수는 보통 return 키워드를 사용하지 않는다.

# 간접호출 (뒤에 나오기 때문에 생략)

# 함수 전달인자와 매개변수
자바스크립트에서 함수를 정의할 때는 함수 매개변수의 타입, 즉 자료형을 명시하지 않는다.

+ 생략 가능한 매개변수
본래 정의된 값보다 적은 수의 잔달인자로 함수가 호출되면 나머지 매개변수는  undefined 값으로 설정된다.

# 가변길이 전달인자 목록 : Argument객체
함수가 호출될 때 정의된 매개변수보다 더 많은 인자가 전달되면, 매개변수이름이 붙지 않은 인자값을 직접적으로 참조할 방법이 없다.
Argument객체는 이것을 해결해 준다.인덱스 숫자를 통해 함수의 전달 인자를 얻어 올 수 있다.
<pre>
function max (/*...*/){
  var max = Number.NEGATIVE_INFINITY;
  //전달인자를 순회하며 가장 큰 값을 찾아 기억한다.
  for (var i =0 ;i<argment.length;i++)
  if (argment[i] > max) max = argment[i];
  //가장 큰 값을 반환한다.
  return max;
}
var largest = max(1,10,100,2,3,1000,4,5,10000,6); // => 10000
</pre>
# 객체의 프로퍼티를 전달인자를 사용하기
함수의 세개 이상의 매개변수가 있을 때 호출하는 인자의 올바른 순서를 기억하기 위함

# 값으로서의 함수, 전달인자 형식, 자신만의 함수 프로퍼티 정의하기
-책 본문 참조

# 네임스페이스 함수, 클로저 후반부
이 부분은 아마도 IIFE를 의미하는 부분인데 이 부분은 아래 링크를 참고하며 해당 글을 작성한 원작자의 설명을 듣겠음(원작자 - 강승철)
[Module Pattern](https://github.com/dotNetTree/I-Konow-JS/blob/master/oop-in-js/03_module_pattern_and_....md)

#. 클로저
가장 일반적으로 중첩 함수를 반환할 때 어휘적 유효 범위를 알아볼 경우 사용한다.
<pre>
var scope = "global scope"
function checkscope() {
  var scope = "local scope"
  function f() {return scope;}
  return f();
}
checkscope(); //local scope
</pre>
다음 코드는 무엇을 반환하나
<pre>
var scope = "global scope"
function checkscope() {
  var scope = "local scope"
  function f() {return scope;}
  return f;
}
checkscope(); // function f() { return scope}
</pre>

+ 클로저들이 공유해서는 안되는 변수를 공유하는 실수들
<pre>
//이 함수는 v를 반환하는 함수를 반환한다.
function constfunc(v) {return function(){return v;}}
//상수함수에 배열 생성
var funcs = [];
for (var i=0; i<10; i++) funcs[i] = constfunc(i)
//배열요소 5의 함수는 값 5를 반환한다.
</pre>

+ 또한 클로저를 작성 할 때 this는 자바스크립트의 변수가 아니라는 것이다. 바깥쪽의 this 값을  
 별도의 변수로 저장하지 않으면 클로저는 바깥쪽 함수의 this 값에 접근 할 수없다.

# 함수 프로퍼티, 매서드, 생성자
+ length프로퍼티 (생략)
+ prototype프로퍼티 (생략)
+ call() / apply() 매서드 : 다른 객체의 매서드인것처럼 간접적으로 호출
ES5 에서 call() 또는 apply() 의 첫 번째 인자는 함수내에서 this 값이 되는데 null/undefined든 상관없음

+ 본문 234페이지의 trace()함수 : 지정된 매서드를 새로운 매서드로 교체하는데 새로운 매서드는 원본메서드를 추가 기능으로 둘러쌈
+ Monkey patch :기존 메서드를 동적으로 수정하는 방식 (왠지 안티패턴의 냄새가 난다~!!!)

+ bind() 매서드
함수와 객체를 묶는 함수, 전달하는 인자 중 첫번째 이후의 모든 인자는 this값과 함께 해당 함수의 인자로 바인딩 된다.
커링이라고 부르기도 한다.
[자바스크립트 커링](http://anster.tistory.com/144)

+ toString() 매서드
책에서는 짧게 표현되어 있는데 개인적으로 타입을 체크할 때 유용하게 사용 할 수 있다고 생각한다.  
보통 typeof(null 을 Object로 판단) / instanceof(이미 알고있는 클래스만 확인가능) / constructor (undefined/null 구분불가)
주로 Object.toString() 으로 사용한다.

+ Function() 생성자 외 이하 항목은 다음시간에 나올것이므로 생략

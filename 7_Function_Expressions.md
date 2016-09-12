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
function functionName(arg0,arg1,arg2) {
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
  });
</pre>
이 코드에서는 이름 붙은 함수 표현식 f()를 생성해 변수 factorial에 할당하였다. f라는 이름은 함수를 다른변수에 할당하더라도 그대로 유지되므로 재귀 호출은 정확히 실행된다.  

클로저  
'익명함수' 와 '클로저'는 자주 혼용된다. '클로저'란 다른함수의 스코프에 있는 변수에 접근 가능한 함수이다. 클로저는 보통 이전 예제의 createComparisonFunction()를 다음과 같이 고쳐서 만들어 낸다.  
<pre>
function createComparisonFunction(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName];
    var value2 = Object2[propertyName];
    if (value1 < value2) {
      return -1;
    }else if(value1 > value2) {
      return 1;
    }else{
    return 0;
    }
  };
}
</pre>
이 예제의 var 부분은 내부 함수(익명함수) 이면서 외부함수의 변수 (propertyName)에 접근한다. 내부 함수가 반환되어 다른 컨텍스트에서 실행되는 동안에도 propertyName에 접근 할 수 있다. 이런일이 가능한 것은 내부 함수에 스코프 체인에 createComparisonFunction()의 스코프가 포함되기 때문이다. 클로저를 잘 이해하기 위해서는 스코프 체인이 어떻게 생성되고 사용되는지 알아야 한다. 함수 활성화 객체는 argument 및 이름붙은 매개변수로 초기화 된다. 외부 함수의 활성화 객체는 스코프체인의 두 번째 객체이다. 이 과정이 포함관계에 있는 함수에서 계속 발생하여 스코프 체인이 전역 실행 컨텍스트에서 종료될 때가지 이어진다.  
함수를 실행하면 값을 읽거나 쓸 변수를 스코프체인에서 검색한다. 아래의 코드를 확인하자.  
<pre>
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  }else if(value1 > value2){
    return 1;
  }else{
    return 0l
  }
}
var result = compare(5,10);
</pre>
이 코드는 전역실행 컨텍스트에서 호출하는 compare()라는 함수를 만든다. compare()을 처음으로 호출하면 argument,value1,value2 를 포함한 활성화 객체가 만들어 진다. 전역실행 컨텍스트의 변수 객체는 this와 result,compare를 포함하는 compare()실행 컨텍스트의 스코프 체인의 다음에 위치한다 아래 그림이 이런 관계를 나타내었다.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/fe71.png)  
이면에서는 실행 컨텍스트마다 변수를 나타내는 객체가 존재한다 전역 컨텍스트의 변수 객체는 항상 존재하지만, compare()같은 로컬 컨텍스트 변수 객체는 함수를 실행하는 동안에만 존재한다. compare()함수를 정의하면 스코프 체인이 생성되고  자바스크립트 인터프리터는 이를 전역 변수 객체와 함께 미리 읽어 함수 내부의 [[Scope]]프로퍼티에 저장한다. 함수를 호출하면 실행컨텍스트가 생상되며 함수의 [[Scope]]프로퍼티에 들어있는 객체를 복사하여 스코프  체인이 생성된다. 다음에는 변수 객체로 동작하기도 하는 활성화 객체가 생성되어 컨텍스트의 스코프 체인 앞에 위치한다. 이 예제에서는 compare()함수의 실행 컨텍스트가 스코프 체인 내에 변수 객체를 두 개, 즉 로컬 활성화 객체와 전역변수 객체를 가진다는 뜻이다. 스코프 체인아란 변수 객체를 가리키는 포인터 목록이며 객체를 직접 포함하는 것은 아니다.  
함수에서 변수에 접근할 때마다 스코프 체인에서 해당 이름의 변수를 검색한다. 함수 실행이 끝나면 로컬 활성화 객체는 파괴되고 메모리에는 전역 스코프만 남는다. 하지만 클로저는 이와 다르게 동작한다.   
다른 함수의 내부에 존재하는 함수는 외부 함수의 활성화 객체를 자신의 스코프 체인에 추가한다. 따라서 createComparisonFunction()(외부함수)내부에 있는 익명함수의 스코프 체인에는 외부 함수의 활성화 객체를 가리키는 포인터가 존재하는 것이다. 아래 그림을 확인하자.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/fe72.png)

외부 함수가 실행을 마치고 익명 함수를 반환하면 익명 함수의 스코프 체인은 외부 함수의 활성화 객체와 전역 변수 객체를 포함하도록 초기화 한다. 이 때문에 익명함수는 외부 함수의 변수 전체에 접근 할 수 있게 된다. 한가지 흥미로운 부수 효과는 외부 함수가 실행을 마쳤는데도 활성화 활성화 객체가 파괴되지 않는 점인데 아직 익명 함수의 스코프 체인에서 이를 참조하기 때문이다 외부 홤수가 실행을 마치면 실행 컨텍스트의 스코프 체인은 파괴되지만, 활성화 객체는 익명 함수가 파괴될 때까지 메모리에 남는다.  

<pre>
//함수 생성
var compareNames = createComparisonFunction("name");
//함수 호출
var result = compareNames({name : "Nicholas"}, {name:"Greg"});
//함수 파괴, 메모리 회수 가능
compareNames = null;
</pre>

여기에서는 비교 함수를 생성해서 변수 compareNames에 저장을 했다. compareNames에 null을 할당하면 함수에 대한 참조가 사라지므로 콜랙션에서 메모리를 회수 할 수 있다. 스코프 체인은 파괴되고 전역 스코프를 제외한 모든 스코프를 파괴해도 안전하다. 이 예제를 실행해서 compareNames()를 호출하면 그림 7-2와 같은 스크포 체인 관계가 형성된다.  

클로저와 변수  
스코프 체인에는 한가지 눈에 띄는 부작용이 있다.<b>클로저는 항상 외부함수의 변수에 마지막으로 저장된 값만 알 수 있다.</b>클로저가 특정 변수가 아니라 전체 변수 객체에 대한 참조를 저장함을 기억하라.  
<pre>
function createFunctions(){
  var result = new Array();
  for (var i = 0; i < 10; i++){
    result[i] = function(){
      return i;
    };
  }
  return result;
}
</pre>
이 함수는 함수로 이루어진 배열을 반환한다. 눈으로 보기에는 각 함수가 자신의 인덱스에 해당하는 값을 반환할 것 같아 보인다. 즉 인덱스 0의 함수는 0을 반환하고 인덱스 1의 함수는 1을 반환하는 것처럼 보인다. 사실은 모든 함수가 10을 반환한다. 모든 함수가 스코프체인의 createFunctions()의 활성화객체를 포함하므로 이들은 모두 같은 변수 즉 i를 참조한다. createFunctions()이 실행을 마치면 i에 10이 저장된다. 모든 함수가 같은 변수 객체를 참조하고 그 변수 객체에서 i값은 10이므로 모든 함수에서 i는 10이다. 하지만 다음과 같이 익명함수를 만들어서 클로저가 적절히 의도한대로 동작하게 만들 수도 있다.  
<pre>
function createFunctions() {
  var result = new Array();
  for (var i = 0; i< 10; i++){
    result [i] = function(num) {
      return function(){
        return num;
      };
    }(i);  
  }
  return result;
}
</pre>
고처 쓴 createFunctions()에서는 각 함수가 다른 숫자를 반환한다 클로저를 직접 배열에 할당하는 대신 익명함수를 정의하고 즉시 호출하였다. 익명함수는 매개변수로 num하나만 받는데 이는 결과 함수가 반환할 숫자이다 변수 i를 익명함수에 매개변수로 넘겼다. 함수 매개변수는 값 형태로 전달되므로 i의 현재 값을 매개변수 num에 복사한다. 익명함수는 num을 간직한 클로저를 생성하며 반환한다. 이제result배열에 들어 있는 각 함수는 고유한 numㄹ값을 가지며 그 값을 반환한다.  

this객체  
클로저 내부의 this는 복잡하게 동작한다. this객체는 런타임에서 함수가 실행중인 컨텍스트에 묶인다. 즉 전역 함수에서 this는 스트릭트 모드가 아닐때는 window, 스트릭트 모드에서는 undefined이며 함수가 객체 메서드로 호출되었을 때 this는 해당 객체이다. 이 컨텍스트에서 익명 함수는 특정 객체에 묶에 있지 않으므로 스트릭트 모드가 아니라면 this객체는 window이며 스트릭트 모드에서는 undefined이다. 하지만 클로저를 작성하는 형식 때문에 이를 분명이 알기는 어렵다.  
<pre>
var name = "the window";
var object = {
  name : "My Object",
  getName : function(){
    return function(){
      return this.name;
    };
  }
};
alert(object.getNameFunc()()); // "the window" (스트릭트 모드가 아닐 때)
</pre>
여기에서는 name이라는 전역변수와 객체를 함께 생성했는데 객체에도 name프로퍼티가 있다. 객체의 getNameFunc()매서드는 익명 함수를 반환하고 익명 함수는 this.name를 반환한다. getNameFunc()는 함수를 반환하므로 object.getNameFunc()()을 호출하면 getNameFunc()가 반환하는 함수를 즉시 호출하는 것이나 마찬가지이며 문자열을 얻는다. 반환 받은 문자열은 전역 name변수의 값인 "the window"이다. 익명 함수가 외부 스코프의 this객체를 포함하지 않았던 이유는 무었일까? 모든 함수는 호출되는 순간 자동으로 this와 argument두 특별한 변수를 가지게 된다. 내부 함수는 결코 외부 함수의 this와 argument에 직접적으로 접근 할 수가 없다. 다음과 같이 외부 함수의 this객체를 다른 변수에 저장해서 클로저가 이 변수에 접근하도록 하는 일은 가능하다.  
<pre>
var name = "the window";

var object = {
  name : "My Object",
  getNameFunc : function() {
    <b>var that = this;</b>
    return function(){
      <b>return that.name;</b>
    };
  }
};
alert(object.getNameFunc()()); // "My Object"
</pre>
강조한 두 줄은 이 예제와 이전 예제의 차이를 나타낸다. 익명 함수를 정의하기 전에 외부 함수의 this객체를 that이란 변수에 할당하였다. 이제 클로저를 정의하면 that는 외부 함수에 고유한 변수이므로 클로저는 이 변수에 접근 가능하다. 함수가 반환된 뒤에도 that 은 여전이 object에 묶여 있으므로 object.getNameFunc()()을 호출하면 "My Object"를 반환한다.  
this의 값이 예상과 다른 특별한 경우가 몇 가지 있다. 이전 예제를 조금 수정한 아래의 코드를 확인하자.  
<pre>
var name = "the window";
var obj = {
  name : "my object",

  <b>getName : function(){
    return this.name;
    }</b>
}
</pre>
getName()매서드는 단순히 this.name의 값을 반환한다. object.getName()을 호출하는 다양한 방법과 그 결과를 확인하자.  
<pre>
object.getName(); //"my object"
(object.getName)();//"my object"
(object.getName = object.getName)(); // "the window" 스트릭트 모드가 아닐 때
</pre>
첫 번째 줄은 일반적인 방법으로 object.getName()을 호출하는데 this.name과 object.name이 같으므로 "my object"를 반환하였다. 두 번째 줄은 object.getName을 호출하기 전에 괄호로 감쌌다. 함수를 참조한 것처럼 보이지만 object.getName와 (object.getName)은 같은것으로 정의되어 있으므로 this값은 그대로 유지된다. 세 번째 줄에서는 먼저 할당한 후 그 결과를 호출하였다. 할당 표현식의 값은 함수 자체이므로 this값이 유지되지 않아서 the window를 반환한다. 아마 두 번째나 세번째의 패턴을 거의 사용하지 않겠지만 조금만 바뀌어도 this값이 예상하지 못하게 바뀔 수 있음을 알아두자.  

블록 스코프 흉내내기  
자바스크립트에서는 블록레벨 스코프라는 개념이 ES6이전에는 없기 때문에 블록 문장에서 정의한 변수는 해당 문장이 아니라 외부 함수에 묵인다.  
<pre>
function outputNumbers(count){
  for(var i = 0; i < count; i++){
    alert(i);
  }
  alert(i); //count
}
</pre>
이 함수는 for루프를 정의하고 변수 i를 0으로 초기화 한다. 자바나 C++같은 언어에서는 변수 i가 for루프 블록 내부에 존재하므로 루프가 실행을 마치면 변수는 곧 파괴된다. 하지만 자바스크립트에서는 변수 i가 outputNumbers()의 활성화 객체 일부은 것으로 정의되므로 그 시점부터는 함수 내에서 접근 할 수 있다. 심지어 다음과 같이 변수를 재선은 에도 값은 그대로 유지된다. (왠만하면 하지 말 것)  

자바스크립트는 같은 변수를 다시 선언하더라도 에러를 내지 않으며 단순히 선언을 무시할 뿐이다. (선언과 할당을 동시에 하는 초기화는 적용한다.)익명함수를 통해 블룩 스코프를 흉내 내서 이 문제를 해결 할 수 있다.  
익명 함수를 블록 스코프처름 쓰는 문법은 '고유 스코프' 라고 부르기도 하며 다음과 같은 형태이다.  
<pre>
(function(){
  // 코드블록
  })();
</pre>
이 문법에서는 익명함수를 정의하는 즉시 호출한드 이를 '즉시호출함수'라고 부르기도 한다. 함수 선언처럼 보이는 부분을 괄호로 감싸 이것이 사실 함수 표현식임을 나타내었다. 이 함수는 바로 다음에 나오는 괄호를 통해 호출한다. 이 문법이 혼란스러워 보이면 다음 코드를 확인하자.  
<pre>
var count = 5;
outputNumbers(count);
</pre>
이 예제처럼 변수 count는 5로 초기화 되었다. 변수를 선언하자마자 바로 함수에 넘겼으니 불필요한 코드가 있는 셈이다. 다음과 같이 함수를 호출할 때 변수 count대신 5를 넘기면 코드가 한결 간결하다.  
<pre>
outputNumbers(5);
</pre>
결과는 마찬가지 이다. 변수란 결국 다른 값을 표현할 뿐이므로 변수 대신 값을 써도 코드는 똑같이 동작한다.  
<pre>
var someFunction = function () {
  //코드블록
};
someFunction();
</pre>
이 예제에서는 함수를 정의하고 즉시 호출했다 익명함수를 생성하고 변수 someFunction에 할당하였다 그 다음 함수이름뒤에 괄호를 붙여서 즉 someFunction() 으로 함수를 호출하였다 이전 예제에서 변수 count를 실제 값으로 대체해도 마찬가지 이다. 이 코드도 같은데, 하지만 다음 코드는 동작하지 않는다.  
<pre>
function(){
  //코드블록
}(); //에러!
</pre>
이 코드는 문법 에러를 일으킨다. 자바스크립트 function키워드를 함수선언 의 시작으로 인식하는데, 함수 선언 다음에 괄호를 쓸 수 없기 때문이다. 하지만 함수 '표현식'다음에는 괄호가 허용 된다. 함수 선은을 함수 표현식으로 바꾸려면 다음과 같이 괄호로 감싸주기만 하면 된다.  
<pre>
function outputNumbers(count){
  (function(){
    for (var i=0; i<count; i++){
      alert(i);
    }
    })();
    alert(i); // 에러를 일으킨다.
}
</pre></pre>
고쳐 쓴 outputNumbers()함수에서는 for루프를 고유 스코프로 감쌌다. 익명 함수 안에서 정의한 변수는 익명 함수가 실행을 마치는 즉시 파괴되므로 변수 i도 루프 안에서 사용한 후 파괴된다. 익명함수는 클로져여서 외부 스코프 변수에 자유로이 접근 할수 있으므로 ount변수 역시 고유 스코포 안에서 접근 가능하다. 이 테크닉은 전역 스코프에 추가되는 변수나 함수의 수를 제한하는 용도로 자주 사용한다. 일반적으로 전역 스코프에 변수나 함수를 추가하지 않는 편이 좋으며 여러 개발자가 참여하는 대규모 애플리케이션에서는 이를 꼭 지켜야 이름 충돌을 막을 수 있다. 고유 스코프를 이용하려면 전역 스코프를 어지럽힐 염려 없이 자유롭게 변수를 쓸 수 있다.  
<pre>
(function(){
  var now = new Date();
  if(now.getMonth() == 0 && now.getMonth()==1){
    alert("happy new year")
  }
  })();
</pre>
이 코드를 전역 스코프에 두면 실행 날짜가 1월 1일인지 판단하여 사용자에 메시지를 보내는 기능이 생성된다. 변수 now는 전역 스코프의 변수가 아니라 익명함수의 지역 변수이다.  

고유 변수  
엄밀히 말해 자바스크립트에는 고유 구성원이란 개념이 없으며 객체 프로퍼티는 모두 공융이다. 하지만 고유변수 개념은 있다. 함수안에서 정의한 변수는 함수 밖에서 접근할 수 없으므로 모두 고유 변수라고 간주한다. 여기에는 함수 매개변수,지역변수,내부함수등이 포함된다. 다음 코드를 보자  
<pre>
function add(num1, num2) {
  var sum = num1 + num2;
  return sum;
}
</pre>
이 함수에는 num1, num2,sum 세 가지 고유 변수가 있다. 함수 내부에는 이들 변수에 접근 할 수 있지만 함수 밖에서는 불가능하다. 클로저를 이 함수 안에서 만들면 스코프 체인을 통해 이들 변수에 접근 할 수 있다. 이런 지식을 활용해서 고유 변수에 접근 가능한 공융 매서드를 만들 수 있다.  
특권(privileged) 매서드는 고유 변수/함수에 접근 가능한 공용 매서드이다. 객체에 특권 매서드를 만드는 방법은 두가지이다. 첫번째 방법은 다음과 같이 생성자 안에서 만드는 방법이다.  
<pre>
function MyObject(){
  //고유 변수와 함수
  var privateVariable = 10;
  function privateFunction() {
    return false;
  }
  //특권 매서드
  this.publicMethod = function() {
    privateVariable++;
    return privateFunction();
  };
}
</pre>
이 패턴은 생성자 안에서 모든 고유 변수와 함수를 정의한다. 다음에는 이미 정의한 고유 멤버에 접근 가능한 특권 매서드를 만들 수 있다. 이 패턴이 가능한 것은 생성자 안에서 정의한 특권 매서드가 클로저가 되어서 생성자의 스코프 안에서 정의된 모든 변수와 함수에 접근할 수 있기 때문이다. 이 예제의 변수 privateVariable과 함수 privateFunction()sms publicMethod()에서만 접근 가능하다.일단 MyObject의 인스턴스를 생성하면 privateVariable과 privateFunction()에 집적적으로 접근할 방법은 없으며 반드시 publicMethod()를 통해야 한다. 다음과같이 고유 및 특권 멤버를 정의해서 데이터를 직접적으로 수정 할 수 없게 보호 할 수 있다.  
<pre>
function Person(name) {
  this.getName = function () {
    return name;
  };
  this.setName = function(value) {
    name = value;
  };
}

var person = new Person("Nicholas");
alert(person.getName()); //"Nicholas"
person.setName("Greg");
alert(person.getName());//"Greg"
</pre>
이 코드의 생성자는 getName()과 setName()두 매서드를 정의한다. 생성자 밖에서 각 매서드에 접근 할 수 있고, 각 매서드는 고유 변수 name에 접근 가능하다. Person생성자 외부에서 name변수에 직접 접근할 방법은 없다. 두 매서드는 모두 생성자 안에서 정의된 클로저이므로 스코프체인을 name에 접근 가능하다. 생성자를 호출 할 때마다 매서드가 재생되므로 고유 변수 name는 person의 인스턴스마다 고유하다. 한가지 단점은 오직 생성자 패턴을 통해서만 이런 결과가 가능하다는 점이다. 생성자 패턴에서는 매서드가 인스턴스마다 생성된다는 결점이 있다. 정적고유변수를 사용해 특권 매서드를 만들면 이 문제는 사라진다.  

정적 고유 변수  

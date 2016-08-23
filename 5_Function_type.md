##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
Function 타입  
함수는 ECMAscript에서 가장 흥미로운 부분인데 이는 함수가 사실 객체라는 점에 기인한데 모든 함수는 Function타입의 인스턴스이며 다른 참조 타입과 마찬가지로 프로퍼티와 매서드가 있다. 함수 역시 객체이므로 함수 이름은 단순히 함수 객체를 가리키는 포인터일 뿐이지 함수와 단단히 결합되는것은 아니다. 함수는 보통 다음과 같은 함수 선언 문법을 통해 정의한다.  
<pre>
function sum (num1, num2) {
  return num1 + num2;
}
// 함수 표현식은 다음 표현식과 일치한다.
var sum = function (num1, num2) {
  return num1 + num2;
}
</pre>
두번째 코드는 변수 sum을 정의하면서 함수로 초기화 하였다. 함수표현식에서 function 키워드 다음에 함수 이름이 없는점에 주목하라. 변ㅅ sum으로 함수를 참조하므로 함수 이름은 필요하지 않는다. 또한 표현식 마지막에 다른 변수 초기화 문장과 마찬가지로 세미콜론을 쓴점도 주목하라. 함수를 정의하는 마지막방법은 Function생성자를 이용하는 방법이다. Function생성자에 넘기는 매개변수 숫자에는 제한이 없다. 매개변수 중 마지막은 함수 바디로 간주하며 그 이전에 있는 매개변수는 함수의 매개변수로 전달된다. 예를들어 다음 코드를 살펴보자  
<pre>
var sum = new function("num1", "num2", "return num1 + num2");// 권장하지는 않음.
</pre>
사실 이런 문법은 권장하지 않는데, 이런 문법을 쓰면 코드를 두번, 즉 일반적인 ECMAscript표현식으로 평가하고 생성자에 문자를 다시 평가해야 하므로 성능에 영향이 있기 때문이다.  
함수 이름은 단순히 함수를 가리키는 포인터일 뿐이므로 객체를 가리키는 포인터와 마찬가지 이다. 즉 다음과 같이 함수 하나하나에 이름을 여러 개 붙일 수 있다.  
<pre>
function sum (num1, num2) {
  return num1 + num2;
}
alert(10,10) // 20

var anotherSum = sum;
alert(anotherSum(10,10)); // 20

sum = null;
alert(anotherSum(10,10)) //20
</pre>
위 코드는 두 숫자를 더하는 sum()함수를 생성한다 변수 anotherSumㅇ르 선언하고 sum과 같다고 초기화 했다. 괄호를 쓰지 않고 함수 이름만 쓰면 함수를 샐행하지 않고 함수를 가리키는 포인터에 접근하는 것이다. 이 시점에서 anotherSum과 sum변수는 같은 함수를 가리키므로 anotherSum()를 호출해도 같은 결과를 반환한다. sum에 null을 할당하면 함수와의 연결이 사라지지지만 anotherSum()은 아무 문제 없이 함수를 호출한다.  

오버로딩 없음  
함수 이름이 단순한 포인터임을 이해하면 ECMAscript에서 함수 오버로딩이 불가능한 이유도 이해햘 수 있다.  
<pre>
avr addSomeNumber = function(num) {
  return num + 100;
}
addSomeNumber = function(num) {
  return num + 200;
}
var result = addSomeNumber(100); //300
// 두번째 함수를 덮어 쌩성하는 순간 addSomeNumber의 내용을 덮어 쓴다.
</pre>

함수선언 vs 함수 표현식  
자바스크립트 엔진이 실행 컨텍스트에 데이터를 불러 올 때 중요한 차이가 있다. 함수 선언은 어떤 코드도 실행하기 전에 이미 모든 실행 컨텍스트에서 접근하고 실행 할 수 있지만, 함수 표현식은 코드 실행이 해당 줄까지 진행하기 전에는 사용할 수 없다.  
<pre>
alert(10,10);
function sum(num1, num2) {
  return num1 + num2;
}
// 호이스팅을 통해 함수 선언을 읽고 실행컨텍스트에 추가하기 때문에 문제가 없다.

alert(10,10);
var sum = function(num1, num2) {
  return num1 + mu,2
};
//에러
</pre>
두번째 코드에서는 에러가 나는데 함수가 함수 선언이 아니라 초기화 문장의 일부분이기 때문이다. 바꿔말해 이 함수는 강조한 부분까지 실행하기 전에는 변수 sum에 할당되지 않는데 코드 첫 줄에서 예기지못한 식별자 에러를 내기 때문에 강조한 부분은 절대 실행되지 않는다.
이런 차이를 제외하면 함수를 이름으로 호출 할 때 두 문법사이에 다른 차이는 업다.  

값처럼 쓰는 함수  
ECMAscript에서 함수 이름은 단지 변수일 뿐이므로 함수도 다른 값이 올 수 있는곳이라면 어디든 올 수 있다. 이런 특징 덕분에 함수를 다른 함수에 매개변수로 넘기거나, 함수가 실행 결과로 다른 함수를 반환하는 일이 가능하다. 다음 함수를 살펴보자.  
<pre>
function callSomeFunction(someFunction, SomeArgument) {
  return someFunction(SomeArgument);
}
</pre>
이 함수는 매개변수를 두개 받는다. 첫 번째 매겨분수는 반드시 함수여야 하고 두 번째 매겨변수는 콜백 함수에 넘길 값이어야 한다. callSomeFunction에는 다음과 같이 어떤 함수든 넘길 수 있다.  
<pre>
function add10(num) {
  return num + 10;
}
var result1 = callSomeFunction(add10, 10);
alert(result1); //20

function getGreeting(name){
  return "Hello, " + name;
}
var result2 = callSomeFunction(getGreeting, "Nicholas");
alert(result2); // "Hello, Nicholas"
</pre>
callSomeFunction()은 범용 함수이므로 첫 번째 매개변수에 어떤 함수를 넘기든 관계없이 해당 함수에 두 번째 매개변수를 넘겨 실행한 결과를 반환한다. 다시 말해 <b>함수를 실행하지 않고, 단지 함수를 가리키는 포인터에 접근하기만 할 때는 괄호를 쓰면 안된다.(대괄호를 말하는 듯)</b> 함수가 함수를 반환하게 만들 수도 있으며 이런 패턴은 매우 유용하다 예를 들어 객체러 이루어진 배열이 있고 이 배열을 임의의 객체 프로퍼티를 기준으로 정렬하고 싶다고 가정하자 배열의 sort()매서드에 넘길 비교 함수는 매개변수를 두 개 밖에 받지 않아서 기준이 될 프로퍼티를 어떻게 식별 할 지 지정할 방법이 없다. 이 문제는 다음과 같이 프로퍼티 이름에 따라 비교 함수를 생성해 반환하는 함수를 만들어 해결 할 수 있다.  
<pre>
function createComparisonFunction(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName];
    var value2 = object2[propertyName];
    if (value1 < value2) {
      return -1;
    }else if(value1 > value2) {
      return 1;
    }else{
      return 0;
    }
  }
}
</pre>
함수가 좀 복잡해 보이기는 하지만 요점만 보면 return연산자 뒤에 내부함수가 잇는 형태이다 propertyName매개변수는 내부 함수에서 접근 할 수 잇고 대괄호 표기법을 써서 프로퍼티 값을 가져온다. 프로퍼티값을 가져오기만 하면 비교 하기는 쉽다. 다음 예제는 위 함수의 응용이다.  
<pre>
var data = [{name : "Zachary", age:28},{name:"Nicholas", age:29}];

data.sort(createComparisonFunction("name"));
alert(data[0].name); //Nicholas

data.sort(createComparisonFunction("age"));
alert(data[0].name); //Zachary
</pre>
두번째 코드에서 createComparisonFunction("age") 를 호출하면 age를 기준으로 하는 비교 함수가 호출되고 이를 통해 정렬한 인덱스 0 에는 name 프로퍼티가 Zachary이고 28인 객체가 오기때문에 Zachary가 호출된다.  

함수의 내부 구조  
함수 내부에서는 arguments, this 라는 특별한 객체가 있다. arguments객체는 이전에 설명했듯 배열과 비슷한 객체이며 함수에 전달된 매개변수를 모두 포함한다. arguments객체의 소유자인함수를 가리키는 포인터인 callee라는 프로퍼티도 있다 다음 계승 함수를 보자.  
<pre>
function factoroal(num) {
  if (num <= 1){
    return 1;
  }else{
    return num * factoroal(num-1)
  }
}
</pre>
계승 함수는 보통 위 예제처름 재귀 함수로 구현한다. 재귀 패턴은 함수이름이 정해진 뒤 바뀌지 않을 때는 아무 문제도 없다 바뀌 멀해 이 함수가 제대로 실행되려면 함수이름인 "factoroal"이 절대 바뀌지 않아야 한다 다음과 같이 arguments.callee를 쓰면 재귀 함수가 함수 이름에 의존하는 약점을 극복 할 수 있다.  
<pre>
function factoroal(num) {
  if (num <= 1) {
    return 1;
  }else{
    return num * arguments.callee(mun-1)
  }
}
</pre>
고쳐 쓴 factoroal()함수 내부에는 factoroal이란 이름에 대한 참조는 없으므로 함수에 대한 참조가 어떻게 바뀌더라도 재귀 호출은 명학하게 이루어진다. 다음 코드를 보자.  
<pre>
var trueFactorial = factoroal;
factoroal = function () {
  return 0;
}
alert(trueFactorial(5)); // 120
alert(factoroal(5)); //0
</pre>
이 코드에서는 변수 trueFactorial에 factoroal값을 할당했는데, 함수를 가리키는 두 번째 포인터를 생성한 것과 마찬가지이다. 그런다음 factoroal변수에는 무조건 0을 반환하는 함수를 할당하였다. factoroal()함수에서 arguments.callee를 호출하게 바꾸지 않았다면 trueFactorial()은 0을 반환하였을것 이지만 재귀 호출과 함수 이름사이의 연결고리를 고쳤기 때문운에 trueFactorial()은 정확하게 계승을 계산하며 0을 반환하는 함수는 factoroal()뿐이다.  
this역시 특별한 객체인데 함수가 실행중인 컨텍스트 객체에 대한 참조이다. 함수의 웹 페이지 전역스코프에서 호출하였다면 this객체는 window를 가리킨다.  
<pre>
window.color = "red";
var o = {color:"blue"};
function sayColor(){
  alert(this.color);
}
sayColor(); //"red"
o.sayColor = sayColor;
o.sayColor(); // blue
</pre>
sayColor() 전역에서 생성되었고 내부에서는 this객체를 참조한다 this값은 함수를 호출하는 시점에서 정해지므로 이 함수가 반환하는 값 역시 함수를 호출하는 시점에 따라 다르다. sayColor()를 전역 스코프에서 호출하면 "red"를 반환한다 this가 window를 가리키므로 this.color를 평가한 결과는 window.color이기 때문이다. 함수의 객체 o를 매서드로 할당한 다음 o,sayColor()를 호출하면 이 매서드에서 this는 o 를 가리키므로 this.color는 o.color로 평가되고 따라서 "blue"를 반환한다.  

함수 프로퍼티와 매서드  
ECMAscript의 함수는 객체이며 프로퍼티와 매서드를 가진다 모든 함수에 공통인 프로퍼티는 length와 prototype이다. length프로퍼티는 함수가 넘겨 받을것으로 예상되는 이름 붙은 매개변수의 숫자이다.  
<pre>
function sayName(name) {
  alert(name);
}
function sum(num1, num2) {
  return num1 + num2;
}
function sayHi() {
  alert("hi");
}
alert(sayName.length); //1
alert(sum.length)//2
alert(sayHi.length) //0
</pre>
이 코드는 함수를 세 개 정의 했는데 이름붙은 매개변수 숫자는 함수마다 다르다 sayName()함수는 매개변수를 하나만 예약 했기 때문에 length프로퍼티는 1 이다. 마찬가지로 sum()함수의 예약된 매개변수는 2개 이므로 length프로퍼티는 2이고 sayHi()함수에는 예약된 매개변수가 없으므로 length프로퍼티도 0이다.  
함수에는 apply()매서드와 call() 두 가지 매서드가 더 있다. 이들 매서드는 모두 소유자인 함수를 호출하면서 this를 넘기는데 결과적으로 함수 내부에서 this객체의 값을 바꾸는 것이나 마찬가지이다. apply()매서드는 매개변수로 소우자 함수에 넘길 this와 매개변수 배열을 받는다. 두 번째 매개변수는 Array의 인스턴스 일 수도 있고, arguments객체일 수도 있다.  
<pre>
function sum(num1, num2) {
  return num1 + num2;
}
function callSum1(num1, num2) {
  return sum.apply(this, arguments); // arguments객체를 넘김
}
function callSum2(num1, num2) {
  return sum.apply(this, [num1,num2]); // 배열을 넘김
}
alert(callSum1(10,10)) // 20
alert(callSum2(10,10)) // 20
</pre>
callSum1()은 sum()을 호출하면서 자신의 this와 arguments객체를 매개변스로 넘겼다. callSum2()역시 sum()을 호출하지만 이번에는 arguments객체가 아니라 매개변수 배열을 넘겼다.  
call()매서드도 apply()와 마찬가지로 동작하지만 매개변수를 전달하는 방식이 다르다 this가 첫 번째 매개변수인 점을 똑같지만 call()을 사용할 때는 반드시 매개변수를 나열해야 한다.  
<pre>
function sum(num1, num2) {
  return num1 + num2;
}
function callSum(num1, num2) {
  return sum.call(this, num1, num2)
}
alert(callSum(10,10)) //20
</pre>
매개변수로 전달할 데이터가 이미 배열 형태로 준비되어 있다면 apply()가 나을것이고 그렇지 않다면 call()이 더 좋다.  
ECMAscript 5에는 bind()매서드도 정의되어 있다. 이 매서드는 새 함수 인스턴스를 만드는데 그 this는 bind()에 전달 된 값이다.
<pre>
window.color = "red";
var o = {color : "blue"};
function sayColor() {
  alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor(); // blue
</pre>
이 예제에서는 sayColor()에서 bind()를 호출하면서 객체 o를 넘겨 objectSayColor()라는 함수를 생성하였다. objectSayColor()함수의 this는 o에 묶이므로 전역에서 함수를 호출하더라도 항상 "blue"를 표시한다.  

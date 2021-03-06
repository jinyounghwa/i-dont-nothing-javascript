##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
4. 변수와 스코프 메모리
이 장에서 다루는 내용    
변수의 원시 값과 참조 값    
실행 컨텍스트의 이해    
가비지 컬렉션의 이해  

원시 값과 참조 값    

ECMAscript의 변수는 원시값과 참조값에 데이터를 저장 할 수 있다. '원시값' 은 단순한 데이터이며 '참조값'은 여러 값으로 구성되는 객체이다.  
변수에 값을 할당하면 자바스크립트 엔진은 해당 값이 원시 데이터인지 참조 데이터인지 판단한다.
원시 타입은 undefined,null,불리언,숫자,문자열인데 이들 변수는 값으로 접근한다고 하며 실제 값을 조작하기 때문임.
참조 값은 메모리에 저장된 객체이다. 여타 언어와는 달리 자바스크립트는 메모리 위치에 직접 접근하는것을 허용하지 않으므로 객체의 메모리 공간을
직접 조작하는 일은 불가능 하다. 객체를 조작할 때는 사실 객체가 아니라 해당 객체의 '참조'를 조작하는 방법을 사용한다.

동적 프로퍼티  
원시값고 참조 값의 정의는 비슷하게 이루어진다 변수를 생성하고 값을 할당 한다. 참조값을 다룰 때는 언제든 프로퍼티와 메서드를 추가하거나 바꾸고 삭제 할 수 있다.
<pre>
var person = new Object();
person.name = "NiCholas";
alert(person.name) // "Nicholas"
</pre>
여기서는 객체를 생성 한 후 person이란 변수에 저장했다. 다음에는 name이란 프러퍼티를 추가하고 문자열 값 "Nicholas"를 할당했다.
이 시점부터 객체가 파괴되거나 프로퍼티를 명시적으로 제거하기 전까지는 해당 프로퍼티에 접근할 수 있다.
원시 값에는 프로퍼티가 없지만 추가하려 해도 에러가 생기지 않는다.
<pre>
var name = "Nicholas";
name.age = 27;
alert(name.age); //undefined
</pre>
위 코드에서는 문자열 name에 age란 프로퍼티를 정의했고 값으로 27을 할당했다. 하지만 그 프로퍼티는 다음줄에서 사라진다. 동적으로 프로퍼티를 추가 할 수 있는 값은 참조 값 뿐이다.

값 복사  
원시 값과 참조값은 저장되는 방식 외에도 변수에서 다른 변수로 값을 복사할 때도 다르게 동작한다. 원시 값을 다른 변수로 복사할 때 현재 저장된 값을 새로 생성한 다음 새로운 변수에 복사한다.
<pre>
var num1 = 5;
var num2 = num1;
</pre>
여기에서 num1에는 값 5가 저장되어 있다. num2를 num1로 초기화 하면 num2에도 값5가 저장된다. 이 값은 복사된 것이기 때문에 num1에 저장된 값과는 완전히 분리되어 있다.
이 과정을 아래에 그림에 표현하였다. 위의 그림으 복사하기 전의 변수 객체이고 아래의 그림이 복사한 뒤의 변수 객체를 나타낸다.  

![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/obj1.png)  

참조값을 변수에서 다른 변수로 복사하면 원래 변수에 들어있던 값이 다른 변수에 복사되기는 마찬가지이다. 그 차이는 객체 자체가 아니라 힙(heep)에 저장된 객체를 가리키는 포인터라는 점이다. 조작이 끝나면 두 변수는 정확히 같은 객체를 가리킨다. 따라서 다음 예제처럼 한쪽을 조작하면 다른 쪽에도 반영된다.
<pre>
var obj1 = new Object();
var obj2 = obj1;
obj.name = "Nicholas";
alert(obj2.name); //"Nicholas"
</pre>
이 예제에서 변수 obj1에 object의 인스턴스를 할당했다. 이 값을 obj2에 복사했으므로 두 변수는 이제 같은 객체를 가리킨다. obj1에 name프로퍼티를 정의하면 obj2에서도 해당 프로퍼티에 접근할 수 있는 두 변수가 같은 객체를 가리키기 때문이다.

매개변수의 전달  
ECMAscript의 함수 매개변수는 모두 값으로 전달된다. 함수 외부에 있는 값은 함수 내부의 매개변수에 복사되는데, 이는 변수 사이에서 값을 복사하는것과 마찬가지이다. 값이 원시 값이라면 변수사이에서 원시값을 복사하는것과 마찬가지다. 매개변수는 오직 값으로만 전달된다.  
매겨변수를 값 형태로 넘기면 해당 값은 지역 변수에 복사된다. 즉 이름 붙은 매개변수로 복사되며 ECMAscript에서는 arguments객체의 한 자리를 차지한다. 매개변수를 참조 형태로 넘기면 메모리 상의 값의 위치가 지역 변수에 저장되므로 지역변수를 변경하면 함수 바깥에도 해당 변경내용이 반영된다.
<pre>
function addTen(num) {
  num +=10;
  return num;
}
var count = 20;
var result = addTen(count);
alert(count); //20-변동 없음
alert(result); //30
</pre>
이 코드에서 함수 addTen()은 매개변수 num을 넘겨 받는데 이 변수는 기본적으로는 지역 변수이다. 코드를 실행하면 변수 count가 매개변수로 전달된다. 변수의 값은 20이었는데 이 값이 매개변수 num에 복사되어 addTen()내부에서 쓰인다. 함수 내부에서는 매개변수 num에 10을 더해 값을 바꾸지만 함수 외부에 있는 count는 바뀌지 않는다. 매개변수 num과 count는 이제 상관없는 값이 되었다. 물론 우연히 같은 값을 가질 수는 있으나 num을 참조로 전달 했다면 함수 내부의 변화를 반영해 count의 값도 30으로 바뀌었을 것이다 이런 사실은 숫자 같은 원시 값에서는 분명히 드러나지만 객체에서는 그닥 명확하지 않아 보인다. 다음 코드를 보자
<pre>
function setName(obj) {
  obj.name = "Nicholas";
}
var person = new person();
setName(person);
alert(person.name); // "Nicholas"
</pre>
이 코드에서는 객체를 만들어 변수 person에 저장했다. 이 객체를 setName() 함수에 넘기면 함수는 해당 객체를 obj에 복사한다. 함수 내부에서는 obj와 person이 모두 같은 객체를 가리킨다. 결과적으로 obj는 함수에 값 형태로 전달 되었지만, 참조를 통해 객체에 접근한다. 함수 내부에서 obj객체에 name프로퍼티를 추가하면 함수 외부의 객체에도 반영되는데 obj가 가리키는것은 전역객체이기 때문이다. 명확하게 전달하기 위해 다음과 같이 수정해보자
<pre>
function setName(obj) {
  obj.name = "Nicholas";
  obj = new Object;
  obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name); // "Nicholas"
</pre>
이 예제가 이전과 다른 점은 setName()에 코드를 두 줄 추가해서 obj를 새로온 객체로 재정의 했다는 것이다. person을 setName()에 전달하면 name프로퍼티에 "Nicholas"가 전달된다. 그런다음 변수 obj는 새 객체를 가르키며 새 객체의 name프로퍼티는 "Greg"가 저장된다. person이 참조로 전달되었다면, person이 가리키는 객체는 자동으로 name프로퍼티가 "Greg" 인 새 객체로 바뀌었을 것이다. 그러나 person에 다시 접근하면 그 값은 "Nicholas"이다. 함수에 값을 전달하였기 때문에 매개변수의 값이 바뀌었음에도 불구하고 원래 객체에 대한 참조를 그대로 유지한 것이다. 함수 내부에서 obj를 덮어쓰는 obj는 지역 객체를 가리키는 포인터가 된다 이 지역 객체는 함수가 실행을 마치는 즉시 파괴된다. "ECMAscript에서 함수 매개변수는 지역 변수와 다를것이 없다고 생각하라(중요)"  

타입 판별  
이전 장에서 소개한 typeof 연산자는 변수가 원시 타입인지 파악하기 최상이다. 좀더 정확하게 말하면 typeof연산자는 특정 변수가 문자열이나 숫자, 불리언, undefined라면 정확한 타입을 알 수 있다. 값이 객체이거나 null 이면 typeof는 다음과 같이 "Object"를 반환한다.  
<pre>
var s = "Nicholas";
var b = true;
var i = 22;
var u;
var n = null;
var o = new Object;

alert(typeof s); // string
alert(typeof b); // number
alert(typeof i); // boolean
alert(typeof u); // undefined
alert(typeof n); // object
alert(typeof o); // object
</pre>
typeof는 원시 값에 대해서는 잘 동작하지만 참조 값에 대해서는 별 쓸모가 없다. 일반적으로 값이 객체인지 아닌지는 별 관심이 없으며 정말 필요한 건 어떤 타입의 객체인지 이다 ECMAscript의 instanceof연산자는 이런상황에 도움이 되며 다음 문법에 따라 사용한다.
<pre>
'result' = 'variable' instanceof 'constructor'
</pre>
 instanceof연산자는 변수가 주어진 참조 타입의 인스턴스 일 때 (프로토타입 체인으로 판단하는데 8장에서 자세하게 설명) true를 반환한다.
 <pre>
alert(person instanceof Object); // person변수가 Object의 인스턴스인가?
alert(colors instanceof Array); // colors변수가 Array의 인스턴스인가?
alert(pattern instanceof RegExp); // pattern변수가 RegExp의 인스턴스인가?
 </pre>
 모든 참조 값은 Object의 인스턴스인 것으로 정의되어 있으므로 참조 값이나 Object생성자에 instanceof연산자를 사용하면 항상 true를 반환한다. 반면 원시값은 object인스턴스가 아니므로 원시값에 instanceof연산자를 사용하면 항상 false를 반환한다.  

 실행 컨텍스트와 스코프   
 실행컨텍스트는 일명 컨텍스트라고 불리며 자바스크립트에서는 중요한 개념이다. 변수나 함수의 실행 컨텍스트는 다른 데이터에 접근 할 수 있는지, 어떻게 행동하는지를 규정한다. 각 실행 컨텍스트에는 다른 데이터에 접근 할 수 있는지, 어떻게 행동하는지를 규정한다. 각 실행 컨텍스트에는 변수 객체가 연결되어 있으며 해당 컨텍스트에서 정의된 모든 변수와 함수는 이 객체에 존재한다. 이 객체를 코드에서 접근 할 수는 없지만 이면에서 데이터를 다룰 때 이 객체를 이용한다.  
 가장 바깥쪽에 존재하는 실행 컨텍스트는 전역 컨텍스트 이다. ECMAscript를 구현한 환경에 따라 이 컨텍스트를 부르는 이름이 다르다. 함수를 호출하면 족자적인 실행 컨텍스트가 생성된다.코드 실행이 함수로 들어갈 때마다 함수의 컨텍스트가 컨텍스트 스택에 쌓입니다. 함수 실행이 끝나면 해당 컨텍스트를 스택에서 꺼내고 컨트롤을 이전 컨텍스트에 반환한다.  
 컨텍스트에서 코드를 실행하면 변수 객체에 스코프체인이 만들어 진다. 스코프체인의 목적은 실행 컨텍스트가 접근 할 수 있는 모든 변수와 함수에 순서를 정의하는 것이다. 스코프체인의 앞쪽은 항상 코드가 실행되는 컨텍스트의 변수 객체이다. 컨텍스트가 함수라면 활성화 객체를 변수 객체로 사용한다. 활성화 객체는 항상 arguments변수 단 하나로 시작하며 이 변수는 전역 컨텍스트에는 존재하지 않는다. 변수 객체의 다으 ㅁ순서는 해당 컨텍스트를 포함하는 컨텍스트(부모 컨텍스트)이며 그 다음에는 다시 부모의 부모 컨텍스트이다. 이런식으로 전역에 도달할 때까지 계속한다. 식별자를 찾을 때는 스코프 체인 순서를 따라가면서 해당 식별자 이름을 검색한다. 식별자를 찾을 수 없다면 에러가 발생한다.

 <pre>
var color = "blue";
function changeColor() {
  if (color === "blue") {
    color = "red";
  }else {
    color = "blue";
  }
}
changeColor();
 </pre>
이 예제에서 함수 changeColor()의 스코프 체인에는 객체가 두개 들어 있다 하나는 함수 자체의 변수 객체(arguments는 여기에 정의된다.)이며 다른하나는 전역 컨텍스트의 변수 객체이다 변수 color는 함수의 스코프 체인에서 찾을 수 있으므로 접근 가능하다.  
또한 로컬 컨텍스트에는 지역변수와 전역변수 모두를 사용 할 수 있다.
<pre>
var color = "blue";
function changeColor() {
  var anotherColor = "red";
  function swapColors () {
    var tempColor = anotherColor;
    anotherColor = color;
    color = tempColor;
    // color, anotherColor, tempColor모두 접근 가능
  }
  // color, anotherColor, tempColor모두 접근 불가
  swapColors();
}
//color 만 접근가능
changeColor();
</pre>
이 코드에서는 실행컨텍스트가 세 개 있다. 전역 컨텍스트, changeColor()의 로컬 컨텍스트, swapColors()의 로컬 컨텍스트 세 개 이다. 전역컨텍스트에는 color변수와 changeColor() 함수가 포함된다 changeColor()의 로컬 컨텍스트에는 anotherColor()변수와 swapColors()함수만 있지만 전역 컨텍스트의 color변수에도 접근 할 수 있다. swapColors()의 로컬 컨텍스트에 있는  tempColor()변수는 오직 이 컨텍스트에서만 접근 할 수 있다. 전역컨텍스트나 swapColors()로컬 컨텍스트에서는 tempColor에 접근할 수 있다. 하지만 swapColors()에서는 부모인 다른 두 컨텍세트의 변수에 자유로이 접근 할 수 있다.  

![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/sc.png)  

이 그림에서 각 사각형은 실행 컨텍스트를 나타낸다.내부 컨텍스트는 스코프 체인을 통해 외부 컨텍스트 전체에 접근 할 수 있지만 외부 컨텍스트는 내부 컨텍스트에 대해 전혀 알 수 없다. 컨텍스트 사이의 연결은 선형이며 순서가 중요하다. 각 컨텍스트는 체인을 따라 상위 컨텍스트에서 변수나 함수를 검색할 수는 있지만, 스코프 체인을 따라 내려가며 검색할 수는 없다. swapColors()이 로컬 컨텍스트 스코프 체인에는 객체가 세 개 있다.하나는 swapColors()의 변수 객체이고 다른하나나는 changeColor()의 변수 객체이며 마지막은 전역 변수 객체이다. swapColors()의 로컬 컨텍스트는 자신의 변수 객체에서 변수나 함수 이름을 검색한 다음 찾지 못하면 스코프 체인을 따라 한 단계 씩 올라간다 changeColor()의 로컬 컨텍스트 스코프 체인에는 객체가 두개 있다 하나는 변수 객체이며 다른 하나는 전역 변수 객체이다. 즉 changeColor()의 로컬 컨텍스트에서는 swapColors()의 컨텍세트에 접근 할 수 없다.(함수 매개 변수도 변수로 간주되며 실행 컨텍스트에 있는 다른 변수와 같은 변수 규칙을 따른다.)  

스크포 체인 확장  
실행 컨텍스트에는 전역 컨텍스트와 함수 컨텍스트 두 가지 타임만 있지만(eval()을 호출 할 때 생성되는 세번째 타입이 있긴하다) 스코프 체인을 확장 할 수 있는 방법도 있다. 특정 문장은 스코프체인 앞 부분에 임시로 변수 객체를 만들며 해당 변수 객체는 코드 실행이 끝나면 사라진다 이렇게 임시 객체가 생성되는 경우는 다음 두 가지이다.
+ try-catch 문의 catch블록
+ with문
<pre>
function buildUrl() {
  var qs = "?debug = true";
  with(location) {
    var url = href + qs;
  }
  return url;
}
</pre>
여기에선 with문이 location객체에 적용되므로 location객체가 스코프 체인에 추가된다. buildUrl()함수에서 정의하는 변수는 qs하나 뿐이다. 변수 href를 참조하는 문장은 사실 with문이 추가한 변수 객체에 들어 있는 location.href변수를 참조하는 겁니다. 변수 qs를 참조할 때는 buildUrl()함수에서 정의한 변수를 참조하는데 이 변수는 이 변수는 컨텍스트의 변수 객체에 들어 있다 with문 내부에서 선언한 변수 url은 함수에 컨텍스트로 편입되며 따라서 함수 값으로 반환 할 수 있다.  

자바스크립트에는 블록레벨 스코프가 없다.
<pre>
for (var i = 0; i < 10; i++) {
  doSomething(i);
}
alert(i) // 10
</pre>
블록 레벨 스코프를 지원하는 언어에서는 for문의 초기화 부분에서 선언한 변수가 오직 for문의 컨텍스트안에서만 존재한다. 자바스크립트에서는 for문에서 생성한 i 변수가 루프 실행이 끝난 후에서 존재한다.  

변수 선언  
var를 사용해 선언한 변수는 자동으로 가장 가까운 컨텍스트에 추가된다. 함수 내부에서는 함수의 로컬 컨텍스트가 가장 가까운 컨텍스트이며 with문 내부에서는 함수 컨텍스트가 가장 가까운 컨텍스트 이다. 다음과 같이 변수를 선언하지 않은 채 초기화 하면 해당 변수는 자동으로 전역 컨텍스트에 추가된다.
<pre>
function add(num1, num2) {
  var sum = num1 + num2;
  return sum;
}
var result = add(10, 20) //30
alert(sum); //sum은 유효한 변수가 아니므로 에러가 발생한다.
</pre>
여기에서 함수 add()는 sum이라는 지역변수를 생성해서 계산 결과를 저장한다. 변수 sum이 함수 값으로 반환되긴 하지만 함수 밖에서는 이 변수에 접근 할 수 없다. 다음과 같이 var키워드를 생략하면 add()함수를 호출한 다음부터는 변수 sum에 접근 할 수 있다.  
<pre>
function add(num1, num2) {
  sum = num1 + num2;
  return sum;
}
var result = add(10, 20); //30
alert(sum); //30
</pre>
여기에서 var를 써서 변수 sum을 선언하지 않은 채 값을 할당하여 초기화 했다. add()를 호출하면 전역 컨텍스트에서 sum이 생성되며 함수가 완전히 실행된 뒤에도 남아있어서 나중에 사용할 수 있다.  

식별자 검색  
컨텍스트 안에서 식별자를 참조하려 하면 먼저 검색부터 해야 한다. 검색은 스코프체인 앞에서 시작하며, 주어진 이름으로 식별자를 찾는다. 로컬 컨텍스트 에서 식별자 이름을 찾으면 검색을 멈추고 변수를 설정한다. 변수 이름을 찾지 못하면 스코프 체인을 따라 검색을 계속한다. 스코프체인 내부의 객체에서도 프로토타입체인이 있으므로 각 객체의 프로토타입 체인에 따라 검색 할 수 있다. 전역컨텍스트의 변수 객체에 도달할 때까지 이 과정을 ㄸ라 검색을 계속한다. 전역 컨텍스트 변수 객체에서도 식별자를 찾지 못하면 정의되지 않은것으로 반환한다.
<pre>
var color = "blue";
function getColor() {
  return color;
}
alert(getColor()); // "true"
</pre>
이 예제에서는 함수를 호출할 때 변수 color을 참조한다 그 시점에서 두 단계 검색을 시작한다. 먼저 getColor()의 변수 객체에서 color라는 식별자 이름을 검색한다. getColor()의 변수 객체에서 찾지 못하면 다음(전역컨텍스트)변수 객체에서 color식별자를 검색한다. 이 변수 객체에 color변수가 정의되어 있으므로 검색을 마친다.  

![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/sc2.png)  

이런 검색 과정을 보면 지역 변수를 참조할 때는 다음 변수 객체를 검색하지 않도록 자동으로 검색을 멈춤을 알 수 있다. 달리말해 다음과 같이 식별자가 로컬 컨텍스트에 정의되어 있으면 부모 컨텍스트에 같은 이름의 식별자가 있다 해도 참조 할 수 없다.
<pre>
var color = "blue";
function getColor() {
  var color = "red";
  return color;
}
alert(getColor()); // "red"
</pre>
수정한 코드에서는 color가 getColor()함수 안에서 지역 변수로 선언되었다. 함수를 호출하면 변수 color가 선언된다. 함수의 두 번째 줄 var color = "red"; 는 지역 변수color를 로컬 컨텍스트에 선언한다. 로컬 컨텍스트에서 검색을 시작하는데 color란 이름의 변수를 찾고 그 값은 "red"이다. 변수를 찾았으므로 검색을 마치고 지역 변수를 사용한다 즉 함수는 red를 반환한다 일단 color을 지역 변수로 선언한 이후에는 해당 컨텍스트에서 전역 컨텍스트의 color변수를 이용 할 수 없다. 전역 컨텍스트이ㅡ color변수를 이용하려면 window.color이라고 명시해야 한다.  

참조 카운팅  
널리 쓰이지는 않지만 참조카운팅이라는 가비지 멀렉션 방법도 있다.
<pre>
var element = document.getElementById("some_element");
var myObject = new Object();
myObject.element = element;
element.someObject = myObject;
//순환참조를 피하려면 네이티브 자바스크립트 객체나 DOM요소를 다 사용한후에는 둘 사이 연결을 끊어준다.
myObject.element = null;
element.someObject = null;
</pre>

메모리 관리  
가능한 한 최소한의 메모리만 사용해야 페이지 성능을 올릴 수 있다. 필요 없어진 데이터는 null을 할당하여 참조를제거 하는 편이 좋다. 수동으로 참조를 제거하는 대상은 주로 전역 변수 및 전역 객체의 프로퍼티이다. 지역 변수는 컨텍스트를 빠져나가는 순간 자동으로 참조가 제거된다.  
<pre>
function createPerson(name) {
  var localPerson = new Object();
  localPerson.name = name;
  return localPerson;
}
var globalPerson = createPerson("Nicholas");
// globalPerson 을 사용하는 코드
globalPerson = null;
</pre>
이코드에서 변수 globalPerson의 값은 createPerson()함수가 반환하는 값이다. createPerson()함수는 localPerson객체를 만들고 name프로퍼티를 추가한다. createPerson()함수는 변수 localPerson을 반환하며 이 값은 globalPerson에 할당된다. createPerson()함수가 실행을 마치는 순간 localPerson은 컨텍스트를 벗어남으로 명시적으로 참조를 제거할 필요는 없다 반면 globalPerson은 전역 변수이기때문에 필요없어진 순간에 마지막 줄과 같이 직접 참조를 제거해야 한다.  
변수에서 참조를 제거한다 해서 할당한 메모리가 자동으로 반환되는건 아니다. 참조 제거의 요저믕ㄴ 값의 컨텍스트를 없애서 다음에 가비지 콜렉션을 실행 할 때 해당 메모리를 회수하도록 하는 것이다. 

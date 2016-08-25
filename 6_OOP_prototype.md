##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
프로토타입 패턴
모든 함수는 prototype 프로퍼티를 가진다. 이 프로퍼티는 해당 참조 타입의 인스턴스가 가져야 할 프로퍼티와 메서드를 담고 있는 객체이다. 이 객첸느 생성자를 호출 할 때 생성되는 객체의, 문자 그대로, 프로포타입이다. 프로토타입의 프로퍼티와 메서드는 객체 인스턴스 전체에서 공유된다는 점이 프로토 타입의 장점이다. 객체 정보를 생성자에 할당하는 대신 다음과 같이 직접적으로 프로토타입에 할당 할 수 있다.  
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function () {
  alert(this.name);
}
var person1 = new Person();
person1.sayName(); //"Nicholas"

var person2 = new Person();
person2.sayName(); //"Nicholas"

alert(person1.sayName == person2.sayName); //true
</pre>
여기에서 프로퍼티들과 sayName()매서드는 Person의 prototype프로퍼티에 직접 추가되었고 생성자 함수는 비워 두었다. 생성자 함수가 비어 있지만 생성자를 호출해 만든 객체에도 프로퍼티와 메서드가 존재한다. 생성자 패턴과는 달리 프로퍼티와 매서드를 모든 인스턴스에서 person1과 person2는 같은 프로퍼티 집합에 접근하며 같은 sayName()함수를 공유한다.  

프로토타입 함수는 어떻게 움직이는가  
함수가 생성될 때마다 prototype프로퍼티 역시 특정 규칙에 따라 생성된다. 기본적으로 모든 프로토타입은 자동으로 constructor프로퍼티를 갖는데 이 프로퍼티는 해당 프로토타입이 프로퍼티로서 소속된 함수를 가리킨다. 예를 들어 이전 예제에서 Person.prototype.constructor은 Person을 가리킨다 다음에는 생성자에 따라 각종 프로퍼티와 메서드가 프로로토 타입에 추가된다.  
커스텀 생성자를 정의하면 해당 프로토 타입은 단지 constructor프로퍼티만 가지며 다른 메서드는 Object에서 상속한다. 생성자를 호출해서 인스턴스를 생성 할 때마다 해당 인스턴스 내부에는 생성자의 프로토 타입을 가리키는 포인터가 생성된다. 이 포인터를 [[Prototype]]이라고 부른다. 스크립트에서 [[Prototype]]에 접근하는 표준은 없지만 크롬과 파이어폭스에는 __proto__ 라는 프로퍼티를 지원한다. 인스턴스와 직접연결되는것은 생성자의 프로토타입이지 생성자가 아님을 이해할 것.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/proto61.png)  

위 그림은 Person 생성자와 Person프로토타이브 Person의 두 인스턴스 사이의 관계를 나타낸다. Person.prototype은 프로토타입 객체를 가리키지만 Person.prototype.constructor는 Person을 가리키고 있다. 프로토타입은 constructor프로퍼티를 가지고 기타 추가된 프로토타입도 가진다. Person의 인스턴스 person1과 person2는 Person.prototype을 가리키는 내부 프로퍼티를 가질 뿐 생성좌아 직접 연결되지는 않는다. 이 이슨턴스들에는 아무 프로퍼티나 매서드도 없는데 person1.sayName()이 동작한다. 이는 객체 프로퍼티를 검색하는 매커니즘 때문이다.  
[[Prototype]]은 구현 환경에 따라 접근 불가능할 수도 있지만, 객체 사이에 프로토타입 연결이 존재하는지는 isPrototypeOf()매서드를 통해 알 수 있다 요약하면, isPrototypeOf()은 다음과 같이 [[Prototype]]이 자신을 호출하는 프로토타입을 가리킬 때 true를 반환한다.  
<pre>
alert(Person.prototype.isPrototypeOf(person1)); //true
alert(Person.prototype.isPrototypeOf(person2)); //true
</pre>
ECMAscript 5판에서는 [[Prototype]]값을 반환하는 Object.getPrototypeof()라는 매서드가 추가되었다.  
<pre>
alert(Object.getPrototypeOf(person1) == Person.prototype); //true
alert(Object.getPrototypeOf(person1).name);// "Nicholas"
</pre>
코드의 첫줄은 단순히 Object.getPrototypeOf()에서 반환한 객체가 해당 객체의 프로토타입이 맞는지만 확인한다. 두번째 줄은 프로토타입의 name프로퍼티값인" Nicholas"를 가져온다. Object.getPrototypeOf()를 쓰면 객채의 프로토타입을 이용한 상속구현에 중요한 역할을 한다. 객체에서 프로퍼티를 읽으려 할 때마다 해당 프로퍼티 이름으로 찾으려고 검색한다. 검색은 객체 인스턴스 자체에서 시작한다. 인스턴스 프로퍼티 이름을 찾으면 그 값을 반환한다. 프로퍼티를 찾지 못하면 포인터를 프로토타입으로 올려서 검색을 계속한다. 프로퍼티를 프로토타입에서 찾으면 그 값을 반환한다. 따라서 person1.sayName()를 호출하면 두 단계가 발생한다. 첫단계에서 자바스크립트 엔진은 person1인스턴스에 sayName이라는 프로퍼티가 있는지 확인한다. 찾을 수 없으므로 person1프로토타입에 프로퍼티 sayName이 있는지 확인한다. 프로토타입에서 프로퍼티를 찾을 수 있으므로 프로토타입에 저장된 함수가 실행된다. person2.sayName()을 호출하면 똑같은 방식으로 검색하여 실행하고 같은 결과를 반환한다. 프로토타입은 이런 식으로 여러 객체 인스턴스에서 프로퍼티와 매서드를 공유한다.   

객체 인스턴스에서 프로토타입에 있는 값을 읽을 수는 있지만 수정은 불가능하다. 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 해당 프로퍼티는 인스턴스에 추가되며 프로토타입까지 올라가지는 않는다.  
<pre>
function Person() {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function () {
  alert(this.name);
}

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
delete(person1.name); // "Greg" -인스턴스 에서
alert(person2.name); // "Nicholas" - 프로토타입에서
</pre>
수정한 예제에서 delete연산자로 값 "Greg"가 설정되어 있던 person1.name을 삭제하였다. 이렇게 하면 프로토타입의 name프로퍼티를 가리키는 링크가 복구되므로 이후 person1.name에 접근할 때는 prototype프로퍼티의 값이 반환된다.  
hasOwnProperty()매서드는 프로퍼티가 인스턴스에 존재하는지 프로토타입에 존재하는지 확인한다. 이 매서드는 Object로부터 상속한 것인데, 다음과 같이 해당 프로퍼티가 객체 인스턴스에 존재할 때만 true를 반환한다.  
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
person.prototype.sayName = function (){
  alert(this.name);
}
var person1 = new Person();
var person2 = new Person();

alert(person.hasOwnProperty("name")); //false

person1.name = "Greg";
alert(person1.name); //"Greg" - 인스턴스에서
alert(person1.hasOwnProperty("name")); // false

alert(person2.name); // "Nicholas" - 프로토타입에서
alert(person2.hasOwnProperty("name")); //true

delete person1.name;
alert(person1.name); // "Nicholas" - 프로토타입에서
alert(person1.hasOwnProperty("name")) // false
</pre>
hasOwnProperty()를 추가함으로서 인스턴스의 프로퍼티에 접근 하는지 프로토 타입의 프로퍼티에 접근하는지를 명확히 할 수 있다. person1.hasOwnProperty("name")을 호출했을 때 true를 반환하는 경우는 person1의 name프로퍼티를 덮어 썻을 때 뿐이며, 이 시점부터 name은 '프로토타입'프로퍼티가 아니라 '인스턴스의 프로퍼티'이다. (ECMAscript 5판의 Object.getOwnPropertyDescriptor()매서드는 인스턴스 프로퍼티만 동장한다. 프로토타입 프로퍼티의 서술자 객체를 얻으려면 반드시 프로토타입 객체에서 Object.getOwnPropertyDescriptor()를 직접 호출해야 한다.)  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/proto62.png)

프로토타입과 in 연산자  
in연산자에는 두가지 쓰임이 있다. 하나는 그 자체로 사용하는 경우이고 다른 하나는 for - in 루프에서 사용하는 경우이다. 그 자체로서 사용하는 경우 in 연산자는 주어진 이름의 프로퍼티를 객체에서 접근 할 수 있을 때, 다시 말해 해당 프로퍼티가 인스턴스에 존재하든 프로토타입에 존재하든 모두 true 를 반환한다.   
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
person.prototype.sayName = function (){
  alert(this.name);
}
var person1 = new Person();
var person2 = new Person();

alert(person1.hasOwnProperty("name")); // false
alert("name" in person1);// true

person1.name = "Greg";
alert(person1.name); // "Greg" - 인스턴스에서
alert(person1.hasOwnProperty("name")); // true
alert("name" in person1)// true

alert(person2.name); // "Nicholas" - 프로토타입에서
alert(person2.hasOwnProperty("name")); //false
alert("name" in person2); //true

delete person1.name;
alert(person1.name); // "Nicholas" - 프로토타입에서
alert(person1.hasOwnProperty("name")) // false
alert("name" in person1) // true
</pre>
코드의 실행 과정을 통틀어 name프로퍼티는 객체에서 직접이든 프로토타입에서든 접근 할 수 있다. 따라서 " name" in person1은 프로퍼티가 인스턴스에 존재하지 않아도 true를 반환한다. 객체프로퍼티가 프로토타입에 존재하는지는 다음과 같이 hasOwnProperty()와 in 연산자를 조합해서 알 수 있다.  
<pre>
function hasOwnProperty(object, name) {
  return !object.hasOwnProperty(name) && (name in object);
}
</pre>
in 연산자는 객체에서 프로퍼티에 접근 할 수 있지만 하면 항상  true를 반환하고 hasOwnProperty()는 프로퍼티가 인스턴스에 존재할 때만 true를 반환하므로, in연산자가 true를 반환하고 hasOwnProperty()는 false를 반환한단다면 해당 프로퍼티가 프로타타입에 존재하는 것이다.  
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
person.prototype.sayName = function (){
  alert(this.name);
}
var person1 = new Person();
alert(hasOwnProperty(person, "name")); //true

var person2 = new Person();
alert(hasOwnProperty(person,"name")); //false
</pre>
이 코드에서는 프로토타입에 name프로퍼티가 존재하므로 hasOwnProperty()는 true를 반환한다. name프로퍼티를 덮어쓰는 순간 이 프로퍼티는 인스턴스에 존재하게 되므로 hasOwnProperty()논 false를 반환한다. name프로퍼티가 아직 프로토 타입에 존재하지만 인스턴스 프로퍼티가 존재하므로 더 이상 사용하지 않는 것이다.   
for - in 루프를 사용할 때는 객체에서 접근 할 수 있고 나열 가능한 프로퍼티를 반환하는데, 여기에는 인스턴스 프로퍼티와 프로토타입 프로퍼티가 모두 포함된다. 인스턴스 프로퍼티 중 나열가능한 prototype프로퍼티([[Enumerable]]이 false로 지정된 프로퍼티) 를 '가리고 있는'프로퍼티 역시 for-in 루프에서 반환되는데 개발자가 지정한프로퍼티는 항상 나열가능하도록 한 규칙 때문이다.  
<pre>
var o = {
  toString : function(){
    return "my Object";
  }
};
for (var prop in o ) {
  if (prop == "toString"){
    alert("Found toString"); // 인터넷 익스플로러에는 표시되지 않음
  }
}
</pre>
이 코드를 실행하려면 alert대화상자가 단 한번 표시되면서 toString()매서드를 찾았다고 알려야 한다. o 객체의 인스턴스프로퍼티 toString()은 프로토타입의 toString()매서드 (나열불가능)를 가린다. 같은 버그가 기본적으로 나열 불가능하도록 지정 된 모든 프로퍼티와 매서드 즉, hasOwnProperty(), propettyIsEnumerable(),toLocalString(),toString(),valueOf()에 적용된다.  
ECMAscript 5판의 Object.keys()매서드를 통해 객체 인스턴스에서 나열 가능한 프로퍼티의 전제 목록을 얻을 수 있다. Object.keys()매서드는 객체를 매개변수로 받은다음 나열 가능한 프로퍼티 이름을 문자열로 포함하는 배열을 반환한다.  
<pre>
function Person () {
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
person.prototype.sayName = function (){
  alert(this.name);
}
var keys = Object.keys(Person.prototype);
alert(keys); // name,age,job,sayName

var p1 = new Person();
p1.name = "Rob";
p1.age = 31;
var p1keys = Object.keys(p1);
alert(p1keys); // "name,age"
</pre>
여기서는 keys변수에 "name","age","job","sayName"이 들어 있는 배열을 할당하였다. 일반적인 for-in문에서는 순서대로 프로퍼티가 나열된다 Person의 인스턴스에서 Object.keys()를 호출하면 인스턴스 프로퍼티 name과 age만 들어 있는 배열을 반환한다.
나열 가능 여부와 관계없이 인스턴스 프로퍼티 전체 목록을 얻으려면 Object.getOwnPropertyNames()매서드를 같은 방법으로 사용한다.  
<pre>
var keys = Object.getOwnPropertyNames(Person.prototype);
alert(keys); // "constructor", "name", "age", "job", "sayName"
</pre>

나을 불가능한 constructor프로퍼티가 결과 목록에 들어 있다. Object.keys()와 Object.getOwnPropertyNames()모두 for-in대신 쓸 수 있다. 다만 인터넷 익스플로러 9이상에서 이 매서드를 지원한다.  

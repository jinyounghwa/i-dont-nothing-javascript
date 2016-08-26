##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
상속  
상속은 객체지향 프로그래밍과 관련하여 자주 설명하는 개념이다 대부분의 객체지향 언어는 시그너처만을 상속하는 인터페이스 상속과 실제 메서드를 상속하는 구현상속 두가지 타입을 지원하지만 ECMAScriot 에는 시그니처가 없으므로 인터페이스 상속은 불가능하며 구현상속만 프로토타입 체인을 통해 지원한다.  

프로토타입 체인  
ECMA-262 는 프로토타입 체인을 ECMAScriot의 우선적 상속방법으로 설명한다. 프로토타입 체인의 기본 아이디어는 프로토타입개념을 이용해 두 가지 참조타입 사이에서 프로퍼티와 매서드를 상속한다는 것이다. 모든 생성자에는 생성자 자신을 가리키는 프로토타입 객체가 있으며 인스턴스는 프로토타입을 가리키는 내부 포인터가 있다. 그렇다면 프로토타입이 사실 다른 타입의 인스턴스라면 어쩔텐가? 이런 경우 프로토타입(A) 자체에 다른 프로토타입(B)를 가리키는 포인터가 있을 것이며 B에는 또다른 생성자를 가리키는 포인터가 있을 것이다. 프로토타입 B역시 다른 타입의 인스턴스라면 이런 패턴이 계속 연결되어 인스턴스와 프로토타입을 잇는 체인이 형성될 것이다. 프로토타입은 바로 이런 개념이다.  
<pre>
function SuperType () {
  this.property = true;
}
SuperType.prototype.getSuperValue = function () {
  return this.property;
};
function SubType() {
  this.subpropety = false;
}
// SuperType을 상속
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function () {
  return this.subpropety;
};
var instance = new SubType();
alert(instance.getSuperValue()); // true
</pre>
이 코드는 SuperType과 SubType 두 가지 타입을 정의하는데 각 타입에는 프로퍼티와 매서드가 단 한개씩만 있다. 두 타입의 주요 차이는 SubType이 SuperType의 새 인스턴스를 생성하여 SuperType을 상속하여 이를 SubType.prototype에 할당한다는 점이다. 이렇게 하면 원래 프로토타입을 새로운 객체로 덮에 쓰는데, SuperType의 인스턴스에 존재했을 프로피티와 매서드가 SubType.prototype에도 존재하게 된다. 상속이 일어난 다음에는 SubType.prototype에도 존재하게 된다. 상속이 일어난 다음에는 SubType.prototype에 매서드를 추가하는데 SubType에서 상속한 매서드는 그대로 유지된다 인스턴스와 생성자, 프로토타입 사이의 관계를 아래 그림으로 확인 할 수 있다.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/proto64.png)

SubType의 기본 프로토타입 대신 새 프로토타입이 할당되었다. 새 프로토타입은 SuperType의 인스턴스이므로 SuperType인스턴스가 가질 프로티이와 매서드 외에 SuperType의 프로토타입을 가리키는 포인터도 가진다. 즉  instance는 SubType.prototype을 가리키고 SubType.prototype은 SuperType.prototype을 가리킨다. getSuperValue()매서드는 SuperType.prototype객체에 남지만 property는 SubType.property에 존재함을 기억하라. 이렇게 되는 것은 getSuperValue()이 프로토타입 매서드이며 property는 인스턴스의 프로퍼티이기 때문이다. SubType.prototype은 SuperType이므로 property는 SubType.property에 저장된다. SubType.property의 constructor프로퍼티를 엎어썻으므로 instance.constructor가 SuperType을 가리키는 점도 확인하라.  

기본 프로토 타입  
현실에서는 프로토타입 체인에 한 단계가 더 존재한다 모든 참조 타입은 기본적으로 프로토타입 체인을 통해 Object를 상속한다. 함수의 기본 프로포로토 타입은 Object의 인스턴스이므로 함수의 내부 프로토타입 포인터는 Object.prototype을 가리킨다. 커스텀 타입은 이런 방식으로 toString()이나 valueOf()같은 기본매소드를 상속한다. 즉 이전 예쩨는 상속의 일부분만을 나타낸 것이며 프로토타입 체인의 완전한 형태는 아래 그림과 같다. SubType은 SuperType을 상속하며 SuperType은 Object를 상속한다. instance.toString()을 호출하면 Object.prototype에서 해당 메서드를 찾아 호출한다.  
![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/proto65.png)

프로토타입과 인스턴스 사이의 관계  
프로토타입과 인스턴스의 관계는 두 가지 방법으로 알아볼 수 있다. 첫번째 방법은 instanceof연산자이다. instanceof연산자는 다음과 같이 인스턴스 생성자가 프로포타입 체인에 존재 할 때 true를 반환한다.  
<pre>
alert(instance instanceof Object); //true
alert(instance instanceof SuperType); //true
alert(instance instanceof SubType); //true
</pre>
instance객체는 프로토타입 체인 관계에 의해 Object와 SuperType, SubType모두의 인스턴스이다. 따라서 instanceof는 세 가지 생성자에서 모두 true를 반환한다. 두 번째 방법은 isPrototypeOf()매서드이다. 체인에 존재하는 각 프로토타입은 모두 isPrototypeOf()매서드를 호출 할 수 있는데 이 매서드는 다음과 같이 체인에 존재하는 인스턴스에서 true를 반환한다.  
<pre>
alert(Object.prototype.isPrototypeOf(instance)); //true
alert(SuperType.prototype.isPrototypeOf(instance)); //true
alert(SubType.prototype.isPrototypeOf(instance)); //true
</pre>

생성자 훔치기  
프로토타입과 참조 값에 얽힌 상속 문제를 해결하고자 개발자들은 생성자 훔치기라는 테크닉을 쓰기 시작했다 기본아이디어는 하위 타입 생성자 안에 상위타입의 생성자를 호출하는것이다. 함수는 단순히 코드를 특정 컨텍스트에서 실행하는 객체 일 뿐이며 다음과 같이 새로 생성한 객체에서 apply()와 call()매서드를 통해 생성자를 실행 할 수있다.(단점은 재사용이 불가능한 부분이 있다. 이것을 해결하기 위해 조합상속을 사용한다. - 이후설명)  
<pre>
function SuperType () {
  this.colors = ["red", "blue", "green"];
}
function SubType () {
  //SuperType에서 상속
 SuperType.call(this);
}
var instance1 = new SubType();
instance1.colors.push("black");

alert(instance1.colors); // "red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green"
</pre>
이 예제에서 강조한 부분이 생성자 훔치기에사용한 호출 코드이다. call()매서드 (apply()써도 무방하다.)를 사용해서 SuperType생성자를 새로 생성한 SubType의 인스턴스를 컨텍스트에서 호출하였다. 이 코드는 SuperType()함수에 들어 있는 객체 초기화 코드 전체를 SubType객체에서 실행하는 효과가 있다. 결과적으로 모든 인스턴스가 자신만의 colors프로퍼티를 갖게 된다.  

매개변수 전달  
생성자 훔치기 패턴은 하위 타입의 생성자 안에서 상위 타입의 생성자에 매개변수를 전달 할 수 있는데, 이는 생성자 훔치기 패턴이 프로토타입 체인보다 나은 점 중에 하나이다.  
<pre>
function SuperType(name) {
  this.name = name;
}
function SubType() {
  //SuperType에서 상속하되 매개변수를 전달
  SuperType.call(this,"Nicholas");

  //인스턴스 프로퍼티
  this.age = 20;
}
var instance = new SubType();
alert(instance.name);//"Nicholas"
alert(instance.age); //29
</pre>
이 코드에서 SuperType생성자는 매개변수로 name 하나를 받고 단순히 프로피티에서 할당하기만 한다. SubType생성자 내에서 SubType생성자를 호출 할 때 값을 전달할 수 있는데 이는 SubType인스턴스의 name프로퍼티를 지정하는 결과가 된다. SuperType생성자가 이들 프로퍼티를 덮어쓰지 않도록, 상위 타입 생성자를 호출한 뒤에 하위 타입에 프로퍼티를 더 정의할 수 있다.  

조합 상속  
조합상속은 프로토타입 체인과 생성자 훔치기 패턴을 조합해 두 패턴의 장점만을 취하려는 접근법이다. 기본아이디어는 프로토타입 체인을 써서 프로토타입에 존재하는 프로퍼티와 매서드를 상속하고 생성자 훔치기 패턴으로 인스턴스 포퍼티를 상속하는것이다. 이렇게 하면 프로토타입에 매서드를저으이해서 함수를 재사용 할 수 있고 각 인스턴스가 고유한 프로퍼티를 가질 수있다.   
<pre>
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function () {
  alert(this.name);
};
function SubType(name, age) {
  // 프로퍼티 상속
  SuperType.call(this,name)
    this.age = age;
  }
  //매서드 상속
  SubType.prototype = new SuperType();
  subtype.prototype.sayage = function () {
    alert(this.age);
  };
  var instance1 = new SubType("Nicholas", 29);
  instance1.colors.push("black");
  alert(instance1.colors); //"red,blue,green,black"
  instance1.sayName(); //"Nicholas"
  instance1.sayage(); //29

  var instance2 = new SubType("Greg", 27);
  alert(instance2.colors); //"red,blue,green"
  instance2.sayName();//"Greg"
  instance2.sayage();//27

</pre>
이 예제에서 SuperType생성자는 name과 두 가지 프로퍼티를 정의했고 SuperType프로토타입은 sayName()이라는 매서드를 가지고 있다. SubType생성자에서 name매개변수를 넘기며 SuperType생성자 호출을 실행하고 age라는 고유 프로퍼티를 정의한다. 또한 SubType프로타티입에 SuperType의 인스턴스가 할당되며 sayage()라는 새 매서드를 정의했다. 이 패턴에서는 SubType의 인스턴스 들이 colors프로퍼티를 포함해 자신만의 고유 프로퍼티를 가지면서도 매서드는 공유하게 만들 수 있다. 조합상속은 instanceof() 와 isPrototypeOf()에서도 올바른 결과를 반환한다.  

프로토타입 상속  
프로토타입은 새 객체를 생성할 때 반드시 커스텀 타입을 정의할 필요는 없다는 데에서 출발한다.   
<pre>
function object(o) {
  function F() {}
    F.prototype = o
    return new F();
}
</pre>
object함수는 임시 생성자를 만들어 주어진 객체를 생성자의 프로토타입으로 할당한 다음 임시 생성자의 인스턴스를 반환한다. 요약하면 object()는 주어진 객체의 사본 역할을 한다.  
<pre>
var person = {
  name : "Nicholas",
  friends : ["Shelby", "Court", "Van"]};
var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
</pre>
크록포드는 이런 방법을 통해 프로포토타입 상속을 구현하였다. 이 코드는 person의 복제본을 두 개 만드는 효과가 있다.  
ECMAscript 5판은 프로토타입 상속의 개념을 공식적으로 수용하여 Object.create()매서드를 추가하였다 이 매서드는 매개변수를 두 개 받는데 하나는 다른 객체의 프로토타입이 될 객체이며 다른 하나는 (옵션) 새 개개체에 추가할 프로퍼티를 담은 객치이다. 매개변수를 하나만 쓰면 Object.create()는 Object( )매서드와 똑같이 동작한다.  
<pre>
var person = {
  name : "Nicholas",
  friends : ["Shelby", "Court", "Van"]
};
var anotherPerson = Object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.pysh("Barbie");

aler(person.friends)// "Shelby,Court,Van,Rob,Barbie"
</pre>
Object.create()의 두번째 매개변수는 Object.defineProperties()의 두번째 변수와 같은 형식이다. 즉 추가할 프로퍼티마다 서술자와 함께 정의하는 형태이다 이런식으로 추가한 프로퍼티는 모두 프로토타입 객체에 있는 같은이름의 프로퍼티를 가진다.  
<pre>
var person = {
  name : "Nicholas",
  friends:["Shelby", "Court", "Van"]
};
var anotherPerson = Object.create(person, {
  name : {
    value : "Greg"
  }
  });
  alert(anotherPerson.name); //"Greg"
</pre>
인터넷익스플로러 9이상 브라우저에서 Object.create()매서드를 지원한다. 

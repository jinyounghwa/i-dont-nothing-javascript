서브클래스  

서브클래스 정의  
자바스크립트 객체는 클래스와 프로포타입 객체로부터 프로퍼티 (매서드)를 상속한다. 객체 상속을 알아볼려면 inherit()을 사용하여 다음과 같이 확인 할 수 있다.  
<pre>
B.prototype = inherit(A.prototype); //서브클래스는 슈퍼클래스를 상속한다.
B.prototype.constructor = B; // 상속된 constructor프로퍼티를 재정의한다.
</pre>
프로토타입 상속이 없다면 프로토타입 객체는 Object.prototype을 상속받은 평범한 객체일 뿐이고 이는 여러분이 만든 클래스는 다른 클래스처럼 Object의 서브클래스가 된다는 것을 의미한다.  
<pre>
//간단한 서브 클래스를 생성하는 함수
function defineSubClass(superclass, //슈퍼클래스 생성자
                        constructor, //새 클래스 생성자
                        method, //인스턴스 매서드: prototype으로 복사됨
                        statics) // 클래스 프로퍼티 : 생성자로 복사됨
{
  //서브 클래스의 프로토타입 객체를 설정한다.
  constructor.prototype = inherit(superclass.prototype);
  constructor.prototype.constructor = constructor;
  //일반적인 클래스로 다를 수 있도록 매서드와 정적 값들을 복사한다.
  if (methods) extend(constructor.prototype, methods);
  if (statics) extend(constructor, statics);
  //클래스를 반환한다.
  return constructor;
}
//또한 슈퍼클래스 생성자의 매서드로 서브 클래스를 생성 할 수 있다.
Function.prototype.extend = function(constructor, methods, statics){
  return defineSubClass(this, constructor, methods, statics);
};
//서브 함수 구조를 따지자면 아래와 같을 것 같다.(최상의 superclass는 Function()를 상속받게 한다고 가정한다.)
function SubClass(obj){
  // 1. 자식 클래스(함수객체) 생성
  // 2. 생성자 호출
  // 3. 프로토타입체인을 활용한 상속구현
  // 4. Obj를 통해 들어온 변수 및 매서드를 자식 클래스에 추가
  // 5. 자식 함수 객체 반환
}
</pre>
직접 서브 클래스를 사용 한 예제는 다음과 같다.  
<pre>
//생성자 함수
function SingletonSet(number) {
  this.menber = menber; // 이 세트의 단일 멤버를 저장한다.
}
//set의 프로토타입을 상속한 프로토타입 객체를 생성한다.
SingletonSet.prototype = inherit(Set.prototype);
//프로토타입에 프로퍼티를 추가한다.
//이 프로퍼티들은 Set.prototype에 있는 같은 이름을 가진 프로퍼티를 재정의한다.
extend(SingletonSet.prototype,{
  //constructor프로퍼티를 SingletonSet에 적합하게 정의한다.
  constructor:SingletonSet,
  //이 세트는 읽기 전용이다.add(), remove()는 에러 발생
  add : function(){throw "read-only"};
  remove : function(){throw "read-only"};
  //SingletonSet의 크기는 언제나 1
  size : function(){return : 1};
  //멤버가 하나만 있기 때문에 함수를 한번만 호출하면 된다.
  foreach : function (f, context){f.call(context, this.member);},
  //contains()매서드는 한가지 값에 대해서만 참이다.
  contains : function(x){return x === this.member};
  });
</pre>

생성자 함수와 매서드 체이닝  
매서드 체이닝이란 서브클래스를 정의 할 때 종종 매서드를 완전히 교체하지 않고 오직 슈퍼클래스의 매서드의 행위를 확장하거나 수정만 하고 싶을때 서브클래스의 생성자와 매서드가 슈퍼클래스의 생성자와 매서드를 호출하도록 하는것을 말한다.  

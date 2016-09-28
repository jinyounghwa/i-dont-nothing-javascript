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
  this.member = member; // 이 세트의 단일 멤버를 저장한다.
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
아래의 예제는 add()매서드를 완전히 재구현 하고 싶지않고 슈퍼클래스의 add()매서드만 체이닝 한 예제이다.  
<pre>
// NonNullSet은 null과 undefined를 멤버로 허용하지 않는 Set의 서브 클래스이다.

function NonNullSet() {
  //NonNullSet을 위한 별도의 동작 없이, 단지 슈퍼클래스 생성자를 체이닝만 한다.
  //이 생성자 호출에 의해 생성된 객체를 초기화 하기 위해
  //슈퍼 클래스의 생성자를 일반 함수처럼 호출한다.
  Set.apply(this, arguments);
}
//Set의 서브클래스인 NonNullSet을 만든다.
NonNullSet.prototype = inherit(Set.prototype);
NonNullSet.prototype.constructor = NonNullSet;

//null과 undefined를 제외하려면, add()매서드만 재정의하면 된다.
NonNullSet.prototype.add = function(){
  //인자가 null또는 undefined 인지 여부를 검사한다.
  for(var i = 0; i < arguments.length; l++)
  if(arguments[i] == null){
    throw new Error(" null또는 undefined는 불가"
  }
  //실제 멤버 삽입은 슈퍼 클래스의 매서드를 체이닝하여 수행
  return Set.prototype.add.apply(this, arguments);
}
</pre>

조합 대 서브클래스
객체지향 설계에서 지금까지 설명한 특정 필터 함수를 사용하여 집합 원소의 자격을 제한하는 서브 클래스를 만드는 방법 대신 '상속보다는 조합'을 선호하는 것이다. 조합을 사용하면 어떤 집합객체를 감싸는 새로운 집합 객체를 만들어 조건에 맞지 않는 객체에 해단 삽입 요청을 거른뒤에 안쪽 집합 객체에 전달함으로써 목적을 달성 할 수 있다.  
<pre>
var FilterSet = Set.extend(
  function FilterSet(Set, filter){//생성자
    this.set = set;
    this.filter = filter;
  },
  {//인스턴스 매서드
    add : function () {
      //필터가 설정되었다면 적용한다.
      if(this.filter) {
        for(var i =0;i < arguments.length; i++){
          var v = arguments[i];
          if(!this.filter(v))
          throw new Error("FilterSet : value" + v + "filter의해 거부")
        }
      }
      // add()매서드에 대한 요청을 this.set.add()로 전달한다.
      this.set.add.apply(this.set, arguments);
      return this;
    },
    // 나머지 매서드들은 this.add()로 요청을 전달하기만 하고 다른행동을 하지 않는다.
    remove : function(){
      this.set.remove.apply(this.set, arguments);
      return this;
    },
    contains : function(v) {return this.set.contains(v);},
    size: function(){return this.set.size();},
    foreach: function(f, c){this.set.foreach(f, c)}
  });
</pre>
아래의 예제는 추상 집합 클래스의 계층구조를 정의한다. AbstractSet를 상속한 AbstractEnumerableSet에는 추상 매서드 size()와 foreach()가 추가되어 있고 그 매서드들을 이용하는 구체 메서드들(toString(), toArray(), equals())도 정의되어 있다. 아래의 예제는 한번 쳐볼 가치가 있을 것 같다.  
<pre>
//모든 추상 매서드에서 편의상 사용하는 매서드,
function abstractmethod(){throw new Error("추상 매서드를 호출하였다.")};
//AbstractSet클래스는 추상 매서드 contains()하나만을 정의한다.
function AbstractSet(){throw new Error"추상 클래스는 인스턴스화 할 수 없습니다.};
AbstractSet.prototype.contains = abstractmethod;
//NotSet은 AbstractSet을 구현한 클래스이다.
//이 집합에 속한 멤버는 다른 집합의 멤버가 될 수 없다.
//NotSet은 다른 집합으로부터 정의되기 때문에 멤버를 추가할 수 없고,무한한 멤버를 가지고 있기 때문에 열거 될 수도 없다.
//NotSet에 대해서 할 수 있는것은 오직 특정 값에 대한 멤버 자격여부 검사뿐이다.
//이 서브클래스를 정의하려 앞서 정의한 Function.prototype.extend()매서드를 사용한다.
var NotSet = AbstractSet.extend(
  function NotSet(set){this.set = set;},
  {
    contains : function(x){return !this.set.contains(x)};
    toString : function(x){return "~" + this.set.toString();},
    equals : function(that){
      return that instanceof NotSet && this.set.equals(that.set);
    }
  }
  );
  //AbstractEnumerableSet은 AbstractSet의 추상 서브클래스다.
  //이 클래스는 추상 매서드 size(), foreach()를 정의하고
  //구체 매서드인 isEmpty(), toArray(),to[Locale]String(), equals()를 정의한다.
  //이 클래스의 서브클래스에서  contains(), size(), 그리고 foreach()를 구현할 때,
  //이 다섯가지 구체 매서드를 사용 할 수 있다.
  var AbstractEnumerableSet = AbstractSet.extend(
    function(){throw new Error("추상 클래스는 인서튼스 할 수 없습니다.");},
    {
      size : abstractmethod,
      foreach : abstractmethod,
      isEmpty : function(){return this.size() == 0;},
      toString : function() {
        var s = "{", i =0;
        this.foreach(function(v){
          if (i++ > 0) s += ".";
          s += v;
          });
          return s + "}";
      },
      toLocalString : function(){
        var s = "{", i =0;
        this.foreach(function(v){
                    if(i++ > 0) s += ".";
                    if(v == null) s += v; // null & undefined인 경우
                    else s += v.toLocalString(); //이 외의 경우
          });
          return s + "}";
        },
        toArray : function(){
          var a = [];
          this.foreach(function(v){a.push(v);});
          return a;
        },
        equals : function(that){
          if(!(that instanceof AbstractEnumerableSet)) return false;
          //만약 크기가 같지 않으면 두 집합은 다르다.
          if(this.size() != that.size())return false;
          //this의 모든 요소가 that에도 있는지 검사한다.
          try{
            this.foreach(function(v){if(!that.contains(v))throw false;});
            return true; //모든 요소가 같음. 두 집합은 같음
          }catch(x){
            if(x === false) return false; //두 집합은 같지 않다.
            throw x; //다른 예외가 발생하면 외부로 예외를 전달한다.
          }
        }
      }
    });
    //SingletonSet은 AbstractEnumerableSet의 구체 서브 클래스이다.
    //SingletonSet인스턴스는 하나의 멤버만 가진 읽기 전용 세트다.

  var SingletonSet = AbstractEnumerableSet.extend(
    function SingletonSet(member){this.member = member},
    {
      contains : function(x){return x === this.member},
      size : function(){return 1;},
      foreach : function(f, ctx){f.call(ctx,this.member);}
    }
    );
    //AbstractWritableSet은 AbstractEnumerableSet의 추상 서브 클래스이다.
    //이 클래스는 추상 매서드 add()와 remove()를 정의하고 이 추상 매서드들을 사용하는 구체 매서드 union(), intersection(), difference()를 구현한다.
  var AbstractWritableSet = AbstractEnumerableSet.extend(
    function(){throw new Error("추상 클래스는 인서튼스화할 수 없습니다.")},
    {
      add : abstractmethod,
      remove : abstractmethod,
      union : function(that){
        var self = this;
        that.foreach(function(v){self.add(v);});
        return this;
      },
      intersection function(that){
        var self = this;
        this.foreach(function(v){if (!that.contains(v)) self.remove(v);});
        return this;
      },
      difference : function(that){
        var self = this;
        that.foreach(function(v){self.remove(v);});
        return this;
      }
    });
    // ArraySet은 AbstractWritableSet의 구체 서브클래스다.
    // 이 클래스는 집합의 원소를 배열로 나타내고, contains()매서드는 배열에 대해 선형 검색을 한다.
    // contains()aotjemsms 0(1)이 아니라 0(n)이기 때문에, 상대적으로 작은 집합에서만 사용되어야 한다.
    //ArraySet의 구현은 ECMAScript 5의 Arry매서드인 instanceof()와 foreach()에 의존하고 있다는 점을 유의하라
    var ArraySet = AbstractWritableSet.extend(
      function ArraySet() {
        this.values = [];
        this.add.apply(this, arguments);
      },
      {
        contains : function(v){return this.values.indexOf(v) != -1;},
        size : function(){return this.values.length;},
        foreach : function(f, c){this.values.forEach(f, c);},
        add : function(){
          for (var i =0; i < arguments.length; i++){
            var arg = arguments[i];
            if(!this.contains(arg)) this.values.push(arg);
          }
          return this;
        },
        remove : function() {
          for (var i =0; i< arguments.length; i++){
            var p = this.values.indexOf(arguments[i]);
            if (p == -1) continue;
            this.values.splice(p,1);
          }
          return this;
        }
      }
      )
</pre>
위 예제는 Backbone에서 새로운 모델을 정의하는것과 유사한 부분이 있는데 아래의 예제를 확인하면 더더욱 비슷하다 생각될 것이다.  
<pre>
var User = Backbone.Model.extend({
  default : {
    username : '',
    firstName : '',
    lastName : ''
  },
  idAttribute : 'username',
  fallName : function() {
    return this.get('firstName') + this.get('lastName');
  }
  });
</pre>
Backbone은 "컬렉션"클래스라고 불리는것을 제공한다 이것은 개발자가 쉽게 모델 인스턴스의 공통집합을 다루는데 도움이 된다. 개발자는 그것을 강력한 배열로 여길 수 있으며 도움이 되는 유틀리티 함수와 함께 로드된다.  
<pre>
var UserCollection = Backbone.Collection.extend({
  model : User,
  url : '/users'
  });
  var users = new UserCollection();
  user.fetch(); //http 를 통해 사용자 레코드를 가지고 온다.
  var johndoe = users.get('john_doe'); // find by primary idAttribute
</pre>
제이쿼리에서의 객체 확장 - extend()매서드  
제이쿼리에서는 다른 객체의 프로퍼티나 매서드 복사 등으로 객체의 기능을 추가하는데 사용하는 extend()매서드를 제공한다.  
<pre>
Jquery.extend = Jquery.fn.extend = function(obj, prop){
  if (!prop){prop == obj; obj = this}

  // 함수를 호출 할 때 obj인자 하나만을 넘겨서 호출하는 경우 prop인자가 undefined값을 가지므로 !prop인자가 참이 되면서 if문 이하가 호출된다.
  //if문 내부 코드를 살펴보면 obj인자로 전달된 객체를 prop매개 변수에 저장한다 그리고 obj매개변수에는 this를 저장한다 여기서 함수 호출 패턴에 따라
  //함수 호출 extend()매서드 어디서 호출 호출되는지 따라서 다르게 바인딩된다.
  //Jquery 함수 객체에서 extend()매서드가 호출 될 경우 this는 Jquery 함수 객체로 바인딩된다. 반면에 jquery.prototype객체에서 extend()가 호출될 경우
  //jquery.prototype객체로 바인딩된다.

  for (var i in prop) obj[i] = prop[i]
  // for-in 문으로 prop인자의 모든 프로퍼티를 obj인자로 복사한다. 결국 obj객체에 prop객체와 프로퍼티가 추가된다.
  return obj;
}
</pre>
정리하면 extend() 매서드는 함수명 그대로 객체의 기능을 추가하는것이다.
<pre>
class Person {

  construcor(firstname, lastname){
    this.firstname = firstname;
    this.lastname = lastname;
  }
  greet(){
    return 'hi' + firstname;
  }
}
var john = new Person

// extend = Set the Prototype (__Proto__)
class InformalPerson extend Person {
  construcor(firstname, lastname){
    super(firstname, lastname)
  }
  greet(){
    return 'yo' + firstname; // override or hide with Object.create
  }
}

</pre>

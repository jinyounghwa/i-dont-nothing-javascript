##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>
모듈 패턴  
이전 패턴에서는 커스텀 타입에 사용할 고유 변수와 특권 매서드를 만들었다. 더글라스 크록포드가 고안한 모듈 패턴은 싱글톤에서 같은 일을 한다. 싱글톤이란 인스턴스를 단 하나만 가지도록 의도한 객체이다. 전통적으로 자바스크립트에서 싱글톤을 만들 때는 다음 예제와 같이 객체리터럴 표기법을 사용한다.  
<pre>
var singleton = {
  name : value,
  method :function(){
    //매서드 코드
  }
};
</pre>
모듈 패턴은 다음 형식에 따라 기본 싱글톤을 확장해서 고유 변수와 특권 매서드를 사용할 수 있다.   
<pre>
var singleton = function () {
  //고유변수와 함수
  var privateVariable = 10;
  function privateFunction() {
    return false;
  }
  // 특권 및 공용매서드와 프로퍼티
  return {
    publicProperty:true,

    publicMethod : function() {
      privateVariable++
      return privateFunction();
    }
  };
}();
</pre>
모듈 패턴은 객체를 반환하는 익명함수를 사용한다. 익명함수 내부에서는 첫번째로 고유 변수와 함수를 정의한다. 그 다음에는 객체리터럴을 함수 값으로 반환한다. 반환된 객체리터럴에는 공용이 될 프로퍼티와 매서드만 들어 있다. 객체는 익명함수 내에서 정의되었으므로 공용매서드는 모두 고유 변수와 함수에 접근 가능하다. 요약하면 객체 리터럴이 싱글톤에 대한 공용 인터페이스를 정의하는 것이다. 이 패턴은 다음과 같이 싱글톤에 일종의 초기화가 필요하고 고유 변수에 접근해야 할 때 유용하다.  
<pre>
var application = function() {
  //고유 변수와 함수
  var components = new Array();

  //초기화
  components.push(new BaseComponent());

  //공용인터페이스
  return{
    getComponentCount : function(){
      return components.length;
    },
    registerComponent : function(component){
      if(typeof component == "object"){
        components.push(component);
      }
    }
  };
}();
</pre>
웹 애플리케이션에서는 애플리케이션 레벨의 정보를 관리할 싱글톤을 두는 경우가 많다. 이 단순한 예제에서는 구성 요소를 관리할 application객체를 만든다. 객체를 처음 생성하면 고유 변수인 components 배열이 생성되고 BaseComponent의 새 인스턴스가 배열에 추가된다. getComponentCount()와 registerComponent()매서드는 components배열에 접근가능한 특권매서드이다 전자는 단순히 등록된 구성요소를 반환하며 후자는 새 구성요소를 등록한다. 모듈 패턴은 이런 경우, 단 하나의 객체를 반드시 생성하고 몇 가지 데이터를 가지며 고유 데이터에접근 가능한 공용 메서드를 외부에 노출하도록 초기화 해야 할 때 유용하다. 이런 식으로 생성한 싱글톤은 객체 리터럴을 이용하였으므로 모두 Object의 인스턴스이다. 싱글톤이 Object의 인스턴시인 점은 대수로운 문제는 아닌데 싱글톤은 일반적으로 함수에 매개변수를 전달하기 보다는 전역에서 접근하므로 instanceof 연산자로 객체타입을 판단할 필요가 없기 때문이다.  

모듈 확장 패턴  
모듈 패턴에서 한 가지 더 살펴볼 것은 객체를 반환하기 전에 확장하는 패턴이다. 이 패턴은 싱글톤 객체가 특정 타입의 인스턴스지만 프로퍼티나 매서드를 추가하여 확장 할 때 유용하다.  
<pre>
var singleton = function(){
  // 고유 변수와 함수
  var privateVariable = 10;

  function privateFunction(){
    return false;
  }
  //객체 생성
  var object = new CustomType();

  //특권 공용프로퍼티와 매서드 추가
  object.publicProperty = true;
  object.publicMethod = function(){
    privateVariable++;
    return privateFunction();
  };
  return object;
}();
</pre>
모듈 패턴 예제의 application객체거 BaseComponent의 인스턴스여야 한다면 다음 코드를 쓰면 된다.  
<pre>
var application = function(){
  // 고유 변수와 함수
  var components = new Array();
  //초기화
  components.push(new BaseComponent());
  //애플리케이션 인스턴스 생성
  var app = new BaseComponent();

  //공용인터페이스
  app.getComponentCount = function(){
    return components.length;
  };
  app.registerComponent = function(components){
    if(typeof components == 'object'){
      components.push(component);
    }
  };
  return app;
}();
</pre>
애플리케이션 싱글톤의 고쳐 쓴 버전의 첫 줄에는 이전 예제와 마찬가지로 고유 변수를 정의했다. 주요 차이는 BaseComponent의 인스턴스인 변수 app이다. 이 변수는 application 객체의 로컬버전이다. 그 다음 고유 변수에 접근할 공용 매서드를 app객체에 추가했다. 마지막단계에서 application에 할당할 app객체를 반환한다.  

5.1 함수 조깁의 핵심  
4장에서 소개했던 invoker함수는 첫번째 인자로 받은 객체에 매서드를 호출하는 함수다. 대상 객체게 호출하려는 매서드를 제공하지 않으면 undefined를 반환한다는 사실을 기억할 것이다. 이와 같은 특성을 이용해서 여러 invoker를 조립해서 다형적인 함수를 만들거나 인자에 따라 다른 동작을 수행하는 함수를 만들 수 있다. 그러려면 한 개 이상의 함수를 이용해서 undefined가 아닌 다른 값을 반환하는 함수를 찾을 때가지 매서드 호출 시도를 반복해야 한다. 바로다음에 소개할 dispatch는 지금까지 설명한 동작을 수행하는 함수다.  
```javascript

function dispatch(/*funs*/) {
  var funs = _.toArray(arguments);
  var size = funs.length;

  return function(target /*, args */) {
    var ret = undefined;
    var args = _.rest(arguments);

    for (var funIndex = 0; funIndex < size; funIndex ++) {
      var fun = funcs(funIndex);
      ret = fun.apply(fun, comstruct(target, args));

      if(existy(ret)) return ret;
    }
    return ret;
  }
}

```
간단한 작업인데도 꽤 긴 코드가 사용되었다.  
요약하면 객체를 포함하는 배열의 요소를 반복하면서 각 객체에 메서드를 호출해서 첫번째로 반환되는 실제값을 찾는 함수를 반환하는 것이 우리의 목표다. 복잡헤 보이기는 하지만 dispatch는 자바스크립트 함수의 다형성 정의를 만족하는 함수다. dispatch는 구체적인 매서드 실행을 다른 함수에 위임한다. 예를 들면 언더스코어의 많은 함수 구현에서 다음과 같은 패턴이 반복되는것을 발견할 수 있다.  
+ 대상이 존재하는지 확인한다.
+ 네이티브 버전이 있는지 확인하여 있다면 그것을 확인한다.
+ 네이티브 버전이 없다면 필요한 종작을 수행할 테스크를 구현한다.
- 가능하면 형식이 정해진 테스크를 만든다.
- 가능하면 인자가 명확한 테스크를 만든다.
- 가능하면 인자의 개수가 명확한 테스크를 만든다.  
언더스코의의 _ .map 함수는 지금까지 설명한 패턴을 명확하게 보여준다.  
```javascript
_.map = _.collect = function(obj, iterator, context) {
  var result = [];
  if (obj==null) return result;
  if(nativeMap && obj.map === nativeMap) return obj.map(iterator, context);
  each(obj, function(value, index, list){
    results[results.length] = iterator.call(context, value, index, list)
  });
  return results;
}

```
dispatch를 이용해서 위 코드를 좀 더 단순하게 쉽게 확장 할 수 있다. 배열과 문자열 형식을 문자열로 표현하는 함수를 만들어야 한다고 가정하자. dispatch를 활용하면 다음처럼 깔금하게 원하는 기능을 구현 할 수 있다.  
```javascript
var str = dispatch(invoker('toString', Array.prototype.toString)),
                  invoker('toString', String.prototype.toString);

    str('a'); //=>a
    str(_.range(10));
    //=> "0,1,2,3,4,5,6,7,8,9"
```
예제에서 확인 할 수 있는 것처럼 if-then-else 문을 이용하는 함수를 이용하는 대신 Array.prototype.toString이라는 상세 구현에 형식 및 존재확인 작업을 위임했다. 물론 dispatch의 동작은 invoker의 활용에 의존하지 않는다. 하지만 dispatch는 특정 규칙을 따른다. 즉 dispatch는 모든 함수를 실행하거나 아니면 존재하는 값을 반환할 때까지 모든 함수를 반복 실행한다. 이 때 stringReverse라는 함수를 이용해서 dispatch의 규칙에 관여 할 수 있다.  
```javascript
function stringReverse(s) {
  if(!_.isString(s)) return undefined;
  return s.split('').reverse().join("");
}
stringReverse("abc")//=>"cba"
stringReverse(1); //undefined
```
stringReverse에 Array#reverse매서드를 이용해서 rev라는 새로운 다형적 함수를 만들 수 있다.  
```javascript
var rev = dispatch(invoker('reverse', Array.prototype.reverse), stringReverse);

rev([1,2,3]); //=>[3,2,1]

rev("abc"); //=> "cda"
```
또한 dispatch의 규칙을 활용해서 항숭 존재하는 값을 반환하거나 예외를 발생시키는 함수를 추가함으로써 최종 함수를 구현 할 수 있다. 한 걸음 더 나아가 dispatch로 만든 함수를 dispatch의 인자로 제공함으로써 유연성을 극대화 할 수 있다.  
```javascript
var sillyReverse = dispatch(rev, always(42));

sillyReverse([1,2,3]);
//=> [3,2,1]

sillyReverse("abc"); //=> "cda"

sillyReverse(10000); //=>42
```
다음과 같이 수동적으로 명령을 분류하는 switch문을 dispatch로 대체 할 수 있다.  
```javascript

function performCommandHardcoded(command) {
  var result;
  switch (command.type)
  {
  case 'notify' :
  result = notify(command.message);
  break;
  case 'join' :
  result = changeView(command.target);
  break;
  default:
  alert(command.type);  
  }
  return result;
}
```
performCommandHardcoded 함수의 switch문은 command객체의 필드를 확인해서 명령어 문자열값에 따라 관련 코드를 실행한다.  
```javascript
performCommandHardcoded({type: 'notify', message:'hi'});
// notify실행

performCommandHardcoded({type:'join', target:'wait-room'});
// changeView실행

performCommandHardcoded({type:'wait'});
//알림창 팝업
```
다음 예제처럼 dispatch를 이용해서 switch를 사용하던 기존의 패턴을 깔끔하게 제거 할 수 있다.  
```javascript
function isa(type, action) {
  return function(obj) {
    if (type === obj.type)
      return action(obj);
  }
}
var performCommand = dispatch(
  isa('notify', function(obj){return notify(obj.message)}),
  isa('join', function(obj){return changeView(obj, target)}),
  function(obj){alert(obj.type)};
)
```
isa 함수는 type문자열과 action함수를 인자로 받아 새로운 함수로 반환한다. 반환된 함수는 type문자열과 obj.type필드가 일치하면 action함수를 호출하고 일치하지 않으면 undefined를 반환한다. undefined가 반환되면 dispatch가 반환되면 dispatch는 다음 저수준 함수가 명령을 할 수 있는지 시도한다.  
performCommandHardcoded함수의 기능을 확장하려면 switch문 자체를 고쳐야 한다. 그러나 또 하나의 dispatch함수로 performCommandHardcoded를 래핑하면 새 기능을 추가 할 수 있다.  
```javascript
var performAdminCommand = dispatch(
  isa('kill', function(obj){return shutdown(obj.hostname)})
  performCommand);
```
새로 만든 함수 performAdminCommand는 kill명령어 실행을 시도한다. 명령어를 샐행할 수 없으면 performCommand가 kill명령어 실행을 시도한다.  
```javascript
performAdminCommand({type:'kill', hostname:'localhost'}); //셧다운 실행

performAdminCommand({type:'fail'}); //알림창 팝업

performAdminCommand({type:'join', target:'foo'}); //changeView실행
```
두 개 이상의 dispatch가 연결된 dispatch체인에서 특정 명령을 오버라이드 해서 그 명령이 수행되지 않도록 할 수도 있다.  
```javascript
var performTrialUserCommand = dispatch(
  isa('join', function(obj){alert("cannot join until approved")}),
  performCommand;
)
```
다음은 performTrialUserCommand를 실행한 결과다.  
```javascript
performTrialUserCommand({type:'join', target:'foo'});
//실행 할 수 없음을 알리는 창이 팝업 됨
performTrialUserCommand({type:'notify', message:'Hi new user'});
//notify실행
```
동작이 명확하게 알려진 기존 부품을 이용해서 새로운 기능을 만들고 새로 만들어진 기능 역시 자체적으로 어떤 기능을 수행함으로써 다른 기능의 부품이 될 수 있다 이것이 함수 조립의 핵심이다. 이 장의 나머지 부분에서는 커링의 개념을 시작으로 해서 함수를 조립해서 새로운 기능을 만드는 다양한 방법을 소개할 것이다.  

변이는 저수준 동작이다.  
함수는 추상화된 기본 단위다 함수내에서 가장 중요한 점은 함수가 어떤 규칙을 준수하며 요구사항을 실행한다는 것이다 함수 내부에서 변수 변이가 발생했고 외부로 영향을 주지 않는다면 아무도 개의치 않는다. 때로는 변이가 필요하다. 하지만 변이는 상항 멀리해야 할 저수준동작으로 간주한다.  

커링
이미 invoker라는 커리 함수를 예제로 살펴봤다. 각각의 논리적 인자에 대응하는 새로운 함수를 반환하는 함수를 커리(혹은 커리된)함수라고 한다. 다음 예제처름 invoker를 약간 고칠 수 있다.  
```javascript
function rightAwayInvoker() {
  var args = _.toArray(arguments);
  var method = args.shift();
  var target = args.shift();

  return method.apply(target, args);
}
rightAwayInvoker(Array.prototype.reverse, [1,2,3]); //[3,2,1]
```
rightAwayInvoker함수는 target객체를 이용할 함수를 반환하는 대신 두 번째 인자로 제공된 target에 method를 호출한다. 반면 invoker함수는 커리함수 즉 모든 인자가 제공되기 전까지 target에 method를 호출하지 않는다. 다음은 invoker실행 예제다.  
```javascript
invoker('reverse', Array.prototype.reverse)([1,2,3]); // [3,2,1]
```
위예제에서 괄호를 두개 연달아 사용하였는데 이 때문에 중요한 과정을 살펴볼 수 잇는 기회가 사라졌다. 즉 위 코드에서는 invoker가 반환함고 동시에 reverse동작을 수행할 함수가 배열 [1,2,3]을 호출한다.  
자신이 생성된 컨텍스트에 따라 어떤 동작을 실행하도록 설정된 함수나 클로저를 반환하는 것이 얼마나 유용한지 기억하지 이 원리를 커리함수에서 적용 할 수 있다 즉 커리함수는 각각의 논리 파라미터를 이용해서 점차 세부적으로 설정된 함수를 반환한다.  

우향커리, 좌향커리
커리가 진행되는 방향은 조절 할 수 있으므로 커리가 일어나는 방향은 그리 중요한 사항이 아니다. 하지만 커리가 일어나는 방향에 따라 API사용 방법이 달라질 수 있다. 이 책에서는 필자가 선호하는 방식인 좌향 커리(가장 오른쪽 인자에서 커리가 시작하여 왼쪽으로 이동하는)를 기본으로 사용한다. 자바스크립트 같이 여러 인자를 제공할 수 있는 언어에서는 좌향커리를 이용해서 선택사항 인자를 특정값으로 고정 시킬 수 있다. 다음은 커리 진행 방향에 따른 차이를 보여주는 사례다.  
```javascript
function leftCurryDiv(n) {
  return function(d) {
  };
}
function rightCurryDiv(d) {
  return function(n) {
    return n/d;
  }
}
```
나눗셈은 인자의 위치에 따라 결과가 달라지므로 커링의 방향에 따른 변화를 보여 주기에 적합 한 예다 leftCurryDiv 함수의 커리된 파라미터에 따라 어떤 결과가 나오는지 살펴보자.  
```javascript
var divide10By = leftCurryDiv(10);
```
leftCurryDiv에 인자 10을 제공해서 divide10By라는 함수를 만들었다. 결과적으로 divide10By함수는 10/? 라는 계싼을 수행하는 함수다. ?는 가장 오른쪽에 커리된 파라미터로 다음 호출에 의해 ?값이 결정될 것이다.  
```javascript
divide10By(2); //=>5
```
divide10By라는 두번째 커리된 함수를 호출할 때 비로소 10/2라는 수식이 완성되어 5라는 결과가 반환된다. 이제 rightCurryDiv 함수를 사용하면 무엇이 달라지는지 살펴보자.  
```javascript
var divideBy10 = rightCurryDiv(10);
```
divideBy10이라는 커리된 함수의 바디는 ?/10이라는 수식을 가진 상태가 되며 가장 왼쪽인자가 완성되기를 기다린다.  
```javascript
divideBy10(2); //=>0.2
```
책내에서는 좌향커리, 즉 오른쪽 인자로부터 커링을 시작할 것이므로 그림 5-2와 같은 순서로 커링이 일어난다.  
부분적용은 왼쪽부터 인자를 처리하므로 커링은 오른쪽부터 시작하는것이 좋다. 부분적용과 커링을 이용하면 인자를 양쪽 방향 모두로 처리할 수 있으므로 원하는대로 파라미터를 특화 할 수 있다 이제 leftCurryDiv, rightCurryDiv에서 살펴본 것처럼 수동으로 커링하는 함수와 자동으로 커링하는 함수를 구현 할 것이다.  

자동 커링 파리미터  
over10과 divideBy10은 모두 수동으로 커리가 발생한다. 즉 함수 파라미터 수에 대응하는 수의 함수를 반환하도록 함수를 구현했다. 마찬가지로 rightCurryDiv함수도 두 개의 논리적 인자를 갖는 나눗셈 함수를 반환했다 함수를 인자로 받은 다음에 한 개의 파라미터를 잔는 고정된 함수를 반환하는 고차원 함수를 만들 수 있다. 이런 함수를 curry라 부를 것이다. 다음은 고차원 함수 curry의 구현 코드다.  
```javascript
function curry(fun) {
  return function(arg){
    return fun(arg)
  }
}
```
curry의 기능을 다음과 같이 요약 할 수 있다.  
+ 함수를 인자로 받는다.
+ 한 개의 파라미터를 갖는 함수를 반환한다.  
따라서 curry함수는 아무 동작도 수행하지 않는 불필요한 함수라고 생각 할 수 있다. 함수를 직접 이용하면 되는데 굳이 curry라는 함수를 만들 필요가 있을까? 많은 함수형 언어 에서는 curry같은 단순 위임기능을 제공해야 할 이요가 명확하다. 하지만 자바스크립트에서는 다른 함수형 언어와는 상황이 조금 다르다 자바스크립트에서는 인자의 개수가 부가적인 특화 인자의 개수가 정해져 있지 않을 때가 많다. 예를들어 자바스크립트 함수 parseInt는 문자열을 인자로 받아 정수값을 반환한다.  
```javascript
parseInt('11');
//=>11
```
또한 parseInt의 두 번째 인자를 이용해서 파싱에 사용할 기수를 정의 할 수 있다.  
```javascript
parseInt('11', 2); //=>3
```
이전 코드에서 기수는 2, 즉 이진수로 결과를 파싱한다. parseInt를 일급으로 활용하려 하면 두번째 인다 때문에 복잡한 문제가 발생한다.  
```javascript
['11','11','11','11'].map(parseInt);
//=>['11',NaN, 3, 4]
```
Array#map 에 함수를 제공하면 배열의 각 요소에 제공된 함수를 호출한다. (이때 요소의 배열 인덱스와 배열 자체를 제공한다.) 따라서 위와 같은 결과가 어떻게 나왔는지 이미 파악하였을지도 모르지만 parseInt의 두번째 인자인 기수가 0,1,2,3순으로 제공되면서 11,NaN,3,4와 같은 결과가 나온다. 다행히 curry를 이용하면 한 개의 인자만을 받도록 강제 할 수 있다.  
```javascript
['11','11','11','11'].map(curry(parseInt));
//=>['11','11','11','11']
```
curry같은 함수를 이용함으로써 특화에 사용되는 우측인자를 고정(혹은무시)시켜서 직접 호출되는 함수를 제어 할 수 있다.  
예를들어 다음처럼 curry2 함수를 이용해서 두 함수 인자를 커링할 수 있다.  
```javascript
function curry2(fun) {
  return function(secondArg){
    return function(firstArg){
      return fun(firstArg, secondArg)
    }
  }
}
```
curry2는 함수를 인자로 받아 두 개의 인자와 섞는다. 다음은 curry2를 이용해서 divideBy10을 구현한 코드다.  
```javascript
function div(n,d) {return n/d}
var div10 = curry2(div)(10);
div10(50);
//=>5
```
rightCurryDiv와 마찬가지로 div10함수는 ?/10 을 완성하는데 필요한 첫 번째 인자를 기다린다. curry2를 이용해서 parseInt함수가 오직 이진수만 처리하도록 만들 수 있다.  
```javascript
var parseBinaryString = curry2(parseInt)(2);

parseBinaryString("111"); //=>7

parseBinaryString("10"); //=>2
```
커링은 자바스크립트 함수의 특수 동작을 지정하거나 기존 함수를 이용해서 새로운 함수를 조립할 때 아주 유용한 기능이다. 커링을 어떻게 유용하게 샤용할 수 있는지 계속 살펴보자.  
커링으로 새로운 함수 만들기  
지금까지 curry2를 이용하새 간단한 div10 함수를 만드는 예제를 살펴보았다. 이밖에도 다양한 방식으로 curry2를 활용 할 수 있다. 실제로 클로저를 이용해서 같은 기능을 수행 할 수 있다. 예를들어 언더스코어는 배열의 요소를 어떤 범주로 분류해서 각각에 범주에 해당하는 객체의 수를 반환하는 _ .countBy함수를 제공한다. 다음은 사용 예제다.  
```javascript
var plays = [{artist:"Burial", track:"Archangel"},
            {artist:"Ben Frost", track:"Stomp"},
            {artist:"Ben Frost", track:"Stomp"},
            {artist:"Burial", track:"Archangel"},
            {artist:"Emeralds", track:"Snores"},
            {artist:"Burial", track:"Archangel"}
];
_.countBy(playsm function(song){
  return [song.artist, song.track].join(" - ");
})
//Ben Frost-Stomp :2
//Burial - Archangel : 3
//Emeralds - Snores :1
```
_ .countBy가 함수를 두 번 째 인자로 받으므로 curry2를 _ .countBy의 두 번째 인자로 활용하면 원하는 기능을 구현 할 수 있다.  
다음은 아티스트를 카운트하는 songCount구현 코드다.  
```javascript
function songToString(song) {
  return [song.artist, song.track].join(" - ");
}
var songCount = curry2(_.countBy)(songToString);
songCount(plays);
//Ben Frost-Stomp :2
//Burial - Archangel : 3
//Emeralds - Snores :1
```
이전 코드에서 확인 할 수 있는 것처럼 커링을 이용하면 가상의 문장을 직관적으로 구현 할 수 있다. 이러한 플루언트 함수형 인터페이스를 만들 때 이와같은 커링기법이 사용되는 것을 자주 접하게 될 것이다.  

커링을 이용한 플루언트 API  
커링을 사용하면 의도하지 않아도 함수형 API가 된다는 특징이 있다 하스켈 프로그래밍 언어의 라이브러리는 함수가 기본적으로 커리된다는 점을 자연스럽게 활용한다. 그러나 자바스크립트의 함수형 API는 커링을 이용할 수 있도록 의도적으로 설계해야 하며 사용방법을 문서화 해야 한다. API가 고차원 함수를 활용한다면 적어도 하나 이상의 파라미터에 커리된 함수가 적합하다.  
```javascript
var greaterThan = curry2(function(lhs, rhs){return lhs > rhs});
var lessThan = curry2(function(lhs,rhs){return lhs<rhs});
```
어떤 수보다 큰지, 어떤 수보다 작은지를 계산하는 두 함수를 커링한 버전을 찬반형을 필요로 하는 validator에 직접 적용 할 수 있다.  
```javascript
var withinRange = checker(
  validator("arg must be greater than 10", greaterThan(10));
  validator("arg must be less than 20", lessThan(20))
);
```
어똔 수보다 큰지, 어떤 수보다 작은지를 계산하는 임의의 버전을 직접 사용하는 것보다는 커리된 함수를 이용하는것이 훨씬 가독성이 좋다. 다음은 withinRange를 실행한 결과다.  
```javascript
withinRange(15); //=>[]

withinRange(1); //=>["arg must greater than 10"]

withinRange(100); //=> ["arg must be less than 20"]
```
부분 적용  
커리된 함수는 제공된 모든 파라미터를 사용하면서 특정 기능을 수행하는 함수를 점진적으로 반환한다고 설명했다. 이와달리 부분적용 함수는 부분적으로 실행을 마친다음에 나머지 인자와 함께 즉시 실행한 상태가 되는 함수다. 실제 부분적용이 동작 하는 코드를 확인하는 것만큼 이해하하는데 큰 도움을 주는 방법은 없다. over10을 다음 코드처럼 기존과는 다른 방식으로 구현 할 수 있다.  
```javascript
function divPart(n){
  return function(d) {
    return n/d;
  }
}
var over10Part = divPart(10);
over10Part(2);
//=>5
```
over10Part구현이 leftCurryDiv와 상당히 비슷하다는 것을 알 수 있는데 이는 커링과 부분적용의 한계를 잘 보여준다. 실행되기까지 한 개의 인자만 남겨 둔 상태의 커리 함수는 사살상 한 개의 인자만 남겨둔 부분적용 함수와 같다. 그러나 부분적용에서는 한번에 한 개의 인자를 처리하지 않으며 한 번에 필요한 만큼의 인자를 적용한 다음에 이후 실행 시에 나머지 인자를 활용 할 수 있는 상태로 저장된다.  
커링과 부분적용은 서로 비슷한 면이 있지만 사용방법은 전혀 딴판이다. curry2 와 curry3 를 어떤방향으로 적용하느냐에 따라 API의 모양과 방식이 달라지긴 하지만 커리적용방향은 중요한것이 아니다. 부분적용의 가장 큰 특징은 varargs함수에 효율적으로 적용하기 쉽다는 것이다. varargs를 활용하는 자바스크립트 함수는 보통 처음 몇 개의 인자는 직접 바인딩 하고 마지막 인자는 선택사항이나 행동을 특화하는 용도로 사용한다. 즉, 대부분의 자바스크립트 API는 파라미터 집합을 미리 정해 놓는다. 따라서 자바스크립트 API의 행동과 기본 동작은 이미결정되어 있다. 부분적용은 자바스크립트의 이러한 특징을 잘 활용한다.  
한두개의 알려진 인자를 부분적용  
커링과 마찬가지로 가장 간단한 함수로 부분 적용을 설명할 수 있다. 다음은 첫 번째 인자를 부분적용하는 함수 예제다.  
```javascript
function partial(fun, arg1) {
  return function(/*args*/) {
    var args = constrct(arg1, arguments);
    return fun.apply(fun,args);
  }
}
```
partial에서 반혼하는 함수는 기존 호출로부터 arg1인자를 캡처한 다음에 실행 호출의 인자 목록 앞부분에 추가한다. 다음 partial1의 동작 모습이다.  
```javascript
var over10Part1 = partial(div, 10);

over10Part1(5);
//=>2
```
결과적으로 다른 함수와 '설정' 인자를 이용해서 over10함수 동작을 다른 방법으로 구현할 수 있다. 마찬가지로 두 개이 인자를 부분적용하는 함수는 다음처럼 구현 할 수 있다.  
```javascript
function partial2(fun, arg1, arg2){
  return function(/*args*/) {
    var args = cat([arg1, arg2], arguments);
    return fun.apply(fun,args);
  }

}
var divide10By2 = partial2(div, 10, 2);

divide10By2();
//=> 5
```
실생활에서는 한두개의 인자를 부분적용하는 상황을 접할 수 있다. 하지만 정해지지 않는임의의 수의 인자도 캡쳐할 수 있다면 좀 더 유용할 것이다. 이번엔 임의의 수의 인자를 캡쳐하는 방법을 알아보자.  

임의의 수의 인자를 부분 적용  
커링을 사용했을 때 자바스크립트의 varargs때문에 상황이 복잡해지는것과는 달리 임의의 수의 인자를 부분적용하는 방법을 이용하면 별다른 부작용이 없다. 다행히 임의 수의 인자를 부분적용하는 partial구현은 기존의 partial1이나 partial2에 비해 그리 복잡하지 않다 다음 코드를 보면 partial도 구현 방식이 비슷함을 확인 할 수 있다.  
```javascript
function partial(fun /*pargs*/) {
  var pargs = _.rest(arguments);

  return function(/*arguments*/) {
    var args = cat(pargs, _.toArray(arguments));
    return fun.apply(fun, args);
  }
}
```
partial은 몇몇 인자를 캡쳐해서 그들을 최종 호출 시 자신의 인자 앞에 붙여 사용하도록 함수를 반환한다. 다음 코드는 partial의 동작 모습을 보여 준다.
```javascript
var over10Part1(fun /*,pargs*/) {
  var pargs = _.rest(arguments);

  return function(/*arguments*/) {
    var args = cat(pargs, _.toArray(arguments));
    return fun.apply(fun,args)
  };
}
```
partial은 몇몇 인자를 캡쳐해서 그들의 최종 호출 시 자신의 인자 앞에 붙여 사용하도록 함수를 반환한다. 다음 코드는 partial의 동작 모습을 보여 준다.  
```javascript
var over10Part1 = partial(div, 10);
over10Part1(2); //=>5
```
부분 적용으로 자바스크립트에서 varargs때문에 발생하는 문제를 어느정도 해결할 수 있지만 다음 예제가 보여주는 것처럼 여전히 복잡한 문제가 발생할 수 있다.  
```javascript
var divide10By2By4By5000partial = partial(div, 10, 2, 4, 5000);
divide10By2By4By5000partial();
//=>5
```
부분적용하려는 인자의 수를 이미 알고 있는 상황일지라도 partial은 임의의 수의 인자를 받을 수 있으므로 때로는 혼란스러울 수 있다. 부분적용 함수 div에서는 인자 10,2를 이용해서 단 한번만 호출했으며 이 때 나머지 인자는 그냥 무시했다. 부분적용을 서툴게 이용하면 혼란만 가중시킬 뿐이다.  
부분적용 사례 : 선행조건  
다음은 4장에서 소개한 validator함수를 실행한 결과이다.  
```javascript
validator("arg must be a map", aMap)(42);
//=> false
```
고차원 함수 validator검증 찬반형을 인자로 받아서 에러가 발생한 항목 정보를 포함하는 배열을 만든다. 반환된 에러 배열이 비었다는 것은 전혀 에러가 발생하지 않았음을 가리킨다. validator는 함수 인자 수동 검증과 같은 다양한 용도로 활용 할 수 있다.  
```javascript
var zero = validator("connot be zero", function(n){return 0===n});
var number = validator("arg must be a number", _.isNumber);

function sqr(n) {
  if (!number(n)) throw new Error(number.message);
  if(zero(n)) throw new Error(zero.message);

  return n + n;
}
```
다음은 sqr함수를 실행한 결과다.  
```javascript
sqr(10); //=> 100

sqr(0); //=> Error: cannot be zero

sqr(''); //=> Error: arg must be a number
```
위 구현도 상당히 좋은 코드지만 부분적용을 이용하면 좀 더 코드를 개선 할 수 있다. 기본적인 데이터 검증은 수행할 수 있지만 위 코드로 검출 할 수 없는 종류의 에러도 있다. 바로 계산보장과 관련된 종류의 에러가 그 중 하나다. 다음과 같이 두 종류로 재산 보장을 구분할 수 있다.  
+ 선행조건 : 호출하는 함수에서 보장하는 조건
+ 후행조건 : 선행조건이 지켜졌다는 가장하에 함수 호출 결과를 보장하는 조건
선행 조건과 후행조건의 관계를 함수가 처리 할 수 있는 데이터를 제공했을 때 함수는 특정 기준을 만족하는 결과를 반환할 것이다 로 설명할 수 있다.  

위에서 소개한 sqr 함수는 인자가 숫자인가 0 인가와 관련된 두 개의 선행조건을 포함했다. 이들 조건을 매번 수행하는 것도 괜찮지만 선행조건의 충족 여부는 실행중인 애플리케이션의 컨텍스트에 상대적일 수밖에 없다. 따라서 condition이라는 새로운함수를 만들고 핵심적인 계산 로직과는 독립적으로 선행조건을 추가하도록 부분적용을 이용할 수 있다.  
```javascript
function condition1(/*validators*/) {
  var validators = _.toArray(arguments);

  return function(fun,arg) {
    var errors = mapcat(function(isVaild){
      return isVaild(arg) ? [] : [isVaild.message];
    }, validators);

    if (!_.isEmpty(errors))
      throw new Error(errors.join(","));

    return fun(arg);  
  };
}
```
condition1이 반환한 함수는 오직 하나의 인자만 갖는다는 사실을 확인 할 수 있다. condition1이 반환하는 함수는 validator로 생성된 한 개의 함수나 함수 집합을 인지로 받아서 Error을 반환하거나 또는 fun함수실행 결과값을 반환한다. 매우 간단하지만 상당히 유용한 패턴임을 알 수 있다.
```javascript
var sqrPre = condition1(
  validator("arg must not be a zero", _.complement(zero));
  validator("arg must be a number", _.isNumber);
)
```
condition1은 자바스크립트 기준에서 볼 때 상당한 풀르언트 검증 API다 함수 조립을 하다보면 코드가 (무엇을 수행하는지 설명하는)선언형이 됨을 자주 발견하게 될 것이다. sqrPre를 실행해 보면 condition1이 어떻게 동작하는지 확인 할 수 있다.
```javascript
sqrPre(_.identity, 10); //=>10

sqrPre(_.identity, ''); //=> Error " app must be a number"

sqrPre(_.identity, 0); //=> Error: arg must not be zero
```
sqr의 자체 에러 처리 정의를 기억하는 독자라면 sqrPre로 인자를 어떻게 확인 할 수 있는지 힌트를 얻었을 것이다. 무슨이야기인지 잘 이해가 안된다면 불안정한 sqr정의 버전을 살펴보자
```javascript
function uncheckedSqr(n){return n+n};

uncheckedSqr(''); //=>0
```
(자바스크립트의 약점 때문에 위와 같은 결과가 나왔지만) 분명 빈 문자열의 제곱이 0일 수는 없다. 다행이 지금까지 만든도구를 이용하면(validator, partial1, condition1, sqrPre등)를 이용하면 이 문제를 다음처럼 해결 할 수 있다.  
```javascript
var checkerSqr = partial1(sqrPre, uncheckedSqr);
```
함수 만들기, 함수를 만드는 함수, 각 도구들의 상호작용을 통해 새로운 함수 checkerSqr이 탄생했다.  
```javascript
checkerSqr(10); //=>100

checkerSqr(''); //=> Error arg must be a number

checkerSqr(0); //=> Error arg must not be zero
```
위 코드에서 확인 할 수 있는 것처럼 새로운 checkerSqr은 핵심계산 과정과 검증단계가 분리 되었다는것을 제외하면 sqr과 같다. checkerSqr을 통해서 이상적인 수준의 유연성을 달성한 것이다. 결과적으로 함수에 조건을 적용하지 않도록 조건 확인 기능을 오나전히 제거 할 수 있으며 심지어 나중에 새로운 검증을 혼합 할 수 도있다.  
```javascript
var sillySquare = partial1(
  condition1(validator("should be even", isEven)),
  checkerSqr
);
```
condition1의 결과 함수는 작업을 위임할 다른 함수를 필요로 하므로 partial1으로 두 함수를 깔끔하게 묶었다.  
```javascript
sillySquare(10); //=> 100

sillySquare(11); //=>Error: should be even

sillySquare(''); //=> Error: arg must be a number

sillySquare(0); //=> Error: arg must not be zero
```
짝수의 제곱만을 요구하는 상황이 실제로 일어날 가능성은 적을 듯하다. 하지만 위 코드에서 핵심은 명확하게 보여 준다. 다른 함수를 조립하는 함수는 스스로도 조립할 수 있어야 한다 다음절을 살펴보기 전에 4장에 살펴본 커맨드 객체 생성 로직에 지금까지 확인한 검증 기능을 적용해 보자.  
```javascript
var validateCommand = condition1(
  validator("arg must be a map", _.isObject),
  validator("arg must have the correct keys", haskeys("msg", 'type')));

  var createCommand = partial(validateCommand, _.identity);
```
왜 createCommand 함수의 논리 부분에 _ .identity 함수를 사용했을까? 자바스크립트에서는 훈련과 심사숙고를 거쳐야 안정성이라는 목표를 달성 할 수 있다. createCommand의 목표는다음 코드에서 볼 수 있는 것처럼 커멘드 겍체를 만들고 검증하는 일반적인 함수를 제공하는것이다.  
```javascript
createCommand({}); // Error : arg must right keys

createCommand(21); // Error : arg must be a map, arg must right keys

createCommand({msg:"", type:""}); //=> {msg:"", type: ""}
```
함수 조립을 이용하면 기존의 생성 추상화를 기반으로 조직이나 검증 자체를 구현 할 수 있다 어떤 키의 존재를 요구하는 파생된 커맨드 형식을 만들고 싶다면 다음 코드처럼 createCommand를 이용할 수 있다.  
```javascript
var createLaunchCommand = partial1(
  condition1(
    validator("arg must be the count dowm", haskeys('count Down'))
  )
)
```
다음코드는 예상대로 작동함을 보여준다.
```javascript
```
커링을 이용해서 함수를 만들든 아니든 제약이 있다. 두 기법 모두 자신의 인자를 하나이상을 이용해서 함수를 조립한다는 것이다. 함수의 인자와 반환값의 관계에 따라 조립할 함수를 결정하는 상황이 있음을 쉽게 예상할 수 있으며 다음에 함수의 끝을 서로 연결하는 함수 조립 방법을 설명한다.  

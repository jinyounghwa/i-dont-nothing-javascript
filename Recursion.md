재귀함수  
자기자신을 호출하는 함수  
지금까지는 재귀와 함수형 프로즈래밍이 서로 연관이 있는것으로 배웠거나 아니면 적어도 둘을 함께 배웠을 것이다. 우선은 재귀를 이해하는것이 왜 중요한지 세 가지 이유를 알아보자.  
+ 재귀 솔루션은 일반적인 문제를 하위의 작은 문제로 분리한 다음에 하위 문제를 하나의 추상화로 만들어서 문제를 해결한다.  
+ 재귀는 변이 상태를 숨길 수 있다.  
+ 재귀로 게으름(laziness) 그리고 무한히 큰 구조도 구현 할 수 있다.  

배열을 받아서 배열의 길이(요소의 수) 를 반환하는 mylength라는 함수를 다음처럼 설명 할 수 있다.  
1. size는 0부터 시작이다.
2. 배열에 루프를 돌면서 각 항목을 발견할 때마다 size를 1 증가시킨다.  
3. 위 동작이 끝났을 때 size가 배열의 길이이다.  

이렇게 하면 mylength라는 함수를 구현할 수는 있지만 재귀와는 아무런 관련이 없다. 이번에는 다음처럼 재귀를 이용한다.  
1. 빈 배열일 때는 길이가 0이다.  
2. 배열의 나머지로 mylength를 호출한 결과에 1을 더한다.

위 두 규칙을 이용하여 mylength를 구현했다.  
```javascript
function myLength(ary) {
  if(_.isEmpty(art))
  return 0;
  else
  return 1 + myLength(_.rest(ary));
}
```
재귀함수를 이용하면 매우 효과적으로 값을 만들 수 있다. 큰 문제는 하위문제의 어떤 값으로 이루어졌다는 사실을 파악하는것이 재귀 솔루션의 핵심이다. myLength예제에서는 하나의 요소를 가진 배열 여러개에 빈 배열과 길이를 더하는 것으로 전체 솔루션을 구성할 수 있다. myLength는 _ .rest 의 결과로 자신을 호출하므로 결국 빈 배열이 넘어올 때까지 한 요소가 적은 요소를 인자로 받는다.  

다음은 myLength동작 모습이다.  
```javascript
 myLength(_.range(10)); //=> 10

 myLength([]); //=>0

 myLength(_.range(1000)); //=> 1000
```
골치아픈 상황을 피하고 싶다면 재귀 함수에 주어진 인자는 바꾸지 않는것이 좋다는 사실을 기억하라.  
```javascript
var a = _.range(10);

myLength(a); //=> 10

a; //=>[0,1,2,3,4,5,6,7,8,9]
```
얼핏 재귀함수가 자신의 인자를 소비하는것처럼 보일 수 있지만 실제로 재귀 함수는 인자를 소비하지 않는다. myLength는 입력 배열의 길이를 정수로 반환했지만 배열, 객체를 포함한 모든 (합법적인) 값을 이용하도록 재귀 함수를 만들 수 있다. 예를들어 숫자와 배열을 인자로 받아서 배열의 내용을 숫자만큼 반복하고 새로운 배열에 채워서 반환하는 cycle이라는 함수가 있다고 가정하자.  
```javascript
function cycle(times, ary) {
  if(times <=0)
  return [];
  else
  return cat(ary, cycle(times -1, ary))
}
```
cycle함수는 myLength함수와 비슷해 보인다. myLength는 입력 배열을 소비 한 반면, cycle은 반복 횟수를 소비했다. 반복되는 새로운 배열이 만들어 진다.
다음은 cycle을 실행한 결과다.  
```javascript
cycle(2,[1,2,3]); //=> [1,2,3,1,2,3]

_.take(cycle(20,[1,2,3]),11); //=>[1,2,3,1,2,3,3,2,1]
```
이번에는 언더스코에서 제공하는 _ .zip 함수의 반대 동작을 수행하는  unzip이라는 함수를 만들어 보자  
```javascript
_.zip(['a','b','c'],[1,2,3]);
//=> [['a',1],['b',2]['c',3]]
```
언더스코어의 _ .zip는 두 배열을 받아서 첫 번째 배열과 두 번째 배열의 요소를 서로 쌍으로 만든다. _ .zip으로 생성한 '언집' 하는 함수를 구현하려면 '쌍'으로 맺어진 배열의 특성을 생각해 봐야 한다. 다음처럼 기초적인 배열을 언집할 수 있다면 이를 응용해서 전체 문제를 해결 할 수 있다.  
```javascript
var zipped1 = ['a',1];
```
 zipped1보다 더 기초적인 상황은 빈 배열[] 이다. 빈 배열을 언집하면 [[],[]]가 되며 이는 종료상황에 해당한다. 배열 zipped1은 문제를 가정할 수 있는 가장 간단한 상황이며 언집한 결과는 [['a'],[1]]이 될 것이다. 종료상황이 [[],[]]일 때 어떻게 하면 zipped1을 언집해서 [['a'],[1]]을 얻을 수 있을까?  
 zipped1의 첫 번째 요소 배열을 종료 상황의 첫 번째 배열로 넣고, 두 번째 배열요소를 종료 상황의 두 번째 배열로 넣으면 문제를 해결 할 수 있다. 지금까지 설명을 다음처럼 constructPair이라는 함수로 추상화 할 수 있다.  
 ```javascript
 function constructPair(pair, rests) {
   return [constrct(_.first(pair), _.first(rests)),
          constrct(secound(pair), secound(rests));]
 }
 ```
 constructPair로는 범용 '언집' 기능을 수행하기에 부족하다. 하지만 빈 배열과 constructPair를 이용하여 수동으로나마 zipped1을 언집할 수 있다.  
 ```javascript
 constructPair(['a', 1], [[],[]]);
 //=> [['a'],[1]]

 _.zip(['a'],[1]);
//=> [['a'],1]

_.zip.apply(null, constructPair(['a',1], [[],[]]));
//=>[['a',1]]

 ```
 다음처럼 constructPair를 활용해서 좀 더 큰 자료도 언집 할 수 있다.  
 ```javascript
 constructPair(['a',1]),
  constructPair(['b',2]),
   constructPair(['c',3],[[],[]]);
   //=> [['a','b','c'],[1,2,3]]
 ```
 constructPair가 어떻게 작동하는지 이해했다면 이를 기반으로 unzip이라는 재귀 함수를 만들 수 있다.  
 ```javascript
 function unzip(pairs) {
   if(_.isEmpty(pairs)) return [[],[]];
   return constructPair(_.first(pairs), _.unzip(_.rest(pairs))) // pairs는 엔트리가 들어오고 소비되는 과정이며 _.unzip(_.rest(pairs)는 계속 깍으면 나중에 없어진다.
 }
 ```
 unzip에서 발생하는 재귀 호출은 빈 배열이 될 때가지 계속된다. 빈 배열을 만나면 지금까지 누적된 부분 배열을 거슬러 가면서 constructPair이용해서 언집된 배열로 바꾼다. unzip을 이용하면 _ .zip으로 만들어진 배열 쌍을 '이전상태로 돌릴' 수 있다.  
 ```javascript
 unzip(_.zip([1,2,3],[4,5,6])); //=> [[1,2,3],[4,5,6]]
 ```
 myLength, cycle, unzip은 모두 재귀처리(즉,자신을 호출하는)함수다. 재귀 함수를 만들 때는 다음 규칙을 지키는것이 좋다고 알려져 있다.  
 + 언제 멈출지 알아야 한다.  
 + 한 단계에서 무엇을 실행할 지 결정한다.  
 + 문제를 더 작은 문제나 아니면 한 단계로 풀 수 있는 문제로 작게 분리한다.  


 재귀의 규칙  

 <table>
     <tr>
     <td>함수</td>
     <td>언제멈출지</td>
     <td>한단계수행</td>
     <td>작은문제</td>

   </tr>
   <tr>
   <td>myLength</td>
   <td>_ .isEmpty</td>
   <td>1 + ...</td>
   <td>_ .rest</td>
 </tr>
 <tr>
 <td>cycle</td>
 <td>times <= 0</td>
 <td>cat(array ...)</td>
 <td>times -1</td>
 </tr>
 <tr>
 <td>unzip</td>
 <td>_ .isEmpty</td>
 <td>constructPair(_ .first)</td>
 <td>_ .rest</td>
 </tr>

 </table>

 위 규칙을 템플릿으로 삼아 직접 재귀 함수를 만들 수 있다. 아무리 복잡한 재귀 함수라도 이런 재귀의 규칙을 벗어나지 않는다. 좀 더 복잡한 예제를 살펴보면서 규칙의 유사성을 알아보자.  

 재귀를 이용한 그래프 탐색  
 Node와 Arc객체 형식을 이용해서 언어와 연결 관계를 객체나 클래스 기반으로 표현 할 수도 있지만, 여기서는 다음처럼 간단한 문자열의 배열 형식으로 표현했다.  
 ```javascript
 var influences = [
   ['Lisp', 'Smalltalk'],
   ['Lisp', 'Scheme'],
   ['Smalltalk', 'Self'],
   ['Scheme', 'Javascript'],
   ['Scheme', 'Lua'],
   ['Self', 'Lua'],
   ['Self', 'Javascript']
 ];
 ```
 중첩배열 influences는 영향을 받은 언어의 관계를 의미한다. 재귀함수 nexts는 다음과 같이 정의한다.  
 ```javascript
 function nexts(graph, node) {
   if(_.isEmpty(graph) return []

   var pair = _.first(graph);
   var from = _.first(pair);
   var to = secound(pair);
   var more = _.rest(graph);

   if (_isEqual(node, from))
    return constrct(to, nexts(more, node));
    else
      return nexts(more,node);
 )
 }
 ```
 다음 코드에서 확인할 수 있듯이 nexts함수는 재귀적으로 그래프를 탐색하면서 특정 프로그래밍 언어에 영향을 받은 프로그래밍 언어 배열을 반환한다.  
 ```javascript
 nexts(influences, "Lisp");
 //=>['Smalltalk', 'Scheme']
 ```
 nexts의 재귀 호출은 지금까지 살펴본 재귀 호촐과는 다르다. nexts에서는 if문의 두 분기문 모두에서 재귀 호출이 일어난다. next의 'then'분기문에서는 주어진 대상 노드를 확인하며 else분기문에서는 중요하지 않은 노드를 무시한다.  
 <table>
     <tr>
     <td>함수</td>
     <td>언제멈출지</td>
     <td>한단계수행</td>
     <td>작은문제</td>

   </tr>
   <tr>
   <td>nexts</td>
   <td>_ .isEmpty</td>
   <td>constrct(...)</td>
   <td>_ .rest</td>
 </tr>
 </table>

메모리에서 깊이 우선 재귀 탐색하기  
여기서는 그래프 탐색을 이용해서 깊이 우선 탐색 함수 구현을 살펴본다. 함수형 프로그래밍에서는 데이터 구조체에서 특정 데이터를 탐핵하는 상황이 빈번하게 발생하는 편이다. influences같은 계층 구조의 그래프 데이터에서는 자연스럽게 재귀적으로 탐색을 수행한다. 기본적으로 주어진 노드를 찾으려면 그래프의 모든 노드를 탐색해야 한다. 깊이 우선 탐색 기법은 현재 노드를 기준으로 가징 왼쪽의 노드부터 방문하는 방식으로 탐색을 수행한다.  

이전의 재귀 구현과 달리 depthSearch는 이미 탐색한 노드를 메모리에 기억한다. 현제 예제 그래프에서는 사이클이 없이느 괜찮지만 사이클이 존재하는 노드에서는 문제가 발생할 수 있으므로 이미 탐색한 노드를 메모리에 기억해야 하며 그렇지 않으면 자바스크립트가 무한수행 모드로 빠져든다.재귀호출에서 호출 간의 통신 수단은 인자 뿐이므로 누산기라는 인자를 이용해서 메모리를 한 호출에서 다른 호출로 전달 할 필요가 있다. 보통 재귀 호출에서는 누산기 인자를 통해 정보를 전달한다. 다음 누산기를 이용해서 depthSearch를 구현한 코드다.  

```javascript
function depthSearch(graph, nodes, seen) {
  if(_.isEmpty(nodes)) return rev(seen);

  var node = _.first(nodes);
  var more = _.rest(nodes);

  if(_.contains(seen,node))
  return depthSearch(graph, more, seen);
  else {
    return depthSearch(graph,
                        cat(nexts(graph,node),more),
                        constrct(node, seen));
  }
}
```
세번째 파라미터 seen은 기존 노드와 그 자식을 탐색하지 않도록 이전에 탐색한 노드를 누적하는 역할을 한다. 다음은 depthSearch사용 예제다.  
```javascript
depthSearch(influences,['Lisp'],[]);
//=> ['Lisp',"Smalltalk", 'Self', 'Lua', "javascript","Scheme"]

depthSearch(influences,['Smalltalk', 'Self']);
//=> ['Smalltalk', 'Self', 'Lua', 'Javascript']

depthSearch(constrct(['Lua', 'Io'], influences),['Lisp'],[]);
//=> ['Lisp', 'Smalltalk', 'Self', 'Io', 'Javascript', 'Scheme']
```
depthSearch함수는 아무것도 하지 않는다는 사실을 파악했을 것이다. depthSearch함수는 단지 뭔가를 깊이 우선 순서로 실행 할 수 있도록 노드 배열을 만들 뿐이다. 나중에 함수형 조립과 상호재귀를 이용해서 깊이 우선 전략을 다시 구현할 것이므로 지금은 이정도로 충분하다. 먼저 꼬리 호출을 보자.  

꼬리 재귀  
depthSearch도 기존의 재귀 함수와 비슷해 보일 수 있지만 사실 depthSearch는 기존의 재귀함수와 중요한 차이가 있다.  
<table>
    <tr>
    <td>함수</td>
    <td>언제멈출지</td>
    <td>한단계수행</td>
    <td>작은문제</td>

  </tr>
  <tr>
  <td>nexts</td>
  <td>_ .isEmpty</td>
  <td>constrct(...)</td>
  <td>_ .rest</td>
</tr>
<tr>
<td>depthSearch</td>
<td>_ .isEmpty</td>
<td>depthSearch(more..)</td>
<td>depthSearch(cat...)</td>
</tr>
</table>

'한 단계 수행' 과 '작은문제' 항목에서 어떤차이인지 드러난다. 두 가지 항목 중 하나 이상이 재귀면 꼬리재귀라고 한다. 즉, 함수에서 발생한 마지막 동작(종료 요소를 반환하는 상황은제외)이 재귀 호출이다. depthSearch의 마지막 호출이 재귀 호출이므로 함수의 바디는 다시 사용되지 않는다. 스킴같은 언어에서는 이런 특성을 이용하여 꼬리 재귀 함수 바디에서 사용된 리소스를 헤체한다.  
다음은 꼬리 재귀를 이용해서 myLength를 다시 구현한 코드다.  
```javascript
function tcLength(ary, n) {
  var l = n ? n : 0

  if (_.isEmpty(ary))
    return l;
    else
    return toLength(_.rest(ary), l +1)
}
toLength(_.range(10)); //=>10
```
그러나 myLength의 재귀 호출(즉, 1+..) 에서는 마지막 덧샘을 수행하기 위해 함수의 바디를 다시 방문한다. 언젠가 꼬리 재귀 함수를 최적화하도록 자바스크립트 엔진이 개선될 것이다.   

재귀 함수와 합성 함수  
찬반형 몇 개를 인자로 받아 인자 모두의 결과가 참인지 여부를 결정하는 함수를 반환하는 checker 함수를 만들었다. 이번에는 checker와 같은 동작을 수행하는 andify라는 재귀함수를 만들겠다.  
```javascript
function andify(/*preds*/) {
  var preds = _.toArray(arguments);

  return function(/*args*/) {
    var args = _.toArray(arguments);
    var everything = function(ps, truth) {
      if(_.isEmpty(ps))
        return truth;
        else
        return _.every(args, _.first(ps))
              && everything(_.rest(ps), truth)
    };
    return everything(preds, true);
  }
}
```
실행결과이다.  
```javascript
var evenNums = andify(_.isNumber, isEven);

evenNums(1,2); //=> false

evenNums(2,4,6,8); //=> true

evenNums(2,4,6,8,9); //=> false
```
위 코드에서 andify가 반환하는 함수에서 일어나는 재귀 호출에 주목하자 논리 and연산자 && 는 게으른 방식으로 동작하므로 _ .every검사가 실패하면 재귀 호출이 발생하지 않는다. 이런종류의 게으름을 단락이라고 하며 단락의 특성을 이용하면 불필요한 연산을 줄일 수 있다.  

상호재귀 함수  
서로를 호출하는 두개 이상의 함수를 상호재귀 함수라고 한다. 다음에 소개할 짝수인지 홀수인지 검사하는 찬반형 함수는 서로를 호출하는 간단한 상호 재귀 함수 예제다.  
```javascript
function evenSteven(n) {
  if (n ===0)
    return true;
    else
    return oddJohn(Math.abs(n) -1);
}
function oddJohn(n) {
  if (n===0)
    return false;
    else
      return evenSteven(Math.abs(n) -1);
}
```
상호재귀 호출이 일어날 때마다 n이 0이 될 때까지 n을 -1씩 감소시킨다. 각 함수는 다음 코드에서 확인 할 수 있는 것처럼 예상대로 작동한다.  
```javascript
evenSteven(4); //=> true

oddJohn(1); //=> true
```
고차원 함수를 고집하는 독자일수록 상호 배타적인 함수를 더 자주 접할 것이다. 일례로 _ .map 함수를 생각해 보자. 실제로 자신과 _ .map을 호출하는 함수는 상호 재귀를 간접적으로 설명하고 있는것이나 마찬가지다. 다음은 단일 수준으로 배열을 평준화 하는 함수의예제다.  
```javascript
function flat(array) {
  if(_.isArray(array))
    return cat.apply(cat, _.map(array, flat));
  else
  return [array];
}
```
flat함수는 중첩된 각 요소를 배열로 만든 다음에 이들 배열을 재귀적으로 연결하는 방법을 이용해서 중첩된 배열을 평탄화 시킨다. 다음은 flat을 수행한 결과다.  
```javascript
flat([1,2],[3,4]); //=> [1,2,3,4]

flat([[1,2],[3,4],[5,6]],[[[7]],[[8]]]); //=>[1,2,3,4,5,6,7,8]
```
시살 flat은 상호재귀를 명확하게 보여주는것은아니지만 의미는 크다.  

재귀를 이용한 깊은 복제  
깊은 방식으로 객체를 복제할 때 재귀를 이용하면 효율적으로 작업 할 수 있다. 언더스코어도 _ .clone 함수를 제공하지만 이는 깊은 복제가 아닌 얕은 복제로 동작한다.  
```javascript
var x = [{a:[1,2,3],b:42},{c:{d:[]}}];

var y = _.clone(x);

y;
//=> [{a:[1,2,3],b:42},{c:{d:[]}}];

X[1]['c']['d'] = [100000]
y;
//=> [{a:[1,2,3],b:42},{c:{d:100000}}];
```
_ .clone도 상당히 유용한 함수지만 어떤 객체아ㅗ 그 객체의 하위객체까지 모두 복제해야 할 때가 있다. 재귀에서는 중첩방식을 이용해서 모든 객체를 탐색하면서 복제할 수 있으므로 어떤 객체와 그 객체의 하위객체까지 모두 복제 할 때는 재귀를 이용하는것이 좋다. 실제 업무에서 사용할 수 있을 정도의 수준은 아니지만 deepClone은 재귀 방식으로 객체를 복제하는 방식을 보여주는 함수다.  

```javascript
function deepClone(obj) {
  if (!existy(obj) || !_.isObject(obj))
  return obj;
  var temp = new obj.constructor();
  for (var key in obj)
  if (obj.hasOwnProperty(key))
    temp[key] = deepClone(obj[key])

    return temp;
}
```
deepClone에서 숫자와 같은 기본형이 나타나면 해당 데이터를 그냥 반환한다. 그러나 객체를 반환하면 객체를(모든데이터를 표현 할 수 있는) 연상 구조체로 간주하면서 객체의 모든 키 값 매핑을 재귀적으로 복사한다. 프로토 타입의 필드는 복사하는 일이 없도록 obj.hasOwnProperty(key)를 이용했다. 개인적으로 객체를 맵같은 연상 데이터 구조체로 사용하며, 불가피한 상황을 제외하면 데이터를 프로토타입으로 추가하지 않는다. 다음 코드는 deepClone을 활용하는 모습을 보여 준다.  
```javascript
var x = [{a:[1,2,3], b:42}, {c:{d:[]}}];

var y = deepClone(x);

_.isEqual(x,y);
//=>true

y[1]['c']['d'] = 42;
_.isEqual(x,y);
//=> false
```
자바스크립트에서는 모든것이 객체 기반인 덕분에 재귀 기법이 간단명료해진다는 점 이외에는 deepClone에서 특이한 점을 찾을 수 없다 다음 상호재귀를 이용해서 실제로 어떤 동작을 수행하는지 알아보자.  

중첩된 배열 탐색  
다음과 같은 패턴이 자주 보일것이다.  
```javascript
doSomethingWithResult(_.map(someArray, SomeFun));
```
_ .map호출 결과는 추가로 필요한 작업을 처리할 수 있도록 다른 함수로 넘겨진다. 다음에 소개할 visit라는 함수는 충분히 추상화 된 함수다.  
```javascript
function visit(mapFun, resultFun, array) {
  if(_.isArray(array))
    return resultFun(_.map(array, mapFun));
  else
  return resultFun(array);
}
```
visit함수는 처리할 배열 외에 함수를 인자로 받는다. 인자로 받은 mapFun을 배열의 각 요소에 호출한 다음에 최종 작업을 수행할 수 있도록 결과 배열을 resultFun으로 넘긴다, 배열로 넘겨준 요소가 배열이 아니면 해당 요소에 resultFun을 수행한다 부분적용을이용해서 visit로 다양한 동작을 구현할 때 이런 유형의 함수가 매우 유용하다.  

너무 깊은 재귀  
꼬리 재귀를 살펴볼 때 언급했던 것처럼 자바스크립트 엔진은 기술적으로는 충분히 할 수 있음에도 불구하고 재귀호출을 최적화하지 않았다.  
```javascript
evenSteven(100000) // Too much recursion
```
evenSteven과 oddJohn이 상호 재귀적으로 서로를 수천 번 이상 호출하면서 너무깊은 재귀 에러가 발생할 수 있다. 여기서는 트램펄린 이라 불리는 구조를 이용해서 스택 깨짐을 피하는 방법을 설명한다. 중첩 호출을 평탄화 시킨 호출로 바꾸는것이 트램펄린의 기본 원리다.  
```javascript
function even0line(n) {
  if (n === 0)
    return true;
  else
    return partial1(odd0line, Math.abs(n) -1);
}
function odd0line(n) {
  if (n === 0)
    return false;
  else
    return partial1(even0line, Math.abs(n) -1);
}
```
even0line 과 odd0line의 바디에서 상호 재귀함수를 호출하는 대신 상호 재귀를 감싸는 함수를 반환한다. 각 함수에 종료 상황을 입력해 보면 예상대로 작동함을 확인 할 수 있다.  
```javascript
even0line(0); //=>true

odd0line(0); //=> false
```
이제 다음코드처럼 상호재귀를 수작업으로 평탄화 시킬 수 있다.  
```javascript
odd0line(3);
//=> function () {return even0line(Math.abs(n) -1)}
```

trampoline 함수는 호출을 더 근사하게 프로그램적으로 평탄화 시킨다.  
```javascript
function trampoline(fun /*args*/) {
  var result = fun.apply(fun, _.rest(arguments))

  while (_.isFunction(result)) {
    result = result();
  }
  return result;
}
```
trampoline은 단순하게 함수의 반환값을 함수가 아닐때가지 호출한다. 실행결과는 다음과 같다.  
```javascript
trampoline(odd0line, 3) //=> true
```
trampoline을 이용하면 호출 체인이 간접적으로 이루어지면서 기존의 상호 재귀 호출에 비해 추가적으로 오버헤드가 더해진다. 하지만 일반적으로 스택이 깨지는 것보다 속도가 느려지는것이 낫다.  
함수형 파사드를 이용하면 trampoline을 완전히 감출 수 있다.  
```javascript
function isEvenSafe(n) {
  if ( n===0)
    return true;
  else
  return trampoline(partial1(odd0line, Math.abs(n)-1))
}
function isOddSafe(n){
  if (n===0)
    return false;
  else
  return trampoline(partial1(even0line, Math.abs(n) -1))
}

isOddSafe(2000001) //=> true
```

발생기  
```javascript
_take(cycle(20,[1,2,3])11);

//=> [1,2,3,1,2,3,1,2,3,1,2,3,1,2]
```
언더스코에는 _ .first와 _ .rest 라는 함수를 제공한다. 마찬가지로 무한한 배열을 첫 번째는 머리와 나머지는 꼬리로 분석 할 수 있다. head와 tail을 이용해서 객체를 만들어 이와같은 분석을 표현 할 수 있다.  
```javascript
(head : aValue, tail:???)
```
객체의 tail에는 무엇을 포함시켜야 할 까 라는 질문이 생긴다. head/tail 객체를 만들려면 약간의 노고가 필요하다. 현재 셀의 값을 계산할 함수 그리고 다음 시드값을 계산할 함수가 필요하다. 사실 이와 같은 구조는 발생기로 알려진 구조의 일종이다. 발생기의 구현 코드를 살펴보자.  
```javascript
function generator(seed, current, step) {
  return {
    head : current(seed),
    tail : function() {
      console.log("forced");
      return generator(step(send), current, step);
    }
  }
}
```
코드에서 확인할 수 있듯이 current는 head위치의 값을 계산하는 함수이고 step은 재귀 호출에 제공하는 값이다. tail값은 function에 래핑되어 실제 호출이 일어날 때까지는 실현되지 않는다는것을 눈여겨본다.

genTake같은 생성기에서 첫번째 n개의 항목을 배열로만드는 강력한 접근자 함수를 만들 수 있다.   
```javascript
function genTake(n,gen) {
  var doTake = function(x,g,ret) {
    if (x === 0)
      return ret;
    else
      return partial1(doTake, X-1, gentail(g), cat(ret,genHead(g)))
  };
  return trampoline(doTake, n, gen, []);
}
```
위 예제는 트램펄린 원칙을 이용해서 스택을 깨드리지 않으며 요청시아 값을 계산한다는 목표를 달성하는 동시에 무한한 크기의 구조체를 정의하는 방법을 보여 준다. generator를 이용하년 genTake 에는 매번 셀을 계산해줘야 한다는 치명적인 결함이 있다. 

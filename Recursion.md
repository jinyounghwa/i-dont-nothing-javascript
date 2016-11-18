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

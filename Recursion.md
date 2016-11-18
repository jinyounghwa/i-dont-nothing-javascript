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
 <table>
     <tr>
     <td>break</td>
     <td>do</td>
     <td>instanceof</td>
     <td>typeof</td>

   </tr>
   <tr>
   <td>case</td>
   <td>else</td>
   <td>new</td>
   <td>var</td>
 </tr>
 <tr>
 <td>catch</td>
 <td>finally</td>
 <td>return</td>
 <td>void</td>
 </tr>
 <tr>
 <td>continue</td>
 <td>for</td>
 <td>switch</td>
 <td>while</td>
 </tr>
 <tr>
 <td>debugger</td>
 <td>function</td>
 <td>this</td>
 <td>with</td>
 </tr>
 <tr>
 <td>in</td>
 <td>try</td>
 <td>-</td>
 <td>-</td>
 </tr>
 </table>

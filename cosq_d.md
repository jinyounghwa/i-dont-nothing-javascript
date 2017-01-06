배열의 메소드(함수) 중 concat 과 join의 사용법을 익힌 후, markdown문법으로 작성하고, github에 올려, 그 url을 남겨주세요.  

concat   
concat은 이 메서드를 호출한 배열 뒤에 각 인수를 순서대로 붙인 새 배열을 만듭니다. 인수가 배열이면 그 성분이 순서대로 붙고, 배열이 아니면 그 인수 자체가 붙습니다.  

concat은 this나 인수로 넘겨진 배열의 내용을 바꾸지 않고 대신 주어진 배열들을 합친 뒤 그것의 얕은 사본(shallow copy)을 반환합니다. 원본 배열의 요소들은 새 배열에 다음과 같은 방법으로 복사됩니다.    
```javascript
var a = [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ];
var b = [ "foo", "bar", "baz", "bam", "bun", "fun" ];
```

a와 b의 단순 연결은 분명히 다음과 같습니다.  
```javascript
[
   1, 2, 3, 4, 5, 6, 7, 8, 9,
   "foo", "bar", "baz", "bam" "bun", "fun"
]
```

가장 일반적인 방법은 다음과 같습니다.  
```javascript
var c = a.concat( b );

a; [1,2,3,4,5,6,7,8,9]
b; ["foo","bar","baz","bam","bun","fun"]

c; [1,2,3,4,5,6,7,8,9,"foo","bar","baz","bam","bun","fun"]
```
보시다시피, c는 두 개의 a와 b 배열의 조합을 나타내는 완전히 새로운 배열이며 a와 b는 그대로 둡니다. 간단 하죠? a가 10,000 개이고 b가 10,000 개라면? c는 이제 20,000 개의 항목으로 a와 b의 메모리 사용을 기본적으로 두 배로 늘립니다."문제 없습니다!"라고 말합니다. 우리는 a와 b의 설정을 해제하여 가비지 컬렉팅을합니다.

join  
join() 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.  
모든 배열 요소가 문자열로 변환된 다음 하나의 문자열로 연결됩니다. 값이 undefined이거나 null인 요소는 빈 문자열로 변환됩니다.  

```javascript
var arr = new Array("First","Second","Third");

         var str = arr.join();
         document.write("str : " + str );

         var str = arr.join(", ");
         document.write("<br />str : " + str );

         var str = arr.join(" + ");
         document.write("<br />str : " + str );
```

결과는 다음과 같습니다.  
```javascript
str : First,Second,Third
str : First, Second, Third
str : First + Second + Third  
```

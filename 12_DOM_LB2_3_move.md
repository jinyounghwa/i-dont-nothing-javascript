##### 이 글은 프론트엔드 개발자를 위한 자바스크립트 프로그래밍 요약입니다.
##### 공부하기위한 비공식 문서로 블로그에 기재하지 않습니다.
<hr>

이동  
DOM의 이동은 중첩 깊이에 따른 이동이며 타입에 따라 다르긴 하지만 최소 두 가지 방향으로 이동 가능하다. 주어진 노드가 출발점이 되며 DOM트리를 거슬로 올라가지는 못한다. 다음 HTML을 확인해 보자  
```
<!DOCTYPE html>
<html>
  <head>
    <title>
      Example
    </title>
  </head>
  <body>
  <p><b>Hello</b>world</p>
  </body>
</html>
```
위 페이지의 DOM트리는 그림과 같다.  

![Minion](https://github.com/jinyounghwa/i-dont-nothing-javascript/blob/master/image/12_4.png)  
어떤 노드는 출발점이 될 수 있다.

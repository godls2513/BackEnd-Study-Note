## CRUD
컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능을 일컬어 CRUD라고 한다.<br>
CRUD는 다음과 같다<br>
- C : Create(생성)
- R : Read(읽기)
- U : Update(갱신)
- D : Delete(삭제)

참고<br>
[CRUD](https://ko.wikipedia.org/wiki/CRUD)

## CQS (Command Query Seperation)
CQS는 메서드의 동작을 수행하는 명령어(Commmand)와 데이터의 상태를 반환하는 쿼리(Query)를 분리하는 프로그래밍 원칙이다.<br><br>

이를 CRUD에 적용해보면 다음과 같다.
- Command -> Create, Update, Delete => 상태가 변함, 안전하지 않음
- Query -> Read => 상태가 변하지 않음. 안전함. 분산, 캐시 등이 수월함.

참고<br>
[CQS](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation)

## HTTP Method

REST API에서는 HTTP Method와 Collection Pattern을 이용하여 CRUD를 표현할 수가 있다.<br><br>
다음은 HTTP Method와 CRUD의 연관관계를 표로 정리했다.
|HTTP Method| CRUD  |
|-----------|-------|
|   GET     | Read  |
|   POST    | Create|
|PUT, PATCH | Update|
|  DELETE   | Delete|

**HTTP Method와 Collection Pattern**<br>

브라우저는 서버에 HTTP Method를 사용하여 클라이언트가 원하는 동작이나 상태에 대한 응답을 받는다. HTTP Method와 Collection Pattern을 조합해서 서버에 적절한 요청을 보낼 수가 있다.<br>

다음은 HTTP Method와 Collection Pattern을 조합하여 서버에 요청을 보내는 여러가지 예시이다.<br>

### 게시물
1. GET /posts <br>
 => 게시물 목록을 읽어오라고 요청한다.(Read, Collection)<br>
 => List를 표현하는 습관적인 방식이다.<br>
 
 2. GET /posts/{post_id}<br>
=> {post_id} 인 게시물을 읽어오라고 요청한다.(Read, Element)<br>
=> 게시물의 상세, Detail을 표현하는 습관적인 방식이다.<br>

3. POST /Posts<br>
=> 게시물 생성을 요청한다.(Create)<br>
=> {post_id}는 서버에서 생성한다.<br>

4. PUT 또는 PATCH /posts/{post_id}<br>
=> 게시물 수정을 요청한다.(Update, Element)<br>

5. DELETE /posts/{post_id}<br>
=> 게시물 삭제를 요청한다. (Delete, Element)<br>

종종 Bulk update, Bulk delete 등을 하기도 하는데, 이럴 때는 Collection을 활용하고, API 스펙 문서에 정확하게 기록한다.<br>

### 댓글

**바로 comments로 시작하는 경우**

1. GET /commments<br>
=> 전체 댓글 목록<br>

2. GET /comments?post_id={post_id}<br>
=> 특정 게시물의 댓글 목록<br>

3. GET /comments/{comments_id}<br>
=> 댓글의 상세<br>

4. POST /comments<br>
=> 댓글 생성, 이 때는 어떤 게시물에 대한 댓글인지 알 수 있도록 body에 {post_id}에 대한 정보를 담아줘야 한다.<br>

5. POST /comments?post_id={post_id}<br>
=> 특정 게시물에 대한 댓글 생성<br>

6. PUT 또는 PATCH /comments/{commennt_id}<br>
=> 특정 댓글의 수정, {comment_id}에 대한 댓글 수정<br>

7. DELETE /comments/{comments_id}<br>
=> 특정 댓글을 삭제, {comment_id}에 대한 댓글 삭제<br>

**특정 게시물 아래의 댓글로 표현하는 경우**<br>

1. GET /posts/{post_id}/comments<br>
=> 특정 게시물의 댓글 목록<br>

2. GET /posts/{post_id}/comments/{comment_id}<br>
=> 특정 게시물의 특정 댓글 상세<br>

3. POST /posts/{post_id}/comments<br>
=> 특정 게시물의 댓글 생성<br>

4. PUT 또는 PATCH /posts/{post_id}/comments/{comment_id}<br>
=> 특정 게시물의 특정 댓글을 수정<br>

5. DELTE /posts/{post_id}/comments/{comment_id}<br>
=> 특정 게시물의 특정 댓글 삭제<br>

### 로그인
로그인과 로그아웃의 기능을 구현하고자 한다면 session을 사용해볼 수 있다. 로그인은 식별하고자하는 유일한 ID를 가지기 때문이다.<br>

1. POST /session<br>
=> 새션 생성 = 로그인<br>

2. DELETE /session<br>
=> 세션 삭제 = 로그아웃<br>

3. GET /session<br>
=> 세션 확인... 내 정보 확인<br>

4. GET /users/me<br>
=> 내 정보확인, {user_id}를 me라고 쓰면 현재 사용자의 {user_id}로 처리하게 정하고, API 스펙 문서에 기록한다.

## 정리
CRUD는 데이터 처리 방식으로 create, read, update, delete<br>

CQS는 명령어-쿼리-분리 원칙으로 Command에 해당하는 create, update, delete와 Query에 해당하는 read는 서로 분리하여 구현한다는 원칙이다.<br>

마지막으로 HTTP Method는 CRUD와 연관이 있는데 각 메서드를 통해 클라이언트가 하고자 하는 동작이나 상태를 요청하고 collection pattern을 조합하여 적절한 URI를 만들 수가 있다.


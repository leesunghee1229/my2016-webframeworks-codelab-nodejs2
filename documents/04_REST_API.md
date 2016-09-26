REST API
===========

## 클라이언트쪽 REST API

서버 데이터를 구조적으로 사용하기 위한 API 디자인을 REST API라고 합니다. 무슨말인지요? 예를 들어보면 쉽게 이해됩니다. 당신이 사용자 목록을 조회하는 API를 만든다고 생각해 보세요. API 주소를 어떻게 만들수 있을까요?

```
/getUser
```

지금까지는 주로 이러한 방법이었습니다. (동의하지 않는 분도 있겠지만)

하지만 이렇게 설계해 보는 것을 어떨까요?

```
/users
```

"사용자"라는 것이 서버쪽에는 user라는 자원으로 정의할 수 있습니다. 그래서 서버의 user 자원을 조회하는 것이고 목록(복수)를 조회하기 때문에 복수형 users라는 이름으로 했습니다.

만약 1개의 유저를 조회한다면 어떻게 할수 있을까요? 우리는 자원 식별자로 id를 사용합니다.

```
/users/:id
```

목록을 조회하는 api 뒤에 id를 파라매터로 받는 방법입니다.

그럼 페이징은 어떻게 해야할까요? 페이징에는 limit, offset 파라매터를 사용할 수 있습니다.

```
/users?limit=&offset
```

그동안 많이 사용했던 쿼리 문자열(Query string)을 사용합니다.

마지막으로 이 api에는 숨겨진 정보가 하나있는데 메소드(Method) 명입니다. 이 api의 완벽한 표현 방법은 아래와 같습니다.

```
GET /users?limit=&offset=
GET /users/:id
```

리소스명이 명사로 표현 사용된다면 메소드명은 동사역할을 합니다. 그래서

* GET은 "조회하다"로,
* POST는 "생성하다"로,
* PUT은 "갱신하다",
* 그리고 DELETE는 "삭제하다"라는 동사와 매칭됩니다.

만약 사용자를 추가하는 api를 설계한다면 다음과 같이 설계할 수 있습니다.

```
POST /users
```


## 서버쪽 REST API

클라이언트가 이러한 요청에 대해 서버는 어떻게 응답해야 할까요?

서버가 응답은 두 부분으로 구분할 수 있습니다. 헤더와 바디. 바디는 제이슨(Json) 타입으로 응답하는 것인데 앞으로 진행하면서 다뤄볼 것입니다. 크게 어려운 부분은 없구요. 살펴봐야할 부분이 헤더입니다. 그 중에서도 헤더의 상태코드(Status code)를 잘 사용하면 다양한 정보를 담아서 클라이어트에게 전송할 수 있습니다.

응답 헤더의 상태코드는 세 자리 정수로 되어 있는데 크게 세 분류가 있습니다.

* 2XX: 성공
* 4XX: 클라이언트 요청 에러
* 5XX: 서버 응답 에러


### 2XX

2XX 상태코드는 클라이언트 요청이 올바르고 서버도 제대로 응답할 수 있을때 보내는 코드입니다.

* 200: Success. 대부분의 성공 응답에 200번 상태 코드를 사용합니다.
* 201: Created. POST 메소드로 요청한다는 것은 서버에 자원 생성을 요청하는 의미인데 서버쪽에서 자원 생성에 성공하면 201 상태코드를 클라이언트로 응답합니다.
* 204: No Content. 서버에서 성공했는데 응답할 바디가 없을 경우 204 상태코드를 반환합니다.


### 4XX

4XX 상태코드는 클라이언트 요청이 잘못 되었을 경우 응답하는 코드입니다.

* 400: Bad Request. 클라이언트에서 파라매터를 포함하여 서버 API를 요청하는데 파라매터가 잘못되었을 경우 응답하는 코드입니다.
* 401: Unauthorized. 인증이 필요한 API에 대해 인증되지 않은 요청일 경우 401을 응답합니다. 예를 들어 OAuth를 사용할 때 엑세스 토큰(access token)이 유효하지 않을 경우입니다.
* 403: Fobbiden. 401과 유사하면서 사용 방밥에 대한 해석은 개발자마다 다른것 같습니다. 저는 로그인 실패시 403으로 응답하고 있습니다.
* 404: Not found. 조회할 자원이 서버에 없는 경우 응답하는 코드입니다. 웹브라우져로 어떤 페이지를 찾을 때 그 페이지가 없는 경우 보통 404 페이지라고 부르기도 합니다.
* 409: Conflict. 클라이언트에서 POST 메소드로 서버에게 자원 추가를 요청했을 때 이미 그자원이 서버에 있어서 자원을 추가할 수 없는 경우 409 상태코드로 응답합니다.


### 5XX

마지막으로 클라이언트 요청은 정상이지만 서버에서 처리하는 과정중 에러가 발생하면 모두 500번 상태코드를 응답합니다.

좀더 자세한 내용에 대해서는 [서버 개발자 입장에서 바라본 모바일 API 디자인](http://blog.jeonghwan.net/2016/03/29/mobile-rest-api.html)을 참고해 주세요.
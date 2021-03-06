# HTTPNetwork 상태코드

상태코드는 클라이언트가 보낸 요청의 처리 상태를 리스폰스에서 알려주는 기능입니다. 상태코드를 통해 서버가 리퀘스트를 정상적으로 처리했는지, 결과가 에러였는지를 알 수 있습니다.

<br>

### 1) 1xx (Informational)

100번 대 상태코드는 "요청이 수신되어 처리중"이라는 뜻으로 거의 사용되지 않습니다.

<br>

### 2) 2xx (Success)

200번 대 상태코드는 "리퀘스트를 정상적으로 처리했음" 이라는 뜻입니다.

- #### 200 OK

  클라이언트가 보낸 리퀘스트를 서버가 정상 처리한 상황입니다. 리스폰스에서 상태 코드와 함께 되돌아 오는 정보는 메소드에 따라 다르고, GET 메소드의 경우에는 리퀘스트 된 리소스에 대응하는 엔티티가 보내지고, HEAD 메소드의 경우에는 리퀘스트된 리소스에 대응하는 엔티티 헤더 필드가 메시지 바디를 동반하지 않고 되돌아옵니다.

- #### 201 Created

  클라이언트가 보낸 리퀘스트를 서버가 정상 처리하여 새로운 리소스가 생성된 상황입니다. 생성된 리소스는 리스폰스의 Location 헤더 필드로 식별해줍니다.

- #### 202 Accepted

  클라이언트가 보낸 리퀘스트가 접수되었으나 아직 처리가 완료되지않은 상황입니다. 배치 처리 같은 곳에서 사용되는 코드입니다. 

  ( 요청 접수 후 1시간 뒤에 요청을 처리하는 배치 프로세스 )

- #### 204 No Content

  서버가 리퀘스트를 받아서 처리하는데 성공했지만 리스폰스에 보낼 데이터가 없어서 엔티티 바디를 포함하지 않는 상황입니다. 클라이언트에서 서버에 정보를 보내는 것으로 족하고, 클라이언트에 대해서 새로운 정보를 보낼 필요가 없는 경우에 사용됩니다.

- #### 206 Partial Content

  Range에 의해서 범위가 지정된 리퀘스트에 의해서 서버가 부분적 GET 리퀘스트를 받은 상황입니다. 리스폰스에는 Content-Range로 지정된 범위의 엔티티가 포함되게 됩니다.

<br>

### 3) 3xx (Redirection)

300번 대 상태코드는 "요청을 완료하기 위해 유저 에이전트의 추가 조치가 필요함" 이라는 뜻입니다. 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동합니다.

- #### 301 Moved Permanently ( 영구 리다이렉션 )

  리퀘스트된 리소스의 URI 가 영구적으로 변경된 상황입니다. 원래의 URL 를 사용하면 안되고, 새로운 URI를 사용해야한다는 것을 나타내고 있습니다. 리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될 수 있습니다. 

- #### 308 Permanent Redirect ( 영구 리다이렉션 )

  301과 기능 면에서는 동일하나 리다이렉트시 요청 메서드와 본문을 유지해준다. 즉, POST 로 처음에 요청을 보내면 리다이렉트시에도 POST를 유지해준다.

- #### 302 Found ( 일시적인 리다이렉션 )

  리퀘스트된 리소스의 URI가 변경되어있기 때문에 새로운 URI를 참조해 주길 바란다는 의미로 301과 달리 영구적으로 변경된 상황이 아니라 이동하는 곳의 URI는 앞으로도 이동 가능성이 있습니다. 리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될 수 있습니다.

- #### 303 See Other ( 일시적인 리다이렉션 )

  302와 기능 면에서는 동일하나 리다이렉트시 요청 메서드가 무조건 GET으로 변경됩니다. 즉, 리퀘스트에 대한 리소스는 다른 URI에 있기 때문에 GET 메소드를 사용해서 얻어야 한다는 뜻입니다.

- #### 307 Temporary Redirect ( 일시적인 리다이렉션 )

  302와 기능 면에서는 동일하나 리다이렉트시 요청 메서드와 본문을 유지해야됩니다. 즉, POST로 요청할 시 POST로 리다이렉트 해줍니다.

> 처음 302 스펙의 의도는 HTTP 메서드를 유지하는 것이였으나 웹 브라우저들이 대부분 GET 으로 바꾸어버렸다.
>
> --> 모호한 302를 대신하는 명확한 303, 307이 등장!

- #### 304 Not Modified

  클라이언트가 조건부 리퀘스트를 했을 때 리소스에 대한 액세스는 허락하지만, 조건이 충족되지 않음을 표시합니다. 304를 되돌려 줄 경우에는 리스폰스 바디에 어떤 것도 포함되어 있어서는 안됩니다. 캐시를 목적으로 사용되고 3xx에 분류되어 있디만 리다이렉트와는 관계가 없습니다.

> 참고!
>
> 301, 302 상태코드의 사양은 POST 메소드를 GET 메소드로 바꾸는 것을 허용하지 않았지만 대부분의 브라우저가 그렇게 구현을 해놓아서, GET 으로 변하고 본문이 제거될 수 있다고 스펙이 변경되었습니다.

<br>

### 4) 4xx (Client Error)

400번 대 상태코드는 클라이언트의 원인으로 에러가 발생했음을 나타냅니다. 클라이언트의 요청에 잘못된 문법이 포함되어있는 등 오류의 원인이 클라이언트에 있는 상황입니다. **클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에 재시도를 해도 똑같이 실패합니다.**

- #### 400 Bad Request

  요청 구문, 메시지 등등 리퀘스트 구문이 잘못되었음을 나타냅니다. 이 에러가 발생한 경우, 리퀘스트 내용을 재검토해서 재송신해야합니다.

- #### 401 Unauthorized

  해당 리소스에 유효한 인증 자격 증명이 없기 때문에 요청이 적용되지 않았음을 나타냅니다. 401 오류 발생시 응답에 `WWW_Authenticate` 헤더와 함께 구체적인 인증 방법을 명시해줍니다.

- #### 403 Forbidden

  리퀘스트된 리소스의 액세스가 거부되었음을 나타냅니다. 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우에 발생하고 서버 측은 거부의 이유를 엔티티 바디에 기재해서 유저 측에 표시합니다. ( 로그인은 했지만, 내 계정 권한으로 접근할 수 없는 정보를 접근하는 경우 )

- #### 404 Not Found

  리퀘스트한 리소스가 서버 상에 없는 경우 혹은 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶은 경우에 발생합니다.

<br>

### 5) 5xx (Server Error)

500번 대 상태코드는 서버 문제로 오류가 발생했음을 나타냅니다. **서버에 문제가 있기 때문에 재시도하면 성공할 수도 있습니다.**

- #### 500 Internal Server Error

  리퀘스트를 처리하는 도중에 서버 내부 문제로 오류가 발생한 경우입니다.

- #### 503 Service Unavailable

  서버가 일시적인 과부하 또는 예정된 작업으로 현재 리퀘스트를 처리할 수 없는 경우입니다. Retry-After 헤더필드로 얼마 뒤에 복구되는지 보낼 수 있습니다.

  
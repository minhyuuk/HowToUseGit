# JWT
### JWT란?
- JWT (Json Web Token)은 선택적 서명 및 선택적 암호화를 사용하여 데이터는 만들기 위한 인터넷 표준으로, 페이로드는 몇몇 클레임 표명을 처리하는 JSON을 보관하고 있다. 토큰은 비공개 시크릿 키 또는 공개/비공개 키를 사용하여 서명된다.
### JWT의 구성
![jwt example img](/img/jwt구성.png "jwt example")
- JWT는 세 파트로 나누어지며, 각 파트는 점로 구분하여 xxxxx.yyyyy.zzzzz 이런식으로 표현됩니다. 순서대로 헤더 (Header), 페이로드 (Payload), 서명 (Sinature)로 구성합니다.
 Base64 인코딩의 경우 “+”, “/”, “=”이 포함되지만 JWT는 URI에서 파라미터로 사용할 수 있도록 URL-Safe 한  Base64url 인코딩을 사용합니다.

1. Header는 토큰의 타입과 해시 암호화 알고리즘으로 구성되어 있습니다. 첫째는 토큰의 유형 (JWT)을 나타내고, 두 번째는 HMAC, SHA256 또는 RSA와 같은 해시 알고리즘을 나타내는 부분입니다.
    -  HEADER(헤더)
토큰의 헤더는 typ과 alg 두 가지 정보로 구성된다. alg는 헤더(Header)를 암호화 하는 것이 아니고, Signature를 해싱하기 위한 알고리즘을 지정하는 것이다.

        - typ: 토큰의 타입을 지정 ex) JWT

        - alg: 알고리즘 방식을 지정하며, 서명(Signature) 및 토큰 검증에 사용 ex) HS256(SHA256) 또는 RSA  
![jwt header example img](/img/jwtheader.png "jwt header example")
2.  Payload는 토큰에 담을 클레임(claim) 정보를 포함하고 있습니다. Payload 에 담는 정보의 한 ‘조각’ 을 클레임이라고 부르고, 이는 name / value 의 한 쌍으로 이뤄져있습니다. 토큰에는 여러개의 클레임 들을 넣을 수 있습니다.
클레임의 정보는 등록된 (registered) 클레임, 공개 (public) 클레임, 비공개 (private) 클레임으로 세 종류가 있습니다.
    - 토큰의 페이로드에는 토큰에서 사용할 정보의 조각들인 클레임(Claim)이 담겨 있다. 클레임은 총 3가지로 나누어지며, Json(Key/Value) 형태로 다수의 정보를 넣을 수 있다.  

    2.1 등록된 클레임(Registered Claim)

    등록된 클레임은 토큰 정보를 표현하기 위해 이미 정해진 종류의 데이터들로, 모두 선택적으로 작성이 가능하며 사용할 것을 권장한다. 또한 JWT를 간결하게 하기 위해 key는 모두 길이 3의 String이다. 여기서 subject로는 unique한 값을 사용하는데, 사용자 이메일을 주로 사용한다.
    ```
    iss: 토큰 발급자(issuer)

    sub: 토큰 제목(subject)

    aud: 토큰 대상자(audience)

    exp: 토큰 만료 시간(expiration), NumericDate 형식으로 되어 있어야 함 ex) 1480849147370

    nbf: 토큰 활성 날짜(not before), 이 날이 지나기 전의 토큰은 활성화되지 않음

    iat: 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음

    jti: JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며, 일회용 토큰(Access Token) 등에 사용
    ```
    2.2 공개 클레임(Public Claim)

    공개 클레임은 사용자 정의 클레임으로, 공개용 정보를 위해 사용된다. 충돌 방지를 위해 URI 포맷을 이용하며, 예시는 아래와 같다.  
![jwt public claim img](/img/jwtpublicclaim.png "jwt public claim img")  

    2.3 비공개 클레임(Private Claim)

    비공개 클레임은 사용자 정의 클레임으로, 서버와 클라이언트 사이에 임의로 지정한 정보를 저장한다. 아래의 예시와 같다.  
![jwt private claim img](/img/jwtprivateclaim.png "jwt private claim img")
3. 마지막으로 Signature는 secret key를 포함하여 암호화되어 있습니다.
    - 서명(Signature)은 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드이다. 서명(Signature)은 위에서 만든 헤더(Header)와 페이로드(Payload)의 값을 각각 BASE64로 인코딩하고, 인코딩한 값을 비밀 키를 이용해 헤더(Header)에서 정의한 알고리즘으로 해싱을 하고, 이 값을 다시 BASE64로 인코딩하여 생성한다.
### JWT의 목적
- 기존의 토큰 방식 인증은 다이어그램에 표시된 것처럼 토큰은 이후의 모든 서비스 호출에 사용됩니다.
서비스를 받기 위해서는 토큰의 유효성을 확인하여 세부 정보를 쿼리해야합니다.
참조에 의한 호출(By Reference) 형태로 모든 서비스는 항상 상호 작용할 때 다시 접속해야합니다.
- JWT 와 같이 값에 의한 호출이 가능한 토큰이 필요합니다.
토큰이 필요한 모든 정보를 포함하고 있어 참조(적어도 인증 및 권한 부여를 위해)가 필요없기 때문에 마이크로서비스 자체에서 유효성을 검증 합니다.
이것이 JWT ( JSON Web Token )의 목적입니다!
### JWT Process
1. 사용자가 id와 password를 입력하여 로그인을 시도합니다.
2. 서버는 요청을 확인하고 secret key를 통해 Access token을 발급합니다.
3. JWT 토큰을 클라이언트에 전달 합니다.
4. 클라이언트에서 API 을 요청할때  클라이언트가 Authorization header에 Access token을 담아서 보냅니다.
5. 서버는 JWT Signature를 체크하고 Payload로부터 사용자 정보를 확인해 데이터를 반환합니다.
6. 클라이언트의 로그인 정보를 서버 메모리에 저장하지 않기 때문에 토큰기반 인증 메커니즘을 제공합니다.
인증이 필요한 경로에 접근할 때 서버 측은 Authorization 헤더에 유효한 JWT 또는 존재하는지 확인한다.
JWT에는 필요한 모든 정보를 토큰에 포함하기 때문에 데이터베이스과 같은 서버와의 커뮤니케이션 오버 헤드를 최소화 할 수 있습니다.
Cross-Origin Resource Sharing (CORS)는 쿠키를 사용하지 않기 때문에 JWT를 채용 한 인증 메커니즘은 두 도메인에서 API를 제공하더라도 문제가 발생하지 않습니다.
일반적으로 JWT 토큰 기반의 인증 시스템은 위와 같은 프로세스로 이루어집니다.
처음 사용자를 등록할 때 Access token과 Refresh token이 모두 발급되어야 합니다.
### JWT 장단점
- ### JWT 장점
    - JWT 의 주요한 이점은 사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증 저장소가 필요없다는 것입니다.
분산 마이크로 서비스 환경에서 중앙 집중식 인증 서버와 데이터베이스에 의존하지 않는 쉬운 인증 및 인가 방법을 제공합니다.
개별 마이크로 서비스에는 토큰 검증과 검증에 필요한 비밀 키를 처리하기위한 미들웨어가 필요합니다. 검증은 서명 및 클레임과 같은 몇 가지 매개 변수를 검사하는 것과 토큰이 만료되는 경우로 구성됩니다.
토큰이 올바르게 서명되었는지 확인하는 것은 CPU 사이클을 필요로하며 IO 또는 네트워크 액세스가 필요하지 않으며 최신 웹 서버 하드웨어에서 확장하기가 쉽습니다.

    - JSON 웹 토큰의 사용을 권장하는 몇 가지 이유는 다음과 같다.
    ```
    URL 파라미터와 헤더로 사용
    수평 스케일이 용이
    디버깅 및 관리가 용이
    트래픽 대한 부담이 낮음
    REST 서비스로 제공 가능
    내장된 만료
    독립적인 JWT
    ```
- ### JWT 단점
    - 토큰은 클라이언트에 저장되어 데이터베이스에서 사용자 정보를 조작하더라도 토큰에 직접 적용할 수 없습니다.
    - 더 많은 필드가 추가되면 토큰이 커질 수 있습니다.
    - 비상태 애플리케이션에서 토큰은 거의 모든 요청에 대해 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있습니다.
### 다음같은 상황에서 JWT가 유용하게 사용 될 수 있습니다.
- 회원 인증: JWT 를 사용하는 가장 흔한 시나리오 입니다. 사용자가 로그인을 하면, 서버는 사용자의 정보를 기반으로한 토큰을 발급합니다.
그 후, 사용자가 서버에 요청을 할 때 마다 JWT를 포함하여 전달합니다. 서버는 클라이언트에서 요청을 받을때 마다, 해당 토큰이 유효하고 인증됐는지 검증을 하고, 사용자가 요청한 작업에 권한이 있는지 확인하여 작업을 처리합니다.
서버에서는 사용자에 대한 세션을 유지 할 필요가 없습니다. 즉 사용자가 로그인되어있는지 안되어있는지 신경 쓸 필요가 없고, 사용자가 요청을 했을때 토큰만 확인하면 되므로 세션 관리가 필요 없어서 서버 자원과 비용을 절감할 수 있습니다.
- 정보 교류: JWT는 두 개체 사이에서 안정성있게 정보를 교환하기에 좋은 방법입니다. 그 이유는, 정보가 서명이 되어있기 때문에 정보를 보낸이가 바뀌진 않았는지, 또 정보가 도중에 조작되지는 않았는지 검증할 수 있습니다.
### JWT 해석 사이트
- JWTio Link : [JWT.io][JWTio Link]

[JWTio Link]: https://jwt.io/ "Go jwt.io"
![jwt.io img](/img/jwtio.png "jwt.io img")
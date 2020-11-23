# REST, RESTful

## REST
> "Representational State Transfer"

- 자원을 이름으로 구분해 자원의 상태를 주고받는 것을 말한다. 어떠한 방식을 가지고 주고받겠다는 약속이라는 의미. Rest는 기본적으로 웹의 기존 기술과 [HTTP 프로토콜](https://joshua1988.github.io/web-development/http-part1/)을 그대로 사용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일이라고 한다.

### REST의 특징
- **Server-Client:** 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트(FE)-서버(BE)의 각 파트가 독립적으로 개선될 수 있도록 해준다.
- **Stateless (무상태)**: 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다
- **Cacheable (캐시 처리 가능):** WWW에서와 같이 클라이언트는 응답을 [캐싱](https://goddaehee.tistory.com/171)할 수 있어야 한다.
- **Layered System (계층화):** 클라이언트(FE)는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다. 중간 서버는 load balancing 기능이나 공유 캐시 기능을 제공하여 시스템 규모 확장서을 항상시키는 데 유용하다.

### REST의 구성
- Resource(자원): URI는 정보의 자원을 표연해야 한다.
- Verb(행위): HTTP Method
- 
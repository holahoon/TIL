# Front-end Common Sense Knowledge
면접때 유용하게 쓰일수 도 있다.

## Categories

  - [브라우저 동작 원리](#브라우저-동작-원리)
  - [Document Object Model(DOM)](#document-object-modeldom)
  - [CORS](#cors)

## 브라우저 동작 원리

브라우저가 하나의 화면을 그려내는 과정을 **중요 렌더링 경로(Critical Rendering Path)** 라고 부른다. 사용자가 브라우저 주소창에 url을 치게되면 브라우저는 해당 서버에 요청(request)을 보내게된다. 서버에서는 응답(response)으로 HTML 데이터를 준다(요청에 따라 pdf 같은 파일을 줄 때도 있다). 이 HTML 데이터를 실제 우리가 보는 화면으로 그리기까지 브라우저는 아래와 같은 단계를 밟는다.

1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다.
2. HTML을 파싱한 결과로 DOM Tree를 만든다.
3. 파싱하는 중 CSS 파일 링크를 만나면 CSS 파일을 요청해서 받아온다.
4. CSS 파일을 읽어서 CSSOM(CSS Object Model)을 만든다.
5. DOM Tree와 CSSOM이 모두 만들어지면 이 둘을 사용해 Render Tree를 만든다.
6. Render Tree에 있는 각각의 노드들이 화면의 어디에 어떻게 위치할 지를 계산하는 Layout과정을 거쳐서,
7. 화면에 실제 픽셀을 Paint한다.

reference: [브라우저는 웹페이지를 어떻게](https://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)

## Document Object Model(DOM)

문서 객체 모델(DOM)은 XML 이나 HTML 문서에 접근하기 위한 일종의 인터페이스(상호작용)다.
자바스크립트로 HTML 문서와 요소(element)에 접근하기 위하여 DOM이 사용된다. 간단하게 얘기하면 HTML같은 문서(Document)에 `<html>` `<body>`와 같은 엘리먼트들을 자바스크립트가 이용할수 있게 객체(Object)로 만들어 주는것을 **문서 객체** 라고 한다 (makes total sense =)). 그래서 바로 문서 객체를 **"인식하는 방식"** 인것이다.
> The Document Object Model (DOM) is a cross-platform and language-independent interface that treats an XML or HTML document as a tree structure wherein each node is an object representing a part of the document. The DOM represents a document with a logical tree.

## CORS

Cross Origin Resource Sharing의 줄임말이다. 바로, 서로 다른 origin간에 리소를 공유하는 메커니즘이다(well, what the f does it mean?).
HTTP 요청은 cross-site HTTP Request가 가능하다. 그러나 script로 둘려쌓여 있는 스크립트에선 same origin policy를 적용받아서 불가하다. 즉, 같은 오리진이 아니면 허용할 수 없다는거다. 그래서 다른 출처의 선택한 자원에 접근할수 있는 권하는 부여하도록 브라우저에게 알려주어 cross-site HTTP request가 가능하게 해주는거다. 예를 들어 도메인 a라는 사이트가 있고, 도메일 a라는 웹 서버에서 리소스를 요청하면 정상적으로 처리가 된다. 그런데 만약 도메인 b라는 웹 서버에서 리소스를 요청 해야 될때 그게 CORS다.

reference: [CORS](https://im-developer.tistory.com/165), [CORS and its possible solution](https://velog.io/@wlsdud2194/cors)

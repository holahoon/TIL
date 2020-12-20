# Front-end Common Sense Knowledge
면접때 유용하게 쓰일수 도 있다.

## Categories

  - [브라우저 동작 원리](#브라우저-동작-원리)
  - [Document Object Model(DOM)](#document-object-modeldom)
  - [렌더링 최적화 하기](#렌더링-최적화-하기---reflow-repaint-줄이기)
  - [CORS](#cors)
  - [Proxy](#proxy)

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

## 렌더링 최적화 하기 - Reflow, Repaint 줄이기
웹 성능을 최적화 하기 위해선 reflow와 repaint를 짚고 넘어가야한다.
- Reflow: 렌더링 과정을 거친 뒤에 최종적으로 페이지가 그려진다고 해서 렌더링 과정이 다 끝난것이 아니다. 어떠한 액션이나 이벤트에 따라 html 요소의 크기나 위치등 레이아웃 수치를 수정하면 그에 영향을 받는 자식 노드나 부모 노드들을 포함하여 Layout 과정을 다시 수행하게 된다. 이렇게 되면 Render Tree와 각 요소들의 크기와 위치를 다시 계산하게 된다. 이러한 과정을 Reflow라고 한다. Reflow가 일어나는 대표적인 예는 아래와 같다.
  - 페이지 초기 렌더링 시(최초 Layout 과정)
  - 윈도우 리사이징 시 (Viewport 크기 변경시)
  - 노드 추가 또는 제거
  - 요소의 위치, 크기 변경 (left, top, margin, padding, border, width, height, 등..)
  - 폰트 변경 과(텍스트 내용) 이미지 크기 변경(크기가 다른 이미지로 변경 시)
- Repaint: Reflow만 수행되면 실제 화면에 반영되지 않는다. Render Tree를 다시 화면에 그려주는 과정이 필요합니다. 결국은 Paint 단계가 다시 수행되는 것이며 이를 Repaint 라고 한다. 하지만 무조건 Reflow가 일어나야 Repaint가 일어나는것은 아니다. background-color, visibility와 같이 레이아웃에는 영향을 주지 않는 스타일 속성이 변경되었을 때는 Reflow를 수행할 필요가 없기 때문에 Repaint만 수행하게 된다.

그렇다면 어떻게 하면 렌더링을 최적화할 수 있을까?
- 사용하지 않는 노드에선 visibility: hidden; 보단 display: none;을 사용하자
- Reflow가 일어나는 속성을 사용하는 것 보단 Repaint가 일어나는 속성을 가급적 사용하자
  - Reflow의 대표: position, width, height, border, font, overflow, ...
  - Repaint의 대표: background, color, border-style, border-radius, outline, ...
- 애니메이션이 많은 경우 absolute이나 fixed를 사용하여 주변에 영향받는 노드들을 줄일 수 있다. Fixed같이 영향을 받는 노드가 전혀 없는 경우 reflow과정이 전혀 필요없기 떄문에 repaint 연상비용만 든다. 애니메이션이 끝난경우 원상복구 시키는것도 도움된다.

reference: [렌더링](https://boxfoxs.tistory.com/408)

## Document Object Model(DOM)

문서 객체 모델(DOM)은 XML 이나 HTML 문서에 접근하기 위한 일종의 인터페이스(상호작용)다.
자바스크립트로 HTML 문서와 요소(element)에 접근하기 위하여 DOM이 사용된다. 간단하게 얘기하면 HTML같은 문서(Document)에 `<html>` `<body>`와 같은 엘리먼트들을 자바스크립트가 이용할수 있게 객체(Object)로 만들어 주는것을 **문서 객체** 라고 한다 (makes total sense =)). 그래서 바로 문서 객체를 **"인식하는 방식"** 인것이다.
> The Document Object Model (DOM) is a cross-platform and language-independent interface that treats an XML or HTML document as a tree structure wherein each node is an object representing a part of the document. The DOM represents a document with a logical tree.

## CORS
> Cross Origin Resource Sharing

CORS는 추가 HTTP 헤더를 사용해서 한 출처에서 실행중인 어플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에게 알려주는 체제다. 즉, 브랴우저에서 기본적으로 API를 요청 할 때에는 브라우저의 현재 주소와 API의 주소의 도메인이 일치해야만 데이터를 접근할 수 있다. 그래서 만약 다른 도메인에서 API를 요청해서 사용할 수 있게 해주려면 CORS 설정이 필요하다.

reference: [CORS MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

## Proxy

만약 실제 서비스를 개발할때 서버의 API를 요청을 해야하는데 기본적으로 localhost:3000 에서 들어오는 것이 차단된다. 그래서 서버 쪽에 해당 도메인을 허용하도록 구현해야 한다. 보통 백엔드 개발자가 따로 있으면 백엔드 개발자가 한다. 그러나 굳이 그럴 필요가 없다. 웹팩 개발서버에서 제공하는 Proxy라는 기능이 있기 때문이다.

reference: [Velopert CORS](https://react.vlpt.us/redux-middleware/09-cors-and-proxy.html)
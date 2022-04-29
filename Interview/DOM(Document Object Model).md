# DOM (Document Object Model : 문서 객체 모델)

<details>
<summary>요약</summary>
<div markdown="1">

### DOM
  - HTML 문서에 대한 인터페이스
  
## DOM과 HTML의 차이점
### DOM은 HTML 문서로부터 생성되지만 항상 동일하지 않다.
- HTML : 화면에 보이고자 하는 모양과 구조를 문서로 만든 것, 단순 텍스트로 구성되어 있음(최초에 화면을 그릴 때 사용하는 설계도)
- DOM : HTML 문서의 내용과 구조가 객체 모델로 변화되어 다양한 프로그램에서 사용될 수 있음(설계도를 이용하여 실제로 화면에 나타나는 인터페이스)

</div>
</details>

## DOM
- XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스
- 문서 내의 모든 요소를 정의하고, 각각의 요소에 접근하는 방법을 제공

## DOM의 생성 방식
- 원본 HTML 문서의 객체 기반 표현 방식
- DOM의 개체 구조는 노드 트리로 표현

## DOM과 HTML의 차이점
### DOM은 HTML 문서로부터 생성되지만 항상 동일하지 않다.
- HTML : 화면에 보이고자 하는 모양과 구조를 문서로 만든 것, 단순 텍스트로 구성되어 있음(최초에 화면을 그릴 때 사용하는 설계도)
- DOM : HTML 문서의 내용과 구조가 객체 모델로 변화되어 다양한 프로그램에서 사용될 수 있음(설계도를 이용하여 실제로 화면에 나타나는 인터페이스)

### DOM은 브라우저에서 보이는 것이 아니다.
- 브라우저 뷰 포트에 보이는 것은 Render 트리 (DOM + CSSOM)
- Render 트리는 오직 렌더링 되는 요소만 관련이 있고 스크린에 그려지는 것으로 구성되어 시각적으로 보이지 않는 요소는 제외됨
(DOM은 보이지 않는 요소를 포함, Render 트리는 제외)

### DOM은 개발 도구에서 보이는 것이 아니다.
- 개발도구의 요소 검사기는 DOM과 가장 가까운 근사치를 제공하지만, 개발도구의 요소 검사기는 DOM에 없는 추가적인 정보를 포함한다.

### 참고
- [DOM의 개념](http://www.tcpschool.com/javascript/js_dom_concept)
- [DOM 이란? (+ 웹 페이지가 만들어지는 방법)](https://usefultoknow.tistory.com/entry/DOM-%EC%9D%B4%EB%9E%80-%EC%9B%B9-%ED%8E%98%EC%9D%B4%EC%A7%80%EA%B0%80-%EB%A7%8C%EB%93%A4%EC%96%B4%EC%A7%80%EB%8A%94-%EB%B0%A9%EB%B2%95)
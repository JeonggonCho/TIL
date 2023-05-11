# HTML 소개

## - 목차
1. [HTML 소개](#1-html-소개)
    - [HTML](#1-html)
        - [HyperText](#--hypertext)
        - [Markup Language](#--markup-language)
2. [HTML의 구조](#2-html의-구조)
    - [HTML Element(요소)](#1-html-element요소)
    - [HTML Attributes(속성)](#2-html-attributes속성)
    - [HTML 문서의 구조](#3-html-문서의-구조)
3. [Text 구조](#3-text-구조)
    - [HTML Text 구조](#1-html-text-구조)
    - [대표적인 HTML Text 구조](#2-대표적인-html-text-구조)

---

## (1) HTML 소개

### **1) HTML**

- HyperText Markup Language의 약자
- 웹페이지의 의미와 구조를 정의하는 언어

<br>

### - HyperText

- 웹 페이지를 다른 페이지로 `연결`하는 링크
  - HyperLink
- `참조`를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
- 기존의 선형적인 텍스트가 아닌 `비 선형적`으로 이루어진 텍스트

<br>

### - Markup Language

- 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어
  - ex) HTML, Markdown, ...


---

## (2) HTML의 구조

### **1) HTML Element(요소)**

```html
<!--ex)-->

<p>Hello, World!!</p>

<!--<p>는 Opening Tag(여는 태그)-->
<!--</p>는 Closing Tag(닫는 태그)-->
<!--태그 안의 "Hello, World!!" 부분은 content(내용)-->
<!--여는 태그부터 닫는 태그까지 모두를 element(요소)-->
```

- 하나의 `요소`는 `여는 태그`와 `닫는 태그` 그리고 그 안의 `내용`으로 구성됨
- `닫는 태그`는 태그 이름 앞에 `슬래시('/')가 포함`되며 닫는 태그가 없는 태그도 존재

<br>

### **2) HTML Attributes(속성)**

```html
<!--ex)-->

<p class="editor-note">Hello, World!!!</p>

<!--class="editor-note" 부분이 속성-->
```

- 규칙
  - 요소 이름 다음 바로 오는 속성은 요소 이름과 속성 사이에 공백이 있어야 함
  - 하나 이상의 속성들이 있는 경우, 속성 사이에 공백으로 구분
  - 속성 값은 열고 닫는 `따옴표("")`로 감싸야 함
- 목적
  - 나타내고 싶지 않지만 추가적 기능, 내용을 담고 싶을 때 사용
  - `CSS`가 해당 요소를 `선택하기 위한 값`으로 활용

<br>

### **3) HTML 문서의 구조**

```html
<!--기본적인 HTML 문서 구조-->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My page</title>
</head>
<body>
  <p>This is my page</p>
</body>
</html>
```

- `<!DOCTYPE html>` : 해당 문서가 html의 문서라는 것을 표시
- `<html></html>` : 전체 페이지의 콘텐츠를 포함
- `<title></title>` : 브라우저 탭 및 즐겨찾기 시, 표시되는 제목으로 사용
- `<head></head>` : HTML 문서에 관련된 설명, 설정 등(사용자에게는 보이지 않음)
- `<body></body>` : 페이지에 표시되는 모든 컨텐츠


---

## (3) Text 구조

### **1) HTML Text 구조**

- HTML의 주요 목적 중 하나는 텍스트 구조와 의미를 제공하는 것

```html
<h1>Main Heading</h1>
```

- 예를 들어 `<h1>` 태그의 경우, 단순히 텍스트를 크게 만든다의 의미를 넘어 `해당 문서의 최상위 제목`이라는 의미를 부여

<br>

### **2) 대표적인 HTML Text 구조**

- `Heading & Paragraphs` : h1~6, p
- `List` : ol, ul, li
  - Unordered
  - Ordered
- `Emphasis & Important` : em, strong

```html
<!--HTML Text 구조 예시 (각자 위계와 역할을 부여)-->

<body>
    <h1>Main Heading</h1>
    <h2>Sub Heading</h2>
    <p>This is my page</p>
    <p>This is <em>emphasis</em></p>
    <p>Hi <strong>my name</strong> is Jeonggon</p>
    <ol>
      <li>html</li>
      <li>css</li>
      <li>javascript</li>
    </ol>
</body>
```
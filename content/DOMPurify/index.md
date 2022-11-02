---
emoji: 💧
title: Error/ DOM에 렌더링 될 때 target="\_blank" 속성이 사라져요! , Renderer remove my attribute!
date: '2021-11-01 11:09:10'
author: 양다은
tags: DOMPurify, a태그속성사라짐
categories: Error
---

# Intro...

 <br />
 회사에서 열심히 주어진 핫픽스를 하던 와중, 
 <br />
 낯서디 낯선 마크다운 렌더러를 만지게 되는데.
 
 <br />
 
 임무는 외부링크로 연결되는 a태그를 새로운 창에서 열리게 하는 것이었다. <br />
 회사는 마크다운으로 회사 랜딩페이지의 글들을 렌더링하고 있었다. <br />
 렌더링 된 html에 `target="\_blank"` 속성을 넣고 팀장님이 이건 근본적인 해결이 아니라고 코멘트 해주셨다. <br />

마크다운 문법을 html로 렌더링 시키는 렌더러에서 작업을 수행해야 했는데, <br />
묘하게도 `target="\_blank"`속성만 DOM에 나타나지 않았다. <br />
묘한일이다 묘한일이야... 뭔가 이상한데, 하며 입사 2주차였던 나는 선뜻 물어보지도 못하고 <br />
머리를 꽁꽁 싸매고 있었다.

이유는 뭐였을까?

 <br />

# 원인

범인은 [DOM Purify](https://github.com/cure53/DOMPurify) 였다.

> Dom Purify는 악성 스크립트를 삽입하는 XSS 공격을 막기 위한 라이브러리다.  
>  깃허브를 보면 더러운 HTML을 깨끗한 HTML로 바꿔준다고 나와 있다.

DOM Purify가 html을 살균하면서 보안에 취약한 `target="\_blank"`을 더티코드로 인식해 제거해버린 것이다.

<br />

### `target="\_blank"` 왜 보안에 취약할까?

- 해당 속성들을 살펴보면서 알아보자

> `<a href="https://www.google.com/" target="_blank" rel="noopener noreferrer">` <br /><br />
> 이 둘은 보통 짝으로 같이 다니게 되는데, `rel` 속성은 낯설다.<br />
> `rel`은 연결된 리소스와 현재 문서 간의 관계를 정의하는 속성이다. <br />
> 리소스가 연결되는 `<link>`, `<a>`, `<area>`, `<form>`에서 사용하는데 각 태그마다 올 수 있는 값이 다르다.

  <br />
  
  - noopener
    - `<a>`, `<area>`, `<form>`에서 쓸 수 있다.
    - 새롭게 열린 브라우징 컨텍스트에 해당 리소스를 연 문서에 대한 접근 권한을 부여하지 않고 대상 리소스를 탐색하도록 브라우저에 지시한다.
    - 새롭게 열린 창에 window.opener 속성을 부여하지 않는다. (null을 반환)
    - 즉, 새롭게 열린 창이 원본의 창을 참조하지 못하게 막아준다.
    - 완전히 신뢰할 수 없는 링크와 연결 할 때 꼭 써야 한다.
    - [이 문서](https://mathiasbynens.github.io/rel-noopener/)에서 아주 자세히 설명해주고 있다.

- noreferrer
- 다른 페이지를 탐색할 때, 페이지의 주소나 다른 값들을 HTTP 헤더를 통해 보내는 것을 막는다.

<br />

# 문제를 해결한 방법

- 바로 DOMPutify 공식홈페이지에 들어갔다. 역시 반복해서 제기됐던 문제였는지, 커스텀 훅을 제공하고 있었다.

[링크](https://fossies.org/linux/DOMPurify/demos/README.md#:~:text=Hook%20to%20open%20all%20links%20in%20a%20new%20window)

```
// Add a hook to make all links open a new window
DOMPurify.addHook('afterSanitizeAttributes', function (node) {
  // set all elements owning target to target=_blank
  if ('target' in node) {
    node.setAttribute('target', '_blank');
  }
  // set non-HTML/MathML links to xlink:show=new
  if (
    !node.hasAttribute('target') &&
    (node.hasAttribute('xlink:href') || node.hasAttribute('href'))
  ) {
    node.setAttribute('xlink:show', 'new');
  }
});

// Clean HTML string and write into our DIV
var clean = DOMPurify.sanitize(dirty);
```

- 간단히 해결되었다...!

<br />

# outro...

꼬박 이틀을 바친...

좀 헤매게 된다면 정리해서 바로 사수에게 물어보는 것도 좋을듯 하다.
그치만 정말 좋은 경험이었다.

'못할 것 같다'라고 생각했었는데 결국엔 해냈을 때, '어떤 일도 할 수 있다'라는 데이터가 축적되면 더 재밌게 개발할 수 있지 않을까?

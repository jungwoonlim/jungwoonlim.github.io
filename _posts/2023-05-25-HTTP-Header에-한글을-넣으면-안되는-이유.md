---
title: HTTP Header에 한글을 넣으면 안되는 이유
date: 2023-05-25 00:00:00 -0500
categories: ['study']
tags: ['Javascript', 'study']
---

## 서론

프로젝트를 진행하다가 API 호출이 모두 막혀버리는 현상이 발생했었습니다.
원인은 HTTP Header에 한글이 들어가서 HTTP Request가 동작하지 않았던 것이었고, 한글이 들어가지 않게 변경하여 문제를 해결해냈습니다.
하지만 왜? 왜 한글이 들어가면 안되는지, 그 이유에 대해서 명확하게 알고 넘어가고자 이 글을 작성하게 되었습니다.

## HTTP Header란?

HTTP Header에 대한 정의를 살펴보면 다음과 같습니다.

> HTTP 헤더는 클라이언트와 서버가 요청 및 응답에서 부가적인 정보를 보내고 받을 수 있도록 해주는 문자열 목록입니다.

**참고**

- [wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- [MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)

## HTTP Header의 컨벤션

HTTP Header를 다룰 때는 몇 가지 규칙과 표준을 따릅니다.

- 헤더 필드: 각 헤더는 필드 이름과 값으로 구성되며 콜론 ":"으로 구분됩니다.
- 대소문자 구분 없음: 헤더 필드 이름은 대소문자를 구분하지 않습니다. 하지만 일반적으로 소문자를 사용하는 것이 규칙입니다.
- 하이픈(-) vs 언더스코어(_): 헤더 필드 이름이 여러 단어로 구성된 경우, 각 단어를 구분하기 위해 하이픈(-)을 사용하는 것이 관례입니다.
- 여러 헤더 값: 일부 헤더는 여러 값을 가질 수 있습니다. 이 경우 쉼표로 값을 구분합니다.
- 특수 헤더: 특정한 용도로 널리 사용되는 헤더입니다. 주로 사용되는 것은 다음과 같습니다.
  - 'Content-Type': 메시지 본문의 미디어 유형을 지정
  - 'Content-Length': 메시지 본문의 크기를 바이트 단위로 나타냄
  - 'User-Agent': 요청을 보내는 클라이언트 소프트웨어나 사용자 에이전트를 식별
  - 'Authorization': 클라이언트를 인증하기 위한 자격 증명을 포함합니다.
  - 더 많은 자료는 [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)을 참고하시길 바랍니다.
- 사용자 정의 헤더: 표준 헤더 이외에도 응용 프로그램에 특정한 사용자 정의 헤더를 정의할 수 있습니다.
  - 예전에는 'X-'를 붙이는 것이 관례였다고 하는데, 2012년 6월 비표준 필드가 표준이 되었을때 불편함을 유발한다는 이유로 폐기되었습니다. 이제는 그냥 이름만 잘 지으면 됩니다.
  - [참고: RFC 6648](https://datatracker.ietf.org/doc/html/rfc6648)

## Custom Header(사용자 정의 헤더)에 한글을 사용할 수 있는가?

다음 문서 [RTC-5987](https://datatracker.ietf.org/doc/html/rfc5987)의 문서의 첫 줄을 보면, 다음과 같은 문장이 있습니다.

> By default, message header field parameters in Hypertext Transfer Protocol (HTTP) messages cannot carry characters outside the ISO-8859-1 character set.

ISO-8859-1(Latin-1) 이외의 것은 사용할 수 없다고 나와 있습니다.

<img src="https://www.charset.org/img/charsets/iso-8859-1.gif" alt="" width="600" priority />

즉, 한글은 표준 규격(ISO-8859-1)에 맞지 않기 때문에 한글을 사용할 수 없는 것입니다.

그렇다면 앞으로도 표준 규격에 맞지 않기 때문에 한글을 넣지 못하게 되는 것일까요?

## HTTP Header에 한글을 사용하기 위한 방법

HTTP Header에 한글을 직접 사용하는 것은 표준 규격이 바뀌기 전까지는 불가능합니다.
하지만 우회해서 사용하는 방법이 있습니다. 문서 [RTC-3986](https://datatracker.ietf.org/doc/html/rfc3986#section-2.1)를 확인해보면 다음과 같은 문구가 있습니다.

> A percent-encoding mechanism is used to represent a data octet in a component when that octet's corresponding character is outside the allowed set or is being used as a delimiter of, or within, the component.

즉, ISO-8859-1 이외의 것에 접근하기 위해서 인코딩을 진행해야 합니다. 문서에서는 퍼센트 인코딩을 이야기했는데, 프론트엔드에서는 간단하게 [encodeURI()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)와 [encodeURIComponent()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)를 사용해서 인코딩한다고 생각하면 됩니다. 물론 받아온 뒤에 디코딩을 해줘야 합니다.

## 마치며

이상으로 왜 Header에 한글을 넣으면 안되는지에 대해서 알아보았습니다. 만약 반드시 넣어야 한다면 어떻게 사용해야 하는지도 확인해보았는데, 인코딩 디코딩을 항상 해야하는 비용을 생각하니... 앞으로는 한글이 들어가지 않도록 신경써야 겠습니다.

## 참고 자료

- [wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- [MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)
- [HTTP Header](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)
- [HTTP Header에 한글 넣으면 발생하는 오류](https://imksh.com/101)
- [HTTP Header User-Agent](https://jammdev.tistory.com/m/171)
- [RTC-3986](https://datatracker.ietf.org/doc/html/rfc3986#section-2.1)
- [RTC-5987](https://datatracker.ietf.org/doc/html/rfc5987)

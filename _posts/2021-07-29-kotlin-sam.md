---
title: Kotlin SAM
author: leejaeho
date: 2021-07-29 00:19:00 +0900
categories: [Kotlin]
tags: [functional interface, SAM]
toc: false
---

android를 kotlin으로 개발하다보면 다음과 같은 코드를 자주 작성하게 된다.

```kotlin
view.setOnClickListener {
    ...
}
```

코틀린 특성상 메소드의 마지막 파라미터가 람다식인 경우 () 뒤에 `{ ... }` 블록을 작성할 수 있다.

<br>

하지만 view.setOnClickListener 메소드를 확인해보면 다음과 같이 정의되어 있다.

```java
public void setOnClickListener(@Nullable OnClickListener l) {
    ...
}
```

여기서 OnClickListener 는
```java
public interface OnClickListener {
    void onClick(View v);
}
```

setOnClickListener의 파라미터인 OnClickListener 람다식이 아니지만,  람다식처럼 취급되고 있다. <br>
이것이 가능한 이유는 자바8부터 지원한 <u>SAM(single abstract method)</u> 라는 특성 때문인데, <br>
interface가 오직 하나의 abstract method 를 가지고 있는 경우 람다로 wrapping 해주는 특성이다. <br>
코틀린 또한 SAM을 지원하기에 자바의 OnClickListener 를 람다로 wrapping 해준 것.

<br>

문제는 코틀린으로 OnClickListener을 똑같이 정의할 경우 <br>
SAM conversion이 적용되지 않는다. <br>
왜일까 ?

<https://stackoverflow.com/questions/55116769/interface-as-functions-in-kotlin/55117201>
<br> 스택오버플로를 참고해보면, <br> **kotlin에는 적절한 함수 유형이 있으므로 함수를 자동 변환할 필요가 없어 지원하지 않는다고 한다.** <br>
> 해당 페이지 안에는 참조 링크로 공식 문서인 <https://kotlinlang.org/docs/java-interop.html#sam-conversions><br> 링크를 주는데,
현재는 코틀린이 업데이트 되면서 공식 문서에서 해당 문장이 빠진 것으로 보인다.

<br><br>

그럼 코틀린으로 작성된 것에 대해서는 SAM 변환을 지원하지 않는것인가? <br>
jetbrains쪽 이슈를 보면 <https://youtrack.jetbrains.com/issue/KT-7770?source=post_page---------------------------> <br>
SAM 지원에 대해 열띤 얘기를 나눈 것을 확인할 수 있다. <br>
> `object:` 익명 클래스 작성은 코틀린 답지 못하다고 다들 느끼고 있었던 것으로 생각된다. <br>

`Denis Zharkov` 가 20 Jun 2018 15:28 에 추가한 코멘트를 확인해보면, ( 1.3 출시 전으로 보임  ) <br>
1.0 부터 해당 이슈를 인지하고 있었고, 지원을 준비하고 있었지만 여러 복잡한 이슈들을 해결하다보니 시간이 걸렸다고 한다.
<br> 이 때 맛보기로 소개한 아래의 코드.
```kotlin
fun interface MyRunnable {
  fun run()
}
```

interface 앞에 fun 키워드를 붙인 것으로 SAM conversion을 지원할 것이라고 한다. <br>
결국 코틀린 1.4에서 해당 기능이 적용이 되었다. <br>
<https://blog.jetbrains.com/ko/kotlin/2020/07/kotlin-1-4-m3-ko/#fun-interfaces-in-stdlib> <br>

이건 현재 가이드 <br>
<https://kotlinlang.org/docs/fun-interfaces.html>

> 보통은 개발하다보면 모양이 변형되기 마련인데 2018년 6월에 언급된 아이디어가 2020년 7월에 그대로 공개된 부분이 대단하기도 하다.
> fun interface 키워드가 워낙 찰떡이라 그런 것 같기도.

말그대로 fun interface ...😅

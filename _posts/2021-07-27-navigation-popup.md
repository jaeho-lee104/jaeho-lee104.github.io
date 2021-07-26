---
title: android navigation 사용시 백스택 추가하지 않고 이동
author: leejaeho
date: 2021-07-27 01:31:00 +0900
categories: [Android]
tags: [Navigation]
toc: false
---

navigation 을 통해 이동시 몇 가지 옵션을 통해 백스택 설정을 할 수 있다.
<https://developer.android.com/reference/androidx/navigation/NavOptions.Builder#summary>

setPopUpTo 를 보면,
동작은 navigating 전에 일어나고,<br>
지정된 destination 을 찾을 때까지 백스택에서 일치하지 않는 모든 destination 을 팝한다고 한다. <br>
inclusive 는 true일 경우 지정한 destination까지 pop.

따라서 A -> B로 갈 때, A를 popUpTo destination으로 지정하고 inclusive true 로 설정하면 된다.

<br>

------

Splash 화면과 같은 일회성 화면에 popUpTo를 적용한 사례다.

#### nav_graph.xml 에서 코드로 설정하거나
![img](/assets/img/post/nav_1.png)
_nav_graph.xml_

#### nav_graph design panel 에서 설정 가능하다.
![img](/assets/img/post/nav_2.png)
_nav_graph.xml panel_

#### 실제 navigate() 시점에 NavOptions 를 적용해도 된다.


<br>

(+) 번외로, navigating시 fade in/out 같이 간단한 전환 애니메이션을 적용하고자 하는데, <br>
라이브러리에서 제공해주는 anim/nav_default_enter_anim 의 `android:duration="@integer/config_navAnimTime"` 이 원하는 값이 아니라서 <br>
다시 anim resource 파일을 새로 만드는 경우가 있다. <br>

이 때는 `config_navAnimTime` 과 동일한 패키지 경로로 현재 프로젝트에 똑같이 만들어 주고 ( res > values > values.xml )
`<integer name="config_navAnimTime">350</integer>` 이런식으로 리소스 오버라이딩하면 된다.



---
title: Android TV 에서 charles proxy 설정
author: leejaeho
date: 2021-08-08 01:42:00 +0900
categories: [Android]
tags: [Android TV, Charles]
toc: false
---

[Charles](https://www.charlesproxy.com/)를 이용하여 네트워크 트래픽을 보려면 <br>
현재 연결되어 있는 네트워크에 proxy 설정을 해주면 된다. <br>
https 와 같이 ssl proxying 이 필요한 경우에는 Charles 인증서를 설치해주어야 한다. <br><br>

인증서 별도 설치가 필요한 이유에 대해 간단히 설명하면, <br>
Charles 는 Client <-> Charles (proxy) <-> Server 의 형태로 통신하며, 중간자 역할을 한다. <br>
이 때, Client는 Server의 인증서를 보는 대신 Charles가 동적으로 생성하고, <br>
자체 루트 인증서로 서명한(Charles CA Certificate) 인증서를 본다. <br>
Charles는 서버의 인증서를 받고, Client는 Charles의 인증서를 받는다. <br>
Client는 Charles의 인증서를 신뢰할 수 없기에<br> 해당 인증서를 신뢰할 수 있는 인증서로 취급하도록 설정을 해주어야 한다.<br>
따라서 별도 설치가 필요하다. <br><br>

Android나 iOS 의 경우 <http://chls.pro/ssl> 에 들어가서 인증서를 설치할 수 있다. <br>
하지만 Android TV의 경우 인증서를 설치하거나 제어할 수 있는 별도의 UI를 제공하지 않는다. <br>
이 때 Android TV에 Charles의 인증서를 신뢰할 수 있는 인증서로 취급하려면,
다음의 과정을 수행하면 된다.

1. Charles > Help > SSL Proxying > Save Charles Root Certificate... 클릭해서 `charles_ssl_proxying_certificate.pem` 파일을 저장한다.
2. Android Project 에 res > raw 하위에 `charles_ssl_proxying_certificate.pem` 파일을 위치시킨다.
  - res 하위에 raw 디렉토리가 없는 경우 만든다.
3. res > xml 하위에 `network_security_config.xml` 파일을 만든다.
  - res 하위에 xml 디렉토리가 없는 경우 만든다.
4. `network_security_config.xml` 의 내용은 다음 내용으로 채운다.
     -  ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <network-security-config>
            <debug-overrides>
                <trust-anchors>
                    <certificates src="system" />
                    <certificates src="@raw/charles_ssl_proxying_certificate" />
                    <certificates src="user" />
                </trust-anchors>
            </debug-overrides>
        </network-security-config>
        ```
5. AndroidManifest.xml 에서 다음 내용을 추가한다.
    -   ```xml
        <application
            android:networkSecurityConfig="@xml/network_security_config
            ...
        ```


`network_security_config.xml` 에서 certificates 태그와 함께 추가하면, 신뢰할 수 있는 CA로 추가할 수 있다. <br>
[참고: https://developer.android.com/training/articles/security-config?hl=ko#TrustingAdditionalCas](https://developer.android.com/training/articles/security-config?hl=ko#TrustingAdditionalCas) <br>

이와 같은 설정 후 확인하고자 하는 호스트를 enable ssl proxying 목록에 추가하면 트래픽을 확인할 수 있다.

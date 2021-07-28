---
title: fresco 로컬 asset 파일 읽기
author: leejaeho
date: 2021-07-28 15:56:00 +0900
categories: [Android]
tags: [Fresco, Assets, WebP]
toc: false
---

webp 또는 그 외 다른 이미지 파일들을 app assets 폴더에 저장하고 사용하는 케이스가 있다. <br>
보통 fresco 는 웹 상의 이미지를 로드하기에 편리해서 사용했는데 로컬에 있는 파일을 읽어들일 때는 어떻게 하면 될까. <br>
단순히 절대경로나 상대경로를 사용해서는 안된다.

fresco 문서에 보면 지원하는 uri 타입들에 대해 다음과 같이 정의해두었다.

<https://frescolib.org/docs/supported-uris.html>




type | scheme | fetch method used
-- | -- | --
File on network | http://, https:// | HttpURLConnection or network layer
File on device |	file:// |	FileInputStream
Content provider |	content://	 | ContentResolver
Asset in app |	asset:// |	AssetManager
Resource in app	 | res:// as in res:///12345	 | Resources.openRawResource
Data in URI |	data:mime/type;base64, |	Following data URI spec (UTF-8 only)


asset의 경우 uri의 스킴패턴이 asset:// 으로 시작하면 된다. <br>
만약 저장 경로가 `app > src > main > assets > sample.webp ` 이라면 <br>
경로는 `asset:///sample.webp` 로 사용하면 된다.

slash 가 왜 3개인가? 는 <br>
file URL 의 표준에 정의되어 있는데 ( <https://www.ietf.org/rfc/rfc1738.txt > ) <br>
3.10 내용을 보면 host가 localhost인 경우 empty string을 취급할 수 있다고 나와있다.

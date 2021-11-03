---
layout: post
title:  "로티(lottie) json export 시 설정할 것들"
subtitle: "AE에서 작업한 컴포지션(모션)파일을 json 파일로 변환을 하려면 몇 가지 설정을 바꿔야할 필요가 있습니다."
date:   2021-11-03 13:14:00
categories: [tool]
---

# 로티(lottie) json export 시 설정할 것들

작업 과정:
[어도비 애프터 이펙트(AE)](https://www.adobe.com/kr/products/aftereffects.html)로 모션 작업 → [bodymovin 플러그인](https://exchange.adobe.com/creativecloud.details.12557.bodymovin.html)을 통해 컴포지션을 json으로 변환합니다.

# 1. AE 설정

`After Effects > 환경 설정 (Preference) > 스크립팅 및 표현식 (Scripting and Expressions)`으로 들어가 

'스크립트를 통한 파일 쓰기 및 네트워크 엑세스 허용(allow scripts to write files and access network)' 을 체크합니다.

![Untitled](%E1%84%85%E1%85%A9%E1%84%90%E1%85%B5(lottie)%20json%20export%20%E1%84%89%E1%85%B5%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AF%20%E1%84%80%E1%85%A5%E1%86%BA%E1%84%83%E1%85%B3%E1%86%AF%2058d99d109c244fd3972a521171c9cbaf/Untitled.png)

![Untitled](%E1%84%85%E1%85%A9%E1%84%90%E1%85%B5(lottie)%20json%20export%20%E1%84%89%E1%85%B5%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AF%20%E1%84%80%E1%85%A5%E1%86%BA%E1%84%83%E1%85%B3%E1%86%AF%2058d99d109c244fd3972a521171c9cbaf/Untitled%201.png)

# 2. bodymovin 설정

이미지를 사용한 로티의 경우 이미지 파일을 임베딩해야합니다.

`창(Window) > 확장명(Plugin) > bodymovin`을 선택해 bodymovin을 엽니다.

json으로 변환하고자 하는 컴포지션 이름 옆에 톱니바퀴 모양 설정을 들어가 다음 그림과 같이 'Include in json'을 체크합니다.

![Untitled](%E1%84%85%E1%85%A9%E1%84%90%E1%85%B5(lottie)%20json%20export%20%E1%84%89%E1%85%B5%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AF%20%E1%84%80%E1%85%A5%E1%86%BA%E1%84%83%E1%85%B3%E1%86%AF%2058d99d109c244fd3972a521171c9cbaf/Untitled%202.png)
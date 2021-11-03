---
layout: post
title:  "로티(lottie) json export 시 설정할 것들"
subtitle: "AE에서 작업한 컴포지션(모션)파일을 json 파일로 변환을 하려면 몇 가지 설정을 바꿔야할 필요가 있습니다. 컴포지션에 특히 이미지가 들어간 경우 이미지 임베딩을 해야지 개발이 쉬워집니다."
date:   2021-11-03 13:14:00
categories: [tools]
---

작업 과정:
[어도비 애프터 이펙트(AE)](https://www.adobe.com/kr/products/aftereffects.html)로 모션 작업 → [bodymovin 플러그인](https://exchange.adobe.com/creativecloud.details.12557.bodymovin.html)을 통해 컴포지션을 json으로 변환합니다.

# 1. AE 설정

`After Effects > 환경 설정 (Preference) > 스크립팅 및 표현식 (Scripting and Expressions)`으로 들어가 

'스크립트를 통한 파일 쓰기 및 네트워크 엑세스 허용(allow scripts to write files and access network)' 을 체크합니다.

![Untitled](https://user-images.githubusercontent.com/48819383/140011552-2c04f2e2-9979-417e-ade7-d4d1af62459a.png)

![Untitled 1](https://user-images.githubusercontent.com/48819383/140011541-e9038f94-b041-4089-9933-8d2172eb0913.png)

# 2. bodymovin 설정

이미지를 사용한 로티의 경우 이미지 파일을 임베딩해야합니다.

`창(Window) > 확장명(Plugin) > bodymovin`을 선택해 bodymovin을 엽니다.

json으로 변환하고자 하는 컴포지션 이름 옆에 톱니바퀴 모양 설정을 들어가 다음 그림과 같이 'Include in json'을 체크합니다.

![Untitled 2](https://user-images.githubusercontent.com/48819383/140011545-38e048ae-e4a8-4574-88fb-9c471ded5fee.png)
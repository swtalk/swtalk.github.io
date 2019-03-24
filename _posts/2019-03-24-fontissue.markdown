---
layout: post
title:  "Intellij font not showing"
date:   2019-03-24 23:59:00
author: viewrain
categories: Experience
---
```concordion
삽질기, 어디서든 알게된 디지털유목민 스타일의 노하우를 기록합니다.
```
## Why posting?

* `format`은 3개월에 한번씩은 해야한다는 주의지만.. 이것저것 백업할 일 때문에 미루고 미루다가 오늘 포맷!
  * IDE 셋팅을 저장을 안해놓은 ㅠㅠ 말도안되는 상황이긴 함
  
* 개발폰트를 하나씩 가지고 있을테니.. 나도 내가 늘 쓰는 bitstream vera mono 를 자연스럽게 설치 하였으나 이상하게 항상 나오던 Font가 나오지 않았다.
  * 재시작을 안해서 그러나..? 해서 IDE를 계속 재시작했으나 실패 ㅠㅠ
  * 어쩔수 없이 구글링 시작..

* 전부 IDE를 재시작 했니..? 라는 말만 계속되어 짜증이 무르 익을즈음!!
![image](/assets/issue/Fontissue.PNG)

* 오 신이시어,, 맞다. 내가 한일을 곰곰히 생각해보면 다음과 같았다.
  * 포맷 > 윈도우 정품 업데이트 > Intellij 설치 > 폰트설치
    * 번역하면 이슈의 원인은 다음과 같았다.

```
win10 빌드 1809 는 이전 글꼴을 설치방식으로 변경했어, IDE가 폰트를 못찾으면 JRE/lib/font 에 넣어도 되긴 하지만
mac 의 mojave 버전처럼 렌더링 문제가 있어!!
그러니까 마우스를 우클릭해서 '모든 사용자를 위해 설치' 로 폰트를 깔고 다시시작해
```

* 이렇게 친절할 수가.. mac의 mojave 때도 그고생을 해놓고.. 바보짓을 또하다니.. 아무튼 글꼴로 인한 짜증이 더는 있어서는 안될듯 하여 포스팅했다.

* Got it..

### Reference
* [해당 이슈 support 페이지](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360001112320-Idea-does-not-recognize-the-new-fonts-added-to-windows10-LTSC-2019-version)

---
layout: post
title:  Trustless Smart Contract Upgradability
date:   2018-11-01 00:00:00 +09:00
author: "정순형(Kevin)"
categories: ethereum SmartContract solidity
tag: [ethereum, SmartContract, solidity]
youtubeId :
slideWebId :
---
* content
{:toc}

{% include youtubePlayer.html id=page.youtubeId %}
{% include slidePlayer.html id=page.slideWebId %}

# 철학자의 데브콘4 참관기

![](https://cdn-images-1.medium.com/max/800/1*t32qdzUKoOzF4Bm4Se5pPg.jpeg)
*<center>행사장 거울 앞에서 셀피</center>*

전 세계 이더리안(Etherian)들의 축제, 2018.10.30–11.2일 체코 프라하에서 개최된 데브콘4에 다녀왔습니다. 프라하 현지에서
수많은 주제로 세션이 열렸습니다만, 모두를 다루기보다는 인상 깊었던 주제를 중심으로 간단히 참관기를 남기려 합니다.

[온더의 블로그](https://medium.com/onther-tech/tagged/devcon4)에는 모든 팀원들의 참관기가 있으니 이
내용으로는 부족하신 분들께서는 참고해 주시기 바랍니다.

*****

![](https://cdn-images-1.medium.com/max/800/1*Iop82I2K9BT_Szp0mGeibA.png)
*<center>Ali Azam의 Trustless Smart Contract Upgradability 워크아웃. 인도식 영어발음이라 약간 힘들었다 ㅠㅠ</center>*

스마트 컨트렉트의 업그레이드에 관한 2시간 짜리 실습 워크아웃이 있었습니다. 사실 큰 기대를 하지는 않았습니다. 업그레이드 가능한 스마트
컨트렉트의 경우 [Proxy패턴](https://blog.zeppelinos.org/proxy-patterns/)이라는 컨트렉트 프로그래밍
기법이 이미 널리 알려져 있고, 최근 업데이트된 [오픈제플린의
프레임워크](https://github.com/OpenZeppelin/openzeppelin-solidity)에도 반영되어 있고, [한국 최고의
오디팅팀중 하나인 해치랩스도 같은
이야기](https://medium.com/haechi-labs/prerequisites-and-backgrounds-for-making-upgradable-smart-contract-e2035b8472ad)를
여러차례 했기 때문입니다.

> 심지어 워크아웃은 단계별 실습으로 이뤄져 있었는데, 제 옆에 앉았던 저희팀의 박주형(CTO)님은 5분만에 프록시 패턴과 투표 거버넌스를 넣은
> 컨트랙트를 만들어 버리기도 했습니다. 우리가 아는 그걸로 끝인가 싶어서 나가서 다른거 들으려다가 **Trustless에 관한 아이디어 하나를
듣고는 자리에서 일어날 수 없었습니다.** 그게 뭐냐구요? 계속 읽어보세요ㅋㅋ

![](https://cdn-images-1.medium.com/max/800/1*hjUm5kBPrbmJPnqAmyxicQ.png)

워크아웃의 첫 슬라이드는 스마트 컨트렉트의 업그레이드 가능한 특성과 지난 연구들, 그리고 이 발표의 방향에 대해서 이야기를 했습니다.

“스마트 컨트렉트 업그레이드에 관해서는 많은 연구가 이뤄지고 있지만, **탈중앙화(Trustless)** 에 대한 고민은 많이 하지 않는 것
같다. 이더리움이 포킹을 통해서 탈중앙화를 지켜 냈듯이, 컨트랙트 수준에서도 비슷한 걸 할 수 있지 않을까?”

내용을 들으면서 훌륭한 접근이라고 생각했습니다. 사실 탈중앙화의 층위는 여럿으로 나눠서 생각할 수 있습니다. PoW, PoS 등 합의 층위에서의
탈중앙화를 이야기 할 수도 있고, 그 위에서 동작하는 컨트렉트 수준의 탈중앙화 이슈도 많습니다. 사실 후자의 경우는 그렇게 많은 관심이 없는것도
어느정도는 사실입니다. (이더리움 위에 있어서 프로토콜은 탈중앙화 되어 있지만, 컨트렉트를 중앙화 시켜서 관리자가 토큰을 막 찍어내는 로직을
넣는것은 별다른 논의가 없습니다^^;; 요즘에는 오히려 그게 좋다고 말하시는 분들도 있더군요)

![](https://cdn-images-1.medium.com/max/800/1*mxZ946RY_pNXlsnk8P3JyQ.png)

스마트 컨트랙트는 완벽하게(Truely) 업그레이드 가능 하지는 않지만, 방법은 있다.

![](https://cdn-images-1.medium.com/max/800/1*7dKe5kavPVDtO9QBZEgFFQ.png)

업그레이드 가능하게 만드는 이유? 개발 과정에서 버그가 나중에 발견되거나, 기능이 개선될 필요가 있으니까.

![](https://cdn-images-1.medium.com/max/800/1*je-CQRCieiWgCWdPMnzcHw.png)

업그레이드 가능한 방식으로 개발하는건 매우 실험적인 단계라고 꽤 긴시간 이야기 합니다. 조심스럽게 사용해야 하고, 완전한 이해를 바탕으로
해야한다고 강조하죠. 기능이 많아지는 만큼 필연적으로 컨트랙트의 구조가 복잡해지기 때문에, 만약 코드가 커다란 금전을 다루고 있다면 조심해서
써야 합니다.

![](https://cdn-images-1.medium.com/max/800/1*ggwH6SIak1uQStWPd1X2hw.png)

현재 쓰이고 있는 업그레이드 방법들에 대해서 이야기합니다. 크게 둘로 나눠집니다.

1.  새로운 컨트랙트 배포
1.  업그레이드를 할 수 있도록 디자인 패턴을 적용(로직과 데이터 분리, delegateCall기반의 접근)

![](https://cdn-images-1.medium.com/max/800/1*8T29WBhIvrabu3--Ty5yMA.png)
*<center>이게 앞에서 말한 Trustless Upgradability에 관한 아이디어입니다. 중요한 개념이에요.</center>*

가장 중요한 개념중에 하나입니다. 업그레이드를 위해서 많은 수의 트랜잭션이 필요하다면 그건 그만큼 **탈중앙화(Trustless)** 되어 있는
것이고, 적은 수의 트랜잭션으로도 업그레이드 가능하다면 그건 그만큼 **중앙화(Trusted)** 되어 있는 것이겠죠.

예를들어, N명의 유저가 댑을 사용중일때, 관리자가 업그레이드 권한을 다 가지고 있다면 단 한번의 트랜잭션으로 업그레이드가 끝날겁니다. 만약
과반 투표를 한다면 N보다는 적겠죠.(많은 토큰을 들고있다면 더 큰 투표권이 있을테니까요) 만약 유저 본인이 어떤 버전을 쓸지 선택한다면,
업그레이드 하는데 최대 N번의 트랜잭션이 필요할겁니다.

> 마치 과거의 [비트코인 세그윗2x
> 포크](https://www.coindesk.com/understanding-segwit2x-bitcoins-next-fork-might-different)
논란을 떠올리게 합니다. 당시 비트코인은 해시파워가 일정한 수준 이상 모이면 업그레이드를 하겠다고 했는데, 사실 이는 충분한 탈중앙화 접근방식이
아니라는 비판이 있었죠. 차라리 문제가 생기면 하드포크를 해서, 분리시켜 버리는게 더 진정한 탈중앙화 방식이라는 의견이 많았습니다.

그리고 6번의 실습과 토론이 이어졌는데요, 실습은 Trusted한 코드로부터 Trustless한 방향으로 개선해 나가는 방식으로 진행되었습니다.

실습자료 링크 :
[https://github.com/ali2251/Upgradable-contracts](https://github.com/ali2251/Upgradable-contracts)

# Task1 : 컨트랙트 재배포를 통한 업그레이드

![](https://cdn-images-1.medium.com/max/800/1*ySNHd3rg5REILHRK1ahfIA.png)
*<center>새로운 컨트랙트를 배포하고, 유저들에게 그냥 새로운것을 쓰라고 한다.</center>*

요즘 토큰 컨트랙트에 문제가 생기면 가장 많이 쓰는 방법입니다.

![](https://cdn-images-1.medium.com/max/800/1*Nh4rYww4A0HJd73w6ixNvg.png)
*<center>불친절한 가이드라인..</center>*

가이드라인도 줍니다.(내용이 미국식 교과서 방식이네요^^;;) 이걸보고 “응??” 잠깐 멍때리다가 다시 정신차리고 코딩했습니다.

*<center>이정도는 간단히 짜버립시다.</center>*

Score컨트렉트의 setScore() 함수에 대한 개선이 필요하면 유저들에게 ScoreV2를 새로 배포해서 쓰라고 이야기 하면 됩니다.
간단합니다.

# Task2 : 데이터와 로직을 분리

![](https://cdn-images-1.medium.com/max/800/1*Ku_Dg1WJtvCBcwxqBEGxkQ.png)

테스크1과 비교했을 때, 새로운 버전을 위해서 새로운 컨트렉트를 배포해야된다는 점에서는 같지만 이와 같은 방식으로 데이터와 로직을 분리하면
**데이터 마이그레이션에 대한 부담** 이 줄어듭니다. 예를 들어 토큰을 재배포 할 때, 잔액분포를 유지할 필요가 있습니다. 일일이 다시 나눠줄
수는 없으니까요.

> 이 패턴으로 실무에서 많이 쓰이는 컨트랙트는 [giveth팀의
> MinimeToken컨트랙트](https://github.com/Giveth/minime)가 있습니다. 잔액 데이터를 분리해서 관리하는
방식이죠. 온더에서는 이를 응용해, [새로운 컨트랙트 배포 없이 주기적으로 토큰 이자를 일괄적으로 줄 수 있는
PosController라는걸](https://github.com/Onther-Tech/pos-controller) 만들기도 했습니다.

![](https://cdn-images-1.medium.com/max/800/1*KcUC-cvnbp8F_REG0F4x2A.png)
*<center>문제 참 쉽게낸다</center>*

요구사항에 맞춰 코딩을 하면 다음과 같습니다.

*<center>V1, V2는 모두 ScoreStorage를 참조하고 있다</center>*

스코어 정보가 ScoreStorage에 저장되어 있어, setScore()함수의 로직이 변경되어도 기존 스코어 정보는 남아있습니다.

# Task3 : 키-밸류 쌍을 통해 데이터를 저장

![](https://cdn-images-1.medium.com/max/800/1*ct88Z_PnztTmDdGW1iXT5w.png)

Task3이 Task2와 다른점은 스토리지 컨트랙트에 저장할 상태변수와 32바이트의 식별자를 (키,값)쌍으로 만들고, 이를 바탕으로 데이터를
저장한다는 점입니다.

이러한 방식이 제안된 맥락은 로직을 처리하는 컨트랙트는 재배포를 통해서 기능을 업그레이드 할 수 있지만, 데이터를 저장하는 스토리지 컨트랙트는
재배포하기 어렵다는 점에 있습니다. 만약 업그레이드를 했는데, 저장할 데이터가 스코어가 아니라면? 키값으로 쓰이는 식별자를 다른것으로 바꿔주면
되죠. **스토리지에 저장할 수 있는 상태변수의 식별자 통해 저장할 수 있는 데이터의 범주를 넓혀주는 효과** 가 있습니다.

![](https://cdn-images-1.medium.com/max/800/1*im7vd_bhsBfuR_4NXG3WBA.png)
*<center>현장에서 라이브코딩까지 하는Ali Azam</center>*

*<center>이렇게 짜면 됩니다^^</center>*

# Task4: 프록시 패턴을 통한 업그레이드 가능한 컨트랙트

4번 실습을 하기 위해서는 몇가지 사전지식이 필요합니다.

![](https://cdn-images-1.medium.com/max/800/1*TbbcKxtbKOZvR6kP0otBdg.png)
*<center>이더리움의 실행모델에서 스택, 메모리, 스토리지</center>*

EVM은 스택기반의 가상머신이자 컴퓨터로 볼 수 있고, 내부적으로 컴퓨터의 램과 같은 역할을 하는 메모리(Memory)와 저장공간 역할을 하는
스토리지(Storage)가 있습니다.

![](https://cdn-images-1.medium.com/max/800/1*uisf3EmzefW1e10x48wOIQ.png)
*<center>DelegateCall..드디어 나옴</center>*

프록시 패턴(Proxy Pattern)을 사용하기 위한 핵심개념인 DelegateCall에 대한 설명이 이어집니다. 컨트랙트의 스토리지를
이용해서 다른 컨트랙트를 호출해 기능을 사용하는 것을 뜻하고, mgs.sender와 msg.value는 호출과정에서 같이 전달됩니다.

![](https://cdn-images-1.medium.com/max/800/1*9jcoV75hjGc3V61gOb1kag.png)
*<center>Magic code</center>*

프록시 컨트랙트를 쓰기 위한 솔리디티 어셈블리 매직코드입니다. 이코드에 대한 상세한 설명은 [제플린의 블로그
글](https://blog.zeppelinos.org/proxy-patterns/) 참조.

![](https://cdn-images-1.medium.com/max/800/1*OZ9zNQA9Rscl6Ba-YY0XsQ.png)
*<center>이번에 만들건 이거다.</center>*

녹색은 바뀌지 않는 부분이고, 붉은색은 업그레이드가 이뤄지는 부분입니다. 구조적으로 Proxy컨트랙트는 Contract A를
delegateCall하게 되죠.

![](https://cdn-images-1.medium.com/max/800/1*ts6oX-BriJ0Tm5Ps6y8gxQ.png)
*<center>실습 시작~!</center>*

*<center>noi가뿐하게 짜줍니다.</center>*

Proxy컨트랙트의 setImplementation() 함수를 통해서 V1에서 V2로 기능이 업그레이드 된 컨트랙트의 주소를 새롭게 지정해 주면
됩니다.

실행 흐름은 Proxy컨트랙트에다가 setScore()함수를 때리면, 해당 함수가 Proxy에 명시적으로 선언되어 있지 않기 때문에,
fallback function인 익명함수를 치고, 익명함수 내부에서는 어셈블리 코드를 통해 트랜잭션 데이터를 가지고 Score컨트랙트를
delegateCall해서 Score가 상속한 ScoreStorage의 값을 바꿉니다.

# Task5 : 프록시 패턴의 올바른 구현과 올바르지 않은 구현

여기부터는 Unstructured한 업그레이드 가능한 특성에 대해서 이야기합니다. 첫 버전에서는 변수를 1개만 쓰다가 버전이 올라가면서 추후에
변수를 2개이상으로 늘려서 쓸 수도 있겠죠?

![](https://cdn-images-1.medium.com/max/800/1*IGANIWyy8kW4NNZIUdKV2Q.png)

컨트랙트가 스토리지를 다루는 법에 대해서 이야기합니다. 컨트랙트의 상태변수는 각각 (스토리지 키, 값)으로 관리되는데, 위의 그림에서 볼 수
있듯이 address형의 implementation 변수는 처음 선언되었기 때문에 0x00의 스토리지 키를 통해서 변수에 접근하고, 두번째
선언된 uint256형의 number변수는 0x01을 통해 접근하게 됩니다.

*<center>올바른 구현과 올바르지 않은 구현</center>*

올바른 구현의 경우 39:40 라인이 모두 들어갑니다. score변수와 lastPersonToSetTheScore변수는 스토리지 키 값을 각각
0x00, 0x01을 쓰게되죠. 따라서 65라인에서 setScore를 호출하지 않아도 lastPersonToSetTheScore변수의 값은 이미
초기화 되어 있습니다.

만약 올바르지 않은 구현을 하게 된다면, 40라인이 빠지게 되고 74라인 이후부터 보면 되는데, 81라인의 setScore함수가 호출되지
않는다면, lastPersonToSetTheStorage값은 초기화되지 않은 상태가 됩니다. 왜냐하면 이름만 달라졌을 뿐 동일한 스토리지 키값을
가지고 있기 때문입니다.(앞서 레이아웃에서도 이야기했지만, 변수이름과 상관없이 스토리지는 0x00부터 순서대로 할당됩니다)

이 경우 만약 lastPersonToSetTheStorage값이 score로 초기화된상태로 시작되어서 심각한 오류가 생길 수 있죠. 제일 처음
언급했듯이, 완벽한 구조에 대한 이해를 하지 않은 상태로 업그레이드 구조를 짜면 큰 문제가 생길 수 있습니다. (그러니 꼭 전문 오딧팀에게
검토를 받으세요^^)

> Thanks [Carl(4000D)](https://github.com/4000D) for advising this section.

# Task6 : Unstructured Storage

![](https://cdn-images-1.medium.com/max/800/1*a5F67dYsF-AT1oeYFGbDxw.png)

탈중앙화에 대해서 이야기를 하기 전에 Unstructured Storage에 대해서 이야기를 조금 이어갑니다. 만약 로직 부분의
implementation을 특정한 위치에 저장하려고 한다면? 위에 레이아웃에서 말했듯 스토리지 변수는 0x00부터 할당되기 때문에 다양한
구현코드를 별도로 관리하고자 한다면 이러한 제약을 커스터마이징해서 풀어줄 필요가 있습니다.

![](https://cdn-images-1.medium.com/max/800/1*TbAxYtU8hUoBQFE5eI713w.png)

impl이라는 위치에 값을 기록해서 사용하고, 이를 불러서 쓸 때는 regKey값을 통해서 불러오면 됩니다. 저장할 때 특정한 슬롯을 이용하고
불러올때는 슬롯의 키값을 이용하는것이죠.

# Trustless Proxy

이전까지는 전부 업그레이드 가능한 컨트랙트를 만드는 기법(technique)에 관한 내용이었습니다. 이를 탈중앙화시키는건 조금 다른 문제죠.

![](https://cdn-images-1.medium.com/max/800/1*6H_C4pqqrBsCHuyCMW5REA.png)
*<center>프록시를 탈중앙화 시켜봅시다!</center>*

* **프록시 컨트랙트를 탈중앙화 시키는 방법은 유저들에게 어떠한 버전이든 택할 수 있는 자유를 주는 것이다**
* **프록시 컨트랙트는 다른 외부 계정(external account)이나 컨트랙트 계정에 의해 통제(operate)되어서는 안된다.**

정말 중요한 개념입니다. 유저들이 본인들이 쓸 수 있는 버전을 선택할 수 있는 자유를 주는 것이 진정한 탈중앙화라는 것이죠. V1,V2,V3라는
버전이 나왔을 때 유저들은 각각의 버전을 비교해 본인들이 원하는 기능과 스펙이 담긴 기능을 쓰게 열어주는겁니다. 선택의 자유(free to
choose)를 부여하는 것이죠.

![](https://cdn-images-1.medium.com/max/800/1*z86JEJhLvfRz4bLCjxtnWA.png)

*<center>프록시는 delegateCall을 하기 전에 Registry를 체크한다</center>*

프록시 컨트랙트는 delegateCall을 호출하기 전에 Registry에 담긴 정보를 한번 확인합니다. 여기서 확인하는 정보는
`누가`호출하느냐는 것이죠. 예를들어 유저 A는 V1을 호출하고 유저 B는 V2를 호출하게 됩니다. 유저 A는 V1을 택했고, 유저 B는 V2를
택합니다. 컨트랙트 관리자는 이러한 로직에 개입하지 않습니다. 다만 V3, V4등을 계속 붙여서 **유저의 선택의 폭을 넓혀줄** 뿐입니다.

115번 라인을 보면 별도로 Registry컨트랙을 통해 저장된 유저 정보에 따라서 호출되는 로직부분이 다릅니다. 결국 유저들은 본인이 업그레이드 하기로 선택한 버전을 선택하고 이용합니다.

![](https://cdn-images-1.medium.com/max/800/0*g6O-MbwOOweUGvV6.jpg)

*<center>Registry를 통해 유저별로 다른 로직을 delegateCall하게 된다.</center>*


# 결론 및 사견

만약 크립토 키티를 탈중앙화 된 상태로 업그레이드 가능하도록 만든다면 어떻게 될까요? 예를들어

* V1에서는 고양이를 사고 팔 수만 있습니다.
* V2에서는 고양이에게 집을 만들어 줄 수 있습니다.
* V3에서는 고양이들끼리 싸움을 시킬 수 있습니다

개발팀은 그저 V1,V2,V3를 만들어서 펼쳐둡니다. 유저들은 성향에 따라서 마음에 드는 버전을 선택해 고양이 게임을 즐기게 되죠.

그렇지만 이러한 자유로운 선택에는 문제가 있습니다. 만약 유저 A가 V2에서 집을 잘못 만들어 주고, 마음에 안들어서 집 자체가 문제가 안되는 V1으로 돌아가버린다면 어떨까요?

자유와 방임은 다릅니다. 이런 방임 형태의 버전관리를 하게 될 경우 크립토키티라는 고양이게임의 정체성이 유저들의 널뛰기로 매우 혼란스러워 질 수 있죠. 만약 새롭게 추가되는 기능이 경제적인 이익과 관련이 있다면(그럴 가능성이 매우 높습니다) 이는 매우 큰 문제가 될 수 있습니다.

글 내용에 포함하지는 않았지만, 이에 대한 대안으로 Ali Azam은 개발팀은 유저들에게 일정한 기간동안 업그레이드 할 수 있는 기회를 주고,
이 기간이 지나면 더이상 업그레이드를 하지 못하도록 하는 정책 아이디어도 냅니다. 그렇게 되면 앞서 얘기한 널뛰기 문제를 해결할 수 있죠.

하나 더 드는 생각은 개발팀이 만약 애자일한 방식으로 작업을 하게된다면, 어플리케이션을 처음 이용하는 유저는 수십, 수백개의 버전을 선택해야 하는 문제에 봉착하게 됩니다. 그렇게 선택의 자유를 열어주는 것이 유저에게 주는 가치와 개발팀이 중앙화된(Trusted) 의사결정 방식으로
선택해 끌고가는 것이 유저에게 주는 편익중 **어느 것이 더 지속가능한 어플리케이션일까요?**

또한 Trustless와 Trusted의 **중간적인 형태로 투표 혹은 이에 준하는 거버넌스** 룰을 적용하는것과 비교하면 어떨까요?

탈중앙화에 대한 근본적인 질문과 강한 여운을 남긴 상태로 세션은 마무리되었습니다. 마지막날의 마지막 세션 이었음에도, 발표를 마치고 한참동안
앉아서 많은 생각에 잠기게 했습니다. 모두가 함께 고민해볼만한 좋은 주제인 것 같습니다.

지금까지 긴 글 읽어주셔서 감사합니다.


# Related links

* [Blog](https://medium.com/onther-tech/%EC%B2%A0%ED%95%99%EC%9E%90%EC%9D%98-%EB%8D%B0%EB%B8%8C%EC%BD%984-%EC%B0%B8%EA%B4%80%EA%B8%B0-trustless-smart-contract-upgradability-f16ec937b96e)

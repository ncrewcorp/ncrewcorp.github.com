---
layout: entry
title: make의 활용.
author: ncrewent
author-email: taesung.kim@ncrewcorp.com
description: make의 사용법을 정리해 봅니다.
publish: True
---

1 make

소프트웨어 빌드를 자동화하기 위한 툴. Makefile 이란 파일에 빌드방법을 서술하고 make 를 실행하는 식으로 돌아간다.
2 왜 만들어졌나

유닉스 시스템이 난립하던 시절에 여러 시스템에서의 빌드를 자동화 하고자 나온 툴.
3 문제점

빌드라는 작업은 매우 복잡한 작업이며 계속해서 더 복잡해 지고 있는 중이다. 따라서 make 만으로는 처리하기 어려움 문제들이 나오고 이를 위해 추가 유틸리티들이 개발되었다.

    autoconf
    automake
    libtool

이는 결국 make 의 사용법을 더 어렵게 만들었고 이 autotools 들은 욕을 먹기 시작했다. autotools sucks
4 대안점들

수도없이 많다. make alternatives 몇가지만 적어보면

scons
    python 기반. python 기반이라는게 장점이자 단점. 빌드속도가 매우 느리다는 평이 많다. 구글은 주로 이걸 쓴다. 
bjam
    jam 이란 빌드툴의 boost 변종버전. boost 를 직접 빌드해봤다면 알것이다. 실제 이걸로 프로젝트 진행을 시도해본적이 있었으나 몇가지 문제로 인해 사용을 접었다. 그냥 C++ 을 쓴 단순한 라이브러리만 만들어야 하는 경우라면 이걸로 충분. 
waf
    scons 의 변종. scons 의 문제점들을 수정하려고 나왔으며 일부에서 쓰이는 중. 하지만 내가 이걸 살펴볼 당시에는 unstable 상태라서 waf 소스를 프로젝트 내에 같이 넣어서 배포하기를 권장하고 있었고 이는 결국 waf 사용에 있어 하위호환을 철저히 지키지 않겠다는 소리나 마찬가지였으므로 사용을 포기했다. 
cmake
    내가 현재 주로 쓰는 빌드툴. 직접 빌드를 해주는 시스템이 아니고 다른 빌드툴을 위한 빌드환경을 뽑아주는 툴. 

또한 IDE 들이 빌드툴기능을 포함한 경우도 많다. ( msvc 의 솔루션 등등 )

이 외에도 각 언어별로 자신만의 빌드툴을 제공하는 경우도 많다. 예를들어 python 의경우 distutils 를 제공하고 좀더 최신 언어인 go 같은 겨우 아예 언어차원에서 make 를 쓰지 않고 같이 배포되는 내장 빌드툴을 사용하도록 권장한다.
5 cmake

    meta build
    ms 개발자와 같이 프로젝트를 진행할 경우 솔루션 파일을 뽑아주기 때문에 ms 개발자들의 거부감을 줄여주는 효과가 있다. 물론 이경우에라도 ms 개발자들이 cmake 를 배워야 한다는 사실은 변하지 않지만 적어도 최초에 비쥬얼 스튜디오로 소스를 볼수 있다는 점은 다른 팀원들을 납득시키는데 크게 도움이 된다.
    cmake 자체는 정말 병신같다. cmake 스크립트 문법들은 정말 답이없고 make 에서 편히 하던 일을 cmake 에서 하려면 코딩을 해야 한다. 물론 cmake 에서 편한게 make 에선 어려운것도 있으니 트레이드오프.
    cmake 가 병신같지만 계속 쓰는 이유는 다른건 더 병신같기 때문.

6 make
6.1 make 를 배워야 하나

    autotools 까지 필요한 복잡한 빌드를 해야 한다면 모를까 간단한 작업에는 make 처럼 편한게 없다. 아직도 make 는 이바닥 표준 빌드툴이다.
    따라서 간단한 make 사용법정도는 익혀서 알아두되 autotools 는 멀리해야 한다. 만약 자신이 새로운 프로젝트를 시작해야 한다면 autotools 말고 다른 대안을 찾아보자.

6.2 autotools 에 대한 찬성론도 물론 있음

이 유파에 속하는 개발자들은 신규 개발자들이 사용법을 숙지하지 못하고 autotools 를 썼기 때문에, 즉 생각없이 autotools 관련 코드들을 복사해서 썼기 때문에 autotools 가 욕먹는 것이라고 생각하며 autotools 만으로 대부분의 문제를 해결할수 있다고 생각한다.
6.3 Makefile 기본적인 모양

target: dependencies...
        commands
        ...  

6.3.1 foo.c 를 이용해서 foo 를 빌드하는 간단한 예제

foo: foo.c
        gcc -o foo foo.c

6.3.2 Automatic Variables 이란걸 이용하면 반복 타이핑을 줄일수 있다.

foo: foo.c
        $(CC) $< -o $@

6.3.3 빌드외에도 다양하게 사용 가능

foo: foo.c
        $(CC) -o $@ $<
clean:
        rm foo  

6.3.4 변수도 사용 가능하다

FOO = foo
all:
        @echo $(FOO)    

6.3.5 변수값을 환경변수에서 가져오도록 하는것도 가능.

FOO ?= foo
all:
        @echo $(FOO)  

위 Makefile 을 이렇게 실행해보자

FOO=bar make



  [카르테]: http://carte.gamescampus.co.kr/
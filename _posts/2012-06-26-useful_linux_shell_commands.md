---
layout: entry
title: 유용한 리눅스 커맨드.
author: ncrewent
author-email: taesung.kim@ncrewcorp.com
description: 유용한 리눅스 커맨드들을 정리해 봅니다.
publish: True
---

1 RTFM

man
    section 개념을 알아두는게 좋다. man man 을 쳐보자. 
apropos
    내가 뭘 찾는지 모를때. 사실 man man 을 쳐보면 man -k 가 이역할을 해준다. 즉 man 만 알아도 된다. 
info
    man 보다 info 쪽에 정보가 많은 경우도 있다. 단 그경우 man 이 info 보라고 알려줌ㅋ. 즉 man 만 알아도 된다. 

2 pipeline, redirection

    쉘이 제공하는 기능. 즉 man bash 에서 관련 내용을 찾을수 있다.
    pipeline 은 foo | bar 형태로 사용하며 프로그램 foo 의 표준출력을 프로그램 bar 의 표준입력으로 넣어주는 것
    redirection 은 foo < a 또는 foo > a 식으로 사용하며 프로그램 foo 의 표준출력을 a 로 저장하거나 프로그램 foo 에 a 파일의 내용을 표준입력으로 넣어주는것.
    redirection 은 다양한 형태가 가능한데 몇가지만 기억해두자
        > 와 >>
        2> 로 표준 아웃도 redirection 이 가능 
    man bash 가 제일 중요한것. 

3 자주 쓰이는 명령들

bash
    쉘에서 타이핑 하는 명령중 일부는 바이너리가 아니라 bash 가 자체 제공하는 기능들. 
history
    man bash 에서 HISTORY EXPANSION 을 보자. 몇가지 자주 쓰이는 것만 적자면

        history
        !sftp
        !! 

ls,cd,pwd
    기본적인 디렉토리 관리, 단 cd 는 bash 기능이지만.. 
pushd,popd
    여러 디렉토리를 이동할때 편리. cd - 라는 명령도 유용하다. 역시 bash 명령이며 이걸 자주 쓰게 된다면 차라리 screen 을 쓰는걸 고려해보는게 좋다. 또는 필요한경우 bash 를 새로 실행했다가 exit 하는것도 편리함. 
ln,cp,mv,rm
    기본적인 파일 관리 명령들 
cat, more, less
    파일의 내용을 확인할때 쓴다. 
echo
    화면에 뭔가를 출력할때 쓴다. pipeline 과 함께 유용하게 쓰임. 
touch
    빈파일을 생성해 낸다. 
diff
    두 파일을 비교 차이점을 보여준다 
which
    실행파일의 경로를 보여준다. 같은 이름의 파일이 시스템내에 두개이상 존재할때 어떤놈이 실행되는지 알기위해 쓰이게 된다. 
file
    주어진 파일이 뭐하는 파일인지 알려준다. 이건 매우 유용. 디버깅할때도 유용. 관심있다면 libmagic 을 검색해본다. 
ps
    현재 시스템에 돌고 있는 프로세스들을 확인할때 사용. 난 주로 ps -ef 식으로 쓴다. 
kill
    프로세스에 원하는 시그널을 보내기 위해 사용 
killall
    알아두자. 단 쓸때는 신중하게 
pkill,pgrep
    ps-kill, ps-grep 을 간단하게 사용하는 명령들 
pstree
    프로세스 트리를 보여준다. 자주 쓰이지는 않지만 종종 필요함. 
telnet
    상대 호스트에 네트웍 접근이 가능할때 찔러보는 용도로 자주 쓰게 된다. nc 라는 툴을 쓰는것도 좋음 
nc

    이 머신에 방화벽이 돌고있는지 확인하기 위해 특정 포트에 리슨을 할때 간단히 쓸수 있다.
    nc -l 1234
        1234포트에 리스닝해보자. 이미 사용되고 있는지 알수 있다.
    nc 127.0.0.1 1234
        1234포트에 접속해보자

ssh,sftp
    알아야겠지. 
ftp
    아직 널리 쓰임 
screen
    알아두면 굉장히 편하다. tmux 라는 놈도 있으니 개인적으로 쓸때는 취향따라 선택 
nslookup
    DNS 문제는 아주 흔하게 발생한다. 이건 꼭 알고 있어야 함 
curl
    HTTP 방식의 API 를 쓴다면 자주 쓰이게 된다. 
traceroute
    내 경험상 이걸로 이걸로 문제를 발견하고 책임자에게 전달해준적이 종종 있음. 
locate,updatedb
    편리함! 
sudo,su

bc
    계산기 
reset
    화면이 뭔가 노이즈로 가득찼다면 이걸로 복구. 주로 바이너리 파일을 화면에 출력했을때 화면이 엉망이 되는데 이때 유용함. 

4 경험상 자주 쓸일은 없던데 그래도 알아둬야 함

tee
    tee 는 커맨드도 종종 쓰이지만 이 개념 자체가 여기저기서 쓰이며 그때 tee 라고 칭하는 경우가 많다. 개발자라면 알아두자. 
script
    종종 쓸일이 있다. 
scp
    사실 좀 자주 쓰이는 편 

5 현재 시스템 상황을 파악할때 쓰는 명령들

netstat
    현재 포트 상황이 궁금하다면 이걸 써본다. 나는 주로 netstat -anp 로 실행. 윈도우에도 이게 있긴 한데 -p 옵션은 없던걸로 기억함 
ss
    우왕ㅋ굳ㅋ 
top,htop,atop

lsof

lshw

vmstat

w,who
    누가 뭘 하고 있나 
ifconfig
    IP 가 궁금하냐? 
date
    시간이 궁금하냐? 
time
    이건 간단한 측정툴임. date 가 나와서 적긴 했지만 사실 개발할때 주로 씀 
du,df
    디스크 사용량이 궁금하냐? 
uname,hostname

last

uptime
    재미삼아 
ping


6 로그를 뒤질때 유용한 명령들

grep
    서버개발자라면 grep 을 잊을일은 없을것. -v, -A, -B 등의 옵션은 머리속에 넣어두자 
xargs
    주로 find 와 콤보로 쓰임 

find . -name "*.log" | xargs cat

find
    로그가 널려있을때 이를 찾아주는 놈. 그외 1주일전 파일을 찾아서 지운다거나 하는 용도로도 사용. 
tail
    역시 서버 개발자라면 tail -f 를 잊을일이 없을것. 그냥 tail 도 자주 쓰임 
head
    나는 주로 덩치큰 로그파일을 뒤져볼때 커맨드라인 검증을 위해 데이타량을 줄어야 할때 쓴다. 

grep foobar SOME_HUGE_FILES | head        

awk
    쓰기 거북하지만 간단한 몇가지 배워두면 정말 편하다. 

grep speedhack proxyd.?.log.2012-05-23 | awk  '{ if ($18 > 20) {print $0} }'

uniq
    로그를 뒤질때 자주 쓰일것이다. 주로 awk 와 같이 쓰이게 되더군? 
sort
    uniq 의 친구 또는 sort -u 식으로도 사용가능 
wc
    wc -l 이 특히 자주 쓰임 

7 그외 텍스트를 다루는 커맨드들, 유용하지만 난 자주 안쓰게 되더라

paste
    난 이걸 써야할 상황이면 python 스크립팅을 하는 편. 
sed
    사실 좀 자주 씀ㅋ 
tr

printf
    echo 보다 잘난놈 
nl

seq

sha1sum


8 주로 디버깅 상황에서 주로 쓰이는 명령들

gdb
    종결자 
ldd
    디펜던시 워커 라고 보면 된다 
nm

strace

ltrace

pstack

pmap

valgrind
    이건 언젠가 따로 설명해줄듯. 
od
    헥스 덤프가 필요하냐? 
strings


9 압축

tar
    tarball 이란 표현을 알아두자. 개발자끼리 자주 쓰는 용어임 
compress
    이제는 아무도 안쓸것임. 확장자가 .Z 면 이걸로 압축된거. 
gzip
    널리 쓰임. 확장자는 gz 
bzip2
    gzip 보다 압축률이 좋다. .bz2 이게 대세라고 보면 될듯? 물론 아직 gz 를 쓰는 유저들도 많다. 
xz
    7zip 에 가깝다고 보면 되고 이것도 종종 쓰이나 보더라. 내눈엔 잘 안보이긴 하던데. 
zless,bzless,zcat,bzcat
    유용함! 

10 에디터

vi
    이게 없는 유닉스/리눅스는 없을듯 
vim

nano

emacs

ed


11 좀더 깊이 알고 싶어요

bash scripting

man bash

perl scripting

python scripting


  [카르테]: http://carte.gamescampus.co.kr/
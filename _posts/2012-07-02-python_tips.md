# -*-mode:org;coding:utf-8-*-
#+STARTUP: hidestars overview
#+OPTIONS:   ^:nil
#+TODO: TODO(t!) | DONE(d!)
#+TODO: PENDING(p@/!) |
#+TODO: | CANCEL(c@/!)

* python 에서 몇가지 알아두면 좋은 기능들

이라기 보단 현재 우리 프로젝트에서 내가 쓴 기능들.

* lambda

간단히 설명하면 그냥 익명함수임. python 은 lambda 지원이 좋지 못한
언어이므로 나는 가능하면 이름붙여 쓰는 편임.

#+BEGIN_SRC python
  # 이런 함수가 있음ㅋ
  def foo(x):
      return x % 2 == 0
  
  # 실행
  print foo(10)
  
  # lambda 로 표현
  f = lambda x: x % 2 == 0
  
  # 실행
  print f(10)
  
  # 함수가 들어갈 자리에 걍 람다 써도 됨ㅋ
  print (lambda x: x % 2 == 0)(10)
  
  # 함수를 인자로 받는 함수를 쓸때 매우 유용
  print map(lambda x: x % 2 == 0, range(100))
#+END_SRC

* list comprehension

[[http://en.wikipedia.org/wiki/List_comprehension][list comprehension]] 이란 list 에서 list 를 만들어내는 문법이라고 보면
된다. 최근의 언어들은 대부분 유사기능을 지원하는 추세임. 

설명보다는 예제임.

** 1 부터 100 까지의 리스트가 있을때 짝수로만 이루어진 리스트를 뽑아내고 싶다.

#+BEGIN_SRC python
  # 원래 이런 리스트가 있는데
  l = range(100)
  
  # 짝수만 골라내고 싶다.
  r = []
  for e in l:
      if e % 2 == 0:  
          r.append(e)
  print r
  
  # list comprehension 으로 나타내면 한줄임
  print [e for e in l if e % 2 == 0]
  
  # filter 라는 함수로 동일한 효과
  print filter(lambda e: e % 2 == 0, l)
#+END_SRC

** DB 의 응답중 None 이 포함된 값을 -1 로 바꾸고 싶다.

#+BEGIN_SRC python
  rs = ((0, "foo", 10),
        (1, "bar", 12),
        (2, "baz", None))
  
  # for 문 돌리면 이렇게 되겠지.
  r = []
  for x,y,z in rs:
      r.append((x,y,z if z else -1))
  print r
  
  # 걍 한줄임
  print [(x,y,z if z else -1) for x,y,z in rs]
  
  # list comprehension 안에 list comprehension
  print [[f if f is not None else -1 for f in fs] for fs in rs]
  
  # list comprehension 을 겹쳐 쓰지 보기 흉하네.
  def none_to(x):                           # 리스트등을 받아서 그중 원소가 None 인 값을 x 로 바꿔주는 함수 를 리턴
      def fn(l):
          return [e if e is not None else x for e in l]
      return fn
  print [none_to(-1)(r) for r in rs]
  
  # none_to(-1)(r) 이 어색한가?
  none_to_minus_one = none_to(-1)
  print [none_to_minus_one(r) for r in rs]
  
  # map
  print map(none_to_minus_one, rs)  
#+END_SRC

** 이런것도 알아두자

#+BEGIN_SRC python
  # 유저그룹 A 가 있다고 생각해보자
  a = range(10)
  # 유저그룹 B 가 있다고 생각해보자
  b = range(100,110)
  
  # A 와 B  유저 조합
  print [(x,y) for x in a for y in b]
  
  # 특정 조건에 맞는 A 와 B 유저 조합
  print [(x,y) for x in a if x % 2 == 0
               for y in b if y % 2 == 1]
  
  # 아래쪽에서 다시 언급하겠지만.. generator 를 써봤음
  even = lambda x: x % 2 == 0
  odd  = lambda x: not even(x)
  print list(((x,y) for x in a if even(x)
                    for y in b if odd(y)))  
#+END_SRC

* generator

이터레이터처럼 돌아가는 녀석. list comprehension 과 형태가 유사하다.

** yield?

#+BEGIN_SRC python
  # 무한수열을 yield 로 표현했다.
  # functools 에 무한 이터레이터들 참고    
  def infinite(i = 0):
      while True:
          yield i
          i += 1
  
  # iteratable 로부터 n 개의 원소를 꺼낸다.
  # 예제라 직접 구현했는데 실제론 itertools 모듈을 쓰는게 좋겠지.        
  def take(n, iteratable):
      i = 0
      while i < n:
          yield iteratable.next()
          i += 1
  
  # 무한수열로부터 100 개만 꺼내서 출력해봤음ㅋ
  # list(iteratable) 로 리스트로 변환가능하다는걸 잘 봐두자
  print list(take(100, infinite()))
#+END_SRC

** 1 부터 시작하는 짝수들 100 개

#+BEGIN_SRC python
  even = lambda x: x % 2 == 0
  
  def infinite(i = 0):
      while True:
          yield i
          i += 1
  
  def take(n, iteratable):
      i = 0
      while i < n:
          yield iteratable.next()
          i += 1
  
  # 대강 만든거. itertools 의 것을 쓸것.
  def take_while(pred, iteratable):
      while True:
          e = iteratable.next()
          yield e
          if pred(e): break
  
  # infinite(1) 에서 짝수를 100 개 꺼내보자.
  print list(take(100, (x for x in infinite(1) if even(x))))  
#+END_SRC

* decorator

#+BEGIN_SRC python
  if __debug__:
      def whendebug(fn):
          def nfn(*args, **kw):
              return fn(*args, **kw)
          return nfn
  else:
      def whendebug(fn):
          def dummy(*args, **kw):
              pass
          return dummy
      
  @whendebug
  def foo():
      print "> foo"
  
  @whendebug
  def bar():
      print "> bar"
      return "bar"
  
  print foo()
  print bar()
#+END_SRC

functools 라는 모듈을 읽어볼것.
__doc__, __name__ 등도 그대로 옮겨주는 wraps 란 함수가 유용함.

* with statement

#+BEGIN_SRC python
  # open,blahblah,close 는 흔히 쓰는 패턴
  def readfile(name):
      f = open(name)
      buf = f.readlines()
      f.close()
      return buf
  print readfile("/etc/passwd")
  
  # with 를 쓰면 코드가 간단해짐
  def readfile2(name):
      with open(name) as f:
          return f.readlines()
  print readfile("/etc/passwd")
  
  # __enter__ 와 __exit__ 를 구현해주면 됨ㅋ
  class Foo(object):
      def __enter__(self):
          print "Foo::__enter__"
          return self
      def __exit__(self, type, value, traceback):
          if type:
              print "Foo::__exit__ exception"
              print type,value,traceback
          else:
              print "Foo::__exit__ no exception"
  with Foo():
      pass
  with Foo():
      raise Exception("fuck")
#+END_SRC


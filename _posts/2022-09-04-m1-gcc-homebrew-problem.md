---
layout : single
tags : [c++, gcc, m1, mac]
title : bits/stdc++.h 를 위해 gcc 를 M1 Mac 에서 brew 통해서 설치할 때
---



## 1. M1 Mac 에서 Homebrew 설치 경로

* M1 Mac 이 나오면서 Homebrew 설치 경로도 바뀌었다.

* ```sh
  $ which brew
  /opt/homebrew/bin/brew
  ```



## 2. brew 를 통해 설치할 경우 어떻게 실행 해야하나?

* ```sh
  $ cd /opt/homebrew/bin
  $ ls
  aarch64-apple-darwin21-c++-12		lzdiff
  aarch64-apple-darwin21-g++-12		lzegrep
  aarch64-apple-darwin21-gcc-12		lzfgrep
  aarch64-apple-darwin21-gcc-ar-12	lzgrep
  aarch64-apple-darwin21-gcc-nm-12	lzless
  aarch64-apple-darwin21-gcc-ranlib-12	lzma
  aarch64-apple-darwin21-gfortran-12	lzmadec
  acountry				lzmainfo
  adig					lzmore
  ahost					nghttp
  brew					nghttpd
  brotli					nghttpx
  c++-12					node
  code					npm
  corepack				npx
  cpp-12					pzstd
  g++-12					unlz4
  gcc-12					unlzma
  gcc-ar-12				unxz
  gcc-nm-12				unzstd
  gcc-ranlib-12				vue
  gcov-12					xz
  gcov-dump-12				xzcat
  gcov-tool-12				xzcmp
  gfortran				xzdec
  gfortran-12				xzdiff
  h2load					xzegrep
  jemalloc-config				xzfgrep
  jemalloc.sh				xzgrep
  jeprof					xzless
  lto-dump-12				xzmore
  lz4					zstd
  lz4c					zstdcat
  lz4cat					zstdgrep
  lzcat					zstdless
  lzcmp					zstdmt
  ```

* 위와 같이 brew 가 설치된 폴더로 가서 목록을 조회하면 g++12 를 찾을 수 있다.

* 저 놈이 우리가 찾는 녀석이다!

* 근데 왜 g++ 이 아닌 g++12 일까?

* 원인

  > I was having the same issue. First I installed gcc via homebrew
  >
  > ```
  > brew install gcc
  > ```
  >
  > To avoid conflict with the existing gcc (and g++) binaries, homebrew names the binary suffixed with version. At time of this comment, the latest was gcc-10.
  >
  > You dont have to copy the `bits/stdc++.h` after this. Just compile using `g++-<major-version-number>` instead of `g++`, which would use the homebrew installed binary instead of the default osx one. For me it is
  >
  > ```
  > g++-10 -Wall -O2 -std=c++11 test.cpp -o test
  > ```
  >
  > To check the binary name that homebrew installed you can look in the `/usr/local/bin` directory because thats where homebrew installs packages.
  >
  > Also, make sure that `usr/local/bin` is before `/usr/bin` in your `$PATH`
  >
  > link : references [2] 



## 3. 근데 어쩌다 이런 것을 찾게 되었나?

* 코딩 테스트에 자주 사용되는 C++ header 인 bits/stdc++.h 를 include 하기 위함이다.
* 하지만 파일 생성, 경로 설정을 했으나 stdc++.h 내부에 cstdalign 를 불러오지 못했다.
* 아마 gcc 와 apple 의 clang 은 헤더가 모두 같지는 않으리라 생각했다.
* 따라서 brew 를 통해서 새로 설치한 gcc 로 주로 사용하려했다.



## References

[1] [M1 Mac 의 Homebrew 설치 경로](https://earthly.dev/blog/homebrew-on-m1/)

[2] [brew 를 통해 gcc 설치할 경우 접미사가 붙는다. 작성자 : Himanshu Tanwar](

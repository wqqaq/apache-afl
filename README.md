# apache-afl

An automated setup for compiling & fuzzing Apache httpd server.

More info about the process/journey can be found here: https://0xbigshaq.github.io/2022/03/12/fuzzing-smarter-part2

# Usage

To start the build process, run:

```
./afl-toolchain.sh
```

To start fuzzing:
```
cd fuzzer-dir/
./afl-runner.sh
```

> Tested on: AFL Version ++4.00c (release) under a `aflplusplus/aflplusplus` docker image

change /httpd-2.4.52/server/main.c /httpd-2.4.52/server/core.c for afl-fuzz++4.10a

then,run:
```
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
PREFIX=/usr/local/apache_lab/
CC=afl-clang-fast \
CXX=afl-clang-fast++ \
CFLAGS="-g -fsanitize=address -fno-sanitize-recover=all" \
CXXFLAGS="-g -fsanitize=address -fno-sanitize-recover=all" \
LDFLAGS="-fsanitize=address -fno-sanitize-recover=all -lm" \
./configure --with-apr="$PREFIX/apr/" \
            --with-apr-util="$PREFIX/apr-util/" \
            --with-expat="$PREFIX/expat/" \
            --with-pcre="$PREFIX/pcre/" \
            --disable-pie \
            --disable-so \
            --disable-example-ipc \
            --disable-example-hooks \
            --disable-optional-hook-export \
            --disable-optional-hook-import \
            --disable-optional-fn-export \
            --disable-optional-fn-import \
            --with-mpm=prefork \
            --enable-static-support \
            --enable-mods-static=reallyall \
            --enable-debugger-mode \
            --with-crypto --with-openssl \
            --disable-shared
make -j $(nproc)
make install
```

---
title: Nginx luajit memory crash mmap_wrapper
date: 2019-04-01 11:19:55
tags:
---
### Problem
Nginx plays an import role in internet trade. But recently, a problem comes out. a Nginx worker process occupy a cpu core fully, nearly 100 percent, and it used nearly 1G Bytes memory, so that the lua vm crashed. Becuase in x86-64 architecure, lua jit can only use 1 Bytes mem.

Nginx standard errlog:
```
nginx: lua atpanic: Lua VM crashed, reason: not enough memory
```
### Causes
The Nginx accept many big request with big body, then the lua scripts run with long time on string joint and split, while luajit works bad on this scene, which lead to our problem.

### Solutions
Discard these big requests.
Make luajit use more memory.
According to our pratical use scnario, I choose the second way. Finally, My nignx worker can use 4G Bytes memory. Steps as follow:

Get mmap from https://github.com/Neopallium/mmap_lowmem.
Compile and link mmap_wrapper, we got libmmap_lowmem_mt.so.
Get openresty from https://github.com/openresty/openresty/releases
```
./configure ?with-ld-opt=?/opt/mmap_lowmem/libmmap_lowmem_mt.so? -j8
```
Notes:
In step 4, we may meet compile interrupt, like openssl zlib pcre missing, just fix it, finally, openresty installment wil finish.

Run ./nginx, some hint comes out, like ?? got low-mem: len=0xfe3b6000, start=0x1c4a000, end=0x100000000?
```
  curl 127.0.0.1/mem_test
  Mem used:2863.9989748001 M
```
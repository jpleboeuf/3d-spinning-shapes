# Intro

Original work by Andy Sloane ([a1k0n](https://github.com/a1k0n)).

[`donut.c`](donut.c) has been featured in a couple YouTube videos:
- [Lex Fridman](https://www.youtube.com/watch?v=DEqXNfs_HhY),
- [Joma Tech](https://www.youtube.com/watch?v=sW9npZVpiMI)…

Articles by Andy Sloane:
- "[Have a donut.](https://www.a1k0n.net/2006/09/15/obfuscated-c-donut.html)" (2006)

# Build

With Clang LLVM Windows MSVC, it's quite straightforward
 (no needs to explicitly add the math library),
 but you have to appease the compiler so that it can ingest the donut without vomiting:

```sh
clang -Wno-implicit-function-declaration -Wno-implicit-int -o donut.exe donut.c
```

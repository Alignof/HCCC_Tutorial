# 条件分岐・ループ

さて，実用的なプログラムを書くためには，条件分岐やループがないと生きていけませんね．

ということで，このチュートリアルの締めとして，それらをアセンブリで書く方法について説明していきます．

## ラベル

今回重要になってくるのは，[section2: アセンブリの基礎](/sections/section2_BasicOfAssembly.md) でチラッと登場した「ラベル」という概念です．

section 2 で登場したアセンブリをもう一度見てみましょう．

```
.intel_syntax noprefix
.globl main

main:
    push    rbp
    mov     rbp, rsp
    mov     eax, 0
    pop     rbp
    ret
```

`main:` と書くことで `main` という名前のラベルを作り，そしてそれを外部から参照するために `.globl main` をつけていたのでしたね．


# 次のセクション
[section9: 条件分岐](/sections/section9_Branching.md)

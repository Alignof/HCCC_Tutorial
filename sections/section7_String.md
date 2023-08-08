# 文字列

## グローバルな文字列リテラル

```
.LC0:
        .string "foo bar\n"
```

と先頭に書くと，たとえば

```
mov edi, OFFSET FLAT:.LC0
```

と書くことによって，文字列 `"foo bar\n"` （の先頭アドレス）が `edi` レジスタに入ります．

今扱っているのは 64 ビット CPU であるにもかかわらず，諸事情によりここでは 32 ビットのレジスタを使ってアドレスを格納することになっています．

<!-- 
「諸事情」が書いてある PDF: https://inst.eecs.berkeley.edu/~cs164/fa12/ia32-refs/ia32-chapter-four.pdf

The somewhat mysterious OFFSET FLAT: incantation tells the
assembler to figure out the (4-byte) address where the variable x will end up when the
program is loaded. Even the assembler does not have all the information to figure this out,
since a program may be in several pieces and the assembler does not know where each
piece will go. It is up to the loader to figure this out, so in fact all the assembler does with
the OFFSET FLAT: reference is to make a note in the object file and it is the loader that
will finally fill in the right value in the generated instruction. This is one of the respects in
which object code (which ends up in a .o file after assembly) is not pure machine code.
-->

第一引数をつかさどる `edi` レジスタに入れたので，たとえばこのあとに `call 関数名` することで，文字列リテラルを第一引数に入れた関数呼び出しが可能になります．

これを [section5: ローカル変数](/sections/section5_LocalVariable.md) と組み合わせると，たとえば関数内の `const char *p = "foo";` は次のようにして実現できることがわかります．

```
.LC0:
    .string "foo"

mov QWORD PTR [rbp-8], OFFSET FLAT:.LC0
```

## ローカルな char 配列

# 次のセクション
[section8: ループ](/sections/section8_Loop.md)

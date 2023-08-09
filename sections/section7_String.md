# 文字列・グローバル変数

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

一方で，**関数内**での `char array[] = "foo";` は，次のようにして実現できます．

```
mov BYTE PTR [rbp-20], 0x66
mov BYTE PTR [rbp-19], 0x6F
mov BYTE PTR [rbp-18], 0x6F
mov BYTE PTR [rbp-17], 0x00
```

これは，'f', 'o', 'o', NUL の 4 文字を，`rbp-20` から始まる長さ 4 の領域に格納しています．

実は，次のように書くこともできます．

```
mov DWORD PTR [rbp-20], 0x6F6F66
```

## グローバルな変数

グローバル変数としての

```
int a = 3;
int *p = &a;
int **q = &p - 2;
```

は，次のように書くことができます．

```
a:
        .long   3
p:
        .quad   a
q:
        .quad   p-16

mov DWORD PTR [rbp-8], OFFSET FLAT:a
```

`.long`，`.quad`は確保するサイズを，後ろの値は初期値を表しています．
ラベルはアセンブリ内の「位置」を示していたので，pの初期値はグローバル変数`a`の
アドレスになります．

## まとめ
- グローバルな文字リテラルは`.string`
- ローカルなchar配列はスタックに要素ごとに格納する．
- グローバルな変数は`a: .long 3`や`p: .quad a`


# 次のセクション
[section8: ループ](/sections/section8_Loop.md)

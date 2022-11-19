# ローカル変数
遂にローカル変数を扱えるようになります．  
ローカル変数はスタック領域に確保します．  
  
## ローカル変数を確保  
ではスタック領域をどうやって確保するかというと単純で，スタックポインタを欲しい分だけ減らせば良いのです．  
このスタックポインタの増減はprologueとepilogueで行います．  

以下はセクション2で見せたものに変数の確保を足したものです．
```asm
.intel_syntax noprefix ; #1
.globl main ; #2

main: ; #3
    ; #4: prologue
    push rbp
    mov rbp, rsp
    sub rsp, 0x8 ; allocate stack (sizeof(int) * 2)

    mov rax, 0

    ; #5: epilogue
    pop rbp
    ret
```

rspから8だけ引いたので今8byte分確保されました．

## ローカル変数へレジスタの値を代入
rbpから4byteのところにあるデータにraxの値を入れるアセンブリです．
```
mov [rbp - 0x4], rax 
```


## ローカル変数の値をレジスタに代入
rbpから4byteのところにあるデータをraxに入れるアセンブリです．
```
mov rax, [rbp - 0x4] 
```

## ローカル変数のアドレスをレジスタに代入
scanfなどでアドレスを入れる必要があるときは以下のようにします．  
rbpから4byteのところにあるアドレスをraxに入れるアセンブリです．  
```
lea rax, [rbp - 0x4] 
```


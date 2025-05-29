# Option
## Memory Allocation
- `-no-pie`  
VS Windows ASLR.  
e.g. useful for combining gdb and ghidra.

まずPIE(Position Independent Executable)とは、実行可能コードのアドレスを相対アドレスで参照することでコードがメモリ上のどこに配置されたとしても正常に実行できる機能のことです。
また、ASLR(Address Space Layout Randomization)ではランダム化されない実行可能コードのアドレスもランダム化が行われるなどのセキュリティ的利点が存在します。

位置独立コードに関して、詳しくは以下のリンクをご覧ください。
位置独立コード - Wikipedia

現在、gccのversion 7.3.0では、このPIEがデフォルトで有効化されます。
そのため、今回はデバッグをより簡単にするためにno-pieオプションで無効化しました。

もちろん、PIEが有効だからといって、リバースエンジニアリングができなくなる訳ではありません。
実行中のプロセスIDを指定して、アタッチするなどの方法でPIEが有効なバイナリに対して、動的解析をすることが可能になります。

## Compile Error Handling
- `-w`  
全ての警告メッセージを抑制します。
- `-Wall`  
一部のユーザーが簡単に回避できる（または警告を回避するように変更できる）構文に関するすべての警告が有効になります。
- `-Wextra`  
-Wallが含まない警告が有効になります。
- `-Weverything`  
全ての警告オプションを有効にします。
- `-Werror`  
すべての警告をエラーにします

## Compile Optimization
- `-O0`  
最適化しない。これがデフォルトです。
- `-O1,-O2,-O3`  
 1,2,3の順に最適化が強く設定されますが、順にコンパイル速度が犠牲になります。
- `-Ofast`  
速さを重視した最適化をします。標準準拠しない最適化も行われます。

# Ref  
https://qiita.com/keitean/items/28a519ec1caa8c15abcf
# 素因数分解プログラム Ver. 2.1 PocketGCC 1.50版ソースコード
This document is also available in [English](readme_en.md).

**Windows CE**用のソースコードです。

# ビルド時の要件
Windows CE端末に、[Brain Wiki](https://brain.fandom.com/ja)等を参考にしてPocketGCC・DOS窓Openを使える状態にし、付属のbatファイル(環境に合わせて特にパス周りを書き換えて下さい)を用いてビルドしてください。

なお、製作者はコンパイル時にeMbedded Visual C++ 4.0からコピーしたcommctrl.libをリンカに読み込ませています。これはコマンドバーの表示に必要なものですので、**同様の方法でcommctrl.libを入手するか、ダイナミックリンクを使うか、コマンドバーを削除するか**を行ってください(別版のPocketGCCで、この操作が不要なものがあるかもしれません)。

このコンパイラでは**日本語を含むプログラムをコンパイルできません**。日本語で作成したい場合は、eMbedded Visual C++ 4.0などを使用してください。

# 注釈
ソースコードは、このファイルと同階層にある**win.cpp**です。

実行可能バイナリは**AppMain.exe**として生成されます。
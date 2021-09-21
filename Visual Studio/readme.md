# 素因数分解プログラム Visual Studio 版ソースコード
This document is also available in [English](readme_en.md).

**Windows PC** 用のソースコードです。

## ビルド時の要件
このプロジェクトは、**Visual Studio 2019** で作成されています。

また、動作環境を広げるために **Windows XP がサポートされるビルドツール (v141\_xp)** を使用しているほか、**ARM, ARM64** も有効化しています。これらのビルドツールは別途インストールを行わないと使用できません。インストールされていなければ開いたときに求められるので、「不足しているプラグインのインストール」のような旨の項目をクリックしてインストールしてください。なお、ARM, ARM64 用のビルドツールがなくても **x64, Win32 版のビルドは可能**です。

インストールできない環境では、以下の手順で Windows XP サポートを解除します。
1. 「プロジェクト」->「プロパティ」を開き、上部の「構成」を「すべての構成」、「プラットフォーム」を「すべてのプラットフォーム」に切り替える。
1. 「全般」->「プラットフォーム ツールセット」を「Visual Studio 2019 (v142)」※に変更する。
1. 一旦 OK を押して閉じ、再度同じ項目を表示させる。
1. 「全般」->「Windows SDK バージョン」を「10.0 (最新のインストールされているバージョン)」※に変更する。
1. 「C/C++」->「言語」->「準拠モード」を「はい」に変更する。

※ 2021年9月現在の Visual Studio 2019 における設定です。Visual Studio 2017 などの古いバージョンやより新しいバージョンでも、表示されたオプションの中で最新のものを選択すれば大丈夫なはずです。

## プロジェクト作成手順
参考までに、本プロジェクトの作成手順を以下に記します。
1. 「C++ によるデスクトップ開発」と、個別に「MSVC v142 - VS 2019 C++ ARM ビルド ツール (最新)」「MSVC v142 - VS 2019 C++ ARM64 ビルド ツール (最新)」「VS 2017 (v141) ツールの C++ Windows XP サポート \[非推奨\]」をインストール
1. プロジェクトの作成から、「Windows デスクトップ ウィザード」を選択
1. ソリューションとプロジェクトを同じディレクトリに配置する設定にし、名前を適当に決めて作成
1. 種類を聞かれるので、「デスクトップ アプリケーション (.exe)」「空のプロジェクト」と設定して OK
1. 「ソリューション エクスプローラー」でソースコード類を追加
1. 「プロジェクト」->「プロパティ」から「構成マネージャー」を開く
1. 「プラットフォーム」で「新規作成」を押し、「ARM」でコピー元を「Win32」にし、「新しいソリューション プラットフォームを作成する」にチェックを入れて OK
1. 同様の手順で ARM64 も追加 (コピー元は x64 とした)
1. 「すべての構成」及び Win32 を設定対象にする
1. 「全般」の出力・中間ディレクトリを、他のプラットフォームと同じディレクトリ構造に変更
1. 「すべての構成」及び Win32 と x64 を選択した「複数のプラットフォーム」を設定対象にする
1. ツールセットを「Visual Studio 2017 - Windows XP (v141\_xp)」に変更し、適用
1. 「C/C++」->「言語」にて、「C++ 言語標準」を C++17、「準拠モード」を「いいえ」に変更
   - まさかの Windows XP 用の公式 SDK 自体が C++ 規格に準拠しておらず、非準拠モードにせざるを得ない。
1. 「すべての構成」及び ARM と ARM64 を選択した「複数のプラットフォーム」を設定対象にし、プロパティを開き直す
1. C/C++ 言語標準をそれぞれ C17、C++17 に設定
1. 「すべての構成」及び「すべてのプラットフォーム」を設定対象にする
1. 「マニフェストツール」->「入出力」にて、「追加のマニフェスト ファイル」を `HighDPI.manifest`、「DPI 認識」を「モニターごとの高い DPI 認識」に設定
   - このプロジェクトで用意している `HighDPI.manifest` は、標準的な PerMonitorV2 設定から Visual Studio による自動生成と重複する `<dpiAware>` タグを除いたものです。また、これにより Win32, x64 で `Unrecognized Element "dpiAwareness"` という警告が出ますが XP 用の古いビルドツールを使っているためで、埋め込みは正常に行われるので問題ありません。
1. 「リソース」->「全般」->「カルチャ」を「日本語 (日本) (0x411) (/l 0x0411)」に変更
1. Debug/Release のそれぞれについて、「C/C++」->「コード生成」->「ランタイム ライブラリ」を Debug の方は「マルチスレッド デバッグ (/MTd)」、Release の方は「マルチスレッド (/MT)」に変更
   - DLL の静的リンクを行い、実行環境に再頒布可能パッケージを不要にする。

## 補足
ソースコードは **main.cpp**、リソーススクリプトは **resource.rc**、アイコンは **app.ico**、マニフェストは **HighDPI.manifest** です。Visual Studio であれば、**PrimeFactorization.sln** を開くことでプログラムの編集及びビルドが可能です。

実行可能バイナリは、x86 は **Win32**、AMD64 は **x64**、AArch32 は **ARM**、AArch64 は **ARM64** フォルダ内の、**Debug** フォルダにデバッグビルドが、**Release** フォルダにリリースビルドが **PF_(対象 CPU アーキテクチャ名).exe** として生成されます。
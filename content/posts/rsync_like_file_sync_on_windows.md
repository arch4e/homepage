+++
title = "[Tips] Windowsでrsyncのようなファイル同期を行う"
date = 2022-08-18

[taxonomies]
tags = ["Tips", "Windows"]
+++


## 方法

* WSL上にrsyncをインストールする
* robocopyを使用する <- 解説するのはコレ
* その他のツールを利用する


## robocopyを使用したファイル同期

`robocopy` はWindowsに最初からインストールされており、PowerShellかCommand Prompt(cmd.exe)で使用可能。

1. Windowsキーを押下
1. "powershell" or "cmd" と入力し、PowerShellかCommand Promptを開く
1. 後述のコマンドを実行する


## robocopy 実行例

次のコマンドを実行することでフォルダのミラーリングができる。  
つまり、 `<src>` に存在して `<dest>` に存在しないファイルのみ転送され、 `<dest>` のみに存在するファイルは削除される。

コマンド例における `<src>` は転送元のフォルダパス, `<dest>` は転送先のフォルダパスに置き換える。  
ローカルのフォルダ同士でファイルを転送する場合はPowerShell/cmd.exeの画面にフォルダをD&Dすると自動でパスが入力される。

ミラーリング以外にも転送オプションがいくつか存在しているが、本記事では紹介を省略する。  
実際のオプションはページ末尾のドキュメントで確認してほしい。  
なお、日本語ドキュメントは翻訳精度がよくないため、英語に抵抗がなければ英語ドキュメントの利用をおすすめする。

```
robocopy /MIR <src> <dest>
```

## 参考資料

* [robocopy - Microsoft Docs (en)](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy)
* [robocopy - Microsoft Docs (ja)](https://docs.microsoft.com/ja-jp/windows-server/administration/windows-commands/robocopy)


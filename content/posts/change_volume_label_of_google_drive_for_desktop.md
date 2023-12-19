+++
title = "[Tips] Desktop版 Google Driveのボリュームラベルを変更する"
date = 2023-04-24

[taxonomies]
tags = ["Tips", "Windows"]
+++


## 目的

下記のように、Desktop版のGoogle Driveを使用した際にボリュームラベルがメールアドレスになってしまうケースがある。

これらのラベルを任意のものに変える方法を記載する。  
なお、本記事で紹介する方法はpowerで解決しているため、他によい方法があればぜひ教えて欲しい。

{{ image(src="https://fs.arch4e.com/blog/change_volume_label_of_google_drive_for_desktop/before.png", alt="screenshot - explorer", position="center", style="height: 30vh") }}


## 方法

1. メモ帳などのテキストエディタでファイルを作成し、  
   次の内容を記述する
   - "H/J"はGoogle Driveに割り当てられているDrive Letterを指定する
   - "Google Drive 1/2"は任意の名前にする
     ```
     label H: "Google Drive 1"
     label J: "Google Drive 2"
     ```
1. `<任意の名前>.bat` でファイルを保存する
1. ダブルクリックで実行してみる
   - 警告が出た場合は"詳細"をクリックし、実行ボタンを押す
1. 動作確認後、ファイルをスタートアップフォルダに移動する
   - スタートアップフォルダ:  
     `%appdata%\Roaming\Microsoft\Windows\Start Menu\Programs\Startups`
   - 日本語環境の場合は `Start Menu` 以降が日本語になっている
1. 再起動してボリュームラベルが変わっていれば成功

{{ image(src="https://fs.arch4e.com/blog/change_volume_label_of_google_drive_for_desktop/after.png", alt="screenshot - explorer", position="center", style="height: 30vh") }}


## 補足

再起動後もボリュームラベルが変わらない場合はタイムアウト設定  
（下記コードの一行目）を追加してみるとうまくいくかもしれない。

```
timeout /t 10
label H: "Google Drive 1"
label J: "Google Drive 2"
```


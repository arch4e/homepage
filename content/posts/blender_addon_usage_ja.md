+++
title = "[Blender] 自作アドオンの使用方法説明"
date = 2023-12-01

[taxonomies]
tags = ["3DCG", "Blender", "Python"]
+++

## 概要

If you find English easier to understand, please check the [English version](https://arch4e.com/posts/blender-addon-usage-en/).
Translation is done using GPT-3, and while efforts are made for accuracy, there may be some inaccuracies.

自作アドオンの使用方法をまとめます。
動作環境などはそれぞれのアドオンごとに記載します。
最新リリースに完全対応できているわけではないため、記載バージョン以降の変更はGitHubのリリースノート等をご参照ください。

問い合わせやツール作成のご依頼については記事末尾をご確認ください。

## アドオン使用方法

### bpgrip

> Download: [GitHub](https://github.com/arch4e/bpgrip) / [Booth](https://arch4e.booth.pm/items/4566568)

#### 主な機能

* ブラシ系ツールのBlending Mode切り替えをキーボードショートカットに登録できる

#### 使用方法（Weight Paint Modeを想定）

> 動作検証: bpgrip v2.0 / Blender 3.6.5
> 記事執筆時点の最新版であるBlender 4.0でも動くと思います。

1. 画面左上の `Edit >> Preference` から `Keymap` に移動
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/1.png", alt="screenshot", position="center", style="height: 30vh") }}
1. 少し下にスクロールし、 `3D View >> Weight Paint >> Weight Paint (Global)` (※1)へ移動
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/2.png", alt="screenshot", position="center", style="height: 30vh") }}
1. `Add New` を押し、三角のマークを押す
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/3.png", alt="screenshot", position="center", style="height: 30vh") }}
1. `none` となっている箇所を `bpgrip.set_bbmode` に変更し、キーボードショートカットの割り当てを行う
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/4.png", alt="screenshot", position="center", style="height: 30vh") }}
1. `bl_mode` の項目を `Draw` (※2)に、 `bb_mode` の項目をショートカット登録したいツール(※3)にする
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/5.png", alt="screenshot", position="center", style="width: 50vw") }}
1. 以上で、キーボードショートカットからBrush Blending Modeを切り替え可能になる

```
※1,2: Sculpt ModeのブラシなどはGitHubの"readme.md"にある"Keymap"の項目を参照してください。
       https://github.com/arch4e/bpgrip
※3: "Add"ブラシは"ADD"など、基本的には名前の通りですが、Blenderの"Scripting"タブの左下にあるインスペクタに表示される操作ログからも確認できます。
     また、※2のbl_modeも同様のログから確認可能です。
```

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/bpgrip/6.png", alt="screenshot", position="center", style="height: 30vh") }}

### lab04

> Download: [GitHub](https://github.com/arch4e/lab04)

#### 主な機能

※一つのアドオンにするほどでもない、細かな機能を実装しているアドオンのため比較的頻繁に機能の統廃合を行っている

* Mesh名をObject名と一致させる
* 頂点グループの一括リネームを行う（正規表現指定）
* `UV Sync Selection` の切り替えにおいて、選択頂点以外のUV Mapも表示されたまま無効化できる
* 頂点グループへのWeight割り当て補助
* ~~ChatGPTへ指示することで簡単な作業を行う（ネタ機能）~~ v0.5.0にて廃止

#### 使用方法

##### Mesh名をObject名と一致させる

1. 3D Viewの右にある `Tool` エリアから `Lab. 04` パネルを開く
1. `sync name (obj. to mesh)` ボタンを押すと全てのメッシュの名前がObjectの名前で上書きされる

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/1.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/2.png", alt="screenshot", position="center", style="width: 50vw") }}

##### 頂点グループの一括リネームを行う（正規表現指定）

1. Objectを選択し、アクティブにしておく
1. 検索パターンと置換後の文字列を入力し、 `Rename V-Groups` を押す
   * パターンは正規表現で解釈されるため、 `^` （正規表現において"行頭"を意味する）を指定してPrefixを付与するといった使い方もできる

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/1.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/sync_name/2.png", alt="screenshot", position="center", style="width: 50vw") }}

##### `UV Sync Selection` の切り替えにおいて、選択頂点以外のUV Mapも表示されたまま無効化できる

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/1.png", alt="screenshot", position="center", style="width: 50vw") }}

1. 画面左上の `Edit >> Preference` から `Keymap` に移動
1. 少し下にスクロールし、 `Image >> UV Editor >> UV Editor (Global)` に移動
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/2.png", alt="screenshot", position="center", style="height: 30vh") }}
1. 前述の `bpgrip` と同様に `Add New` から `lab04.switch_uv_select_sync` にキーボードショートカットを割り当てる
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/switch_uv_select_sync/3.png", alt="screenshot", position="center", style="height: 30vh") }}
1. `UV Editing` タブで設定したキーボードショートカットを実行するとUV Sync Selectionが切り替わる

##### 頂点グループへのWeight割り当て補助

`Item` エリアに頂点グループ割当用のパネルが表示される。
基本的にはデフォルトの機能と同じだが、Weightの値をマウスカーソルで指定可能になっている。
※連続実行を想定して、Weightの適用は `Assign` ボタンを押したタイミングに行う仕様にしている

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/lab04/weight_assign_util/1.png", alt="screenshot", position="center", style="width: 50vw") }}

### shake

> Download: [GitHub](https://github.com/arch4e/shake/releases) / [Booth](https://arch4e.booth.pm/items/4843394)

#### 主な機能

* CSVからShape Keysを作成
* アクティブなShape Keyをリスト上の任意の位置へ移動
* Prefixを参照して整列（AtoZではなく実行時点の並び順を基準にする）
* Prefix単位の並べ替え
* Shape Keysの転写

#### 使用方法

##### CSVからShape Keysを作成

1. Blender Projectに作成したいShape Keysの一覧を読み込む
   * 区切り文字は `改行(LF or CR+LF)` と `,`
   * Scripting Tabから作成しても良い
1. 3D Viewの `ShaKe` エリアにある `Create Shape Keys from CSV` を開く
1. プルダウンメニューからCSVファイルを選択する
   {{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/create_shape_keys_from_csv/1.png", alt="screenshot", position="center", style="width: 50vw") }}
1. Shape Keysを作成するObjectを選択し、 `create shape key from csv` を押すとShape Keysが作成される

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/create_shape_keys_from_csv/2.png", alt="screenshot", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/create_shape_keys_from_csv/3.png", alt="screenshot", position="center", style="width: 50vw") }}

##### アクティブなShape Keyをリスト上の任意の位置へ移動

1. Objectを選択し、アクティブにしておく
1. 移動させたいShape Keyを選択し、右のメニューから `Move Shape Key` にマウスカーソルを合わせる
1. リストで選んだShape Keyの下にアクティブなShape Keyが移動する

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/move_shape_key/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Prefixを参照して整列（AtoZではなく実行時点の並び順を基準にする）

1. Objectを選択し、アクティブにしておく
1. 右のメニューから `Align by Prefix` を押すとPrefixごとに並べ替える
   * AtoZではなく、Prefixが存在する順番で上に詰める動作をする

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/align_by_prefix/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Prefix単位の並べ替え

1. 3D Viewの `ShaKe` エリアにある `Order Management` を開く
1. パネル右のボタンからPrefixのリストを作成する
   * 矢印のアイコンをクリックするとアクティブなObjectから自動で情報を取得する（上書きのため注意）
   * `+` アイコンで手動作成も可能
   * `MAIN_sub_` のように多段になっていてもよい
     * 並べ替えの際は頭からの最長一致で判定される
1. `Rearrange` ボタンを押すとリストの順番でShape Keysが整列される
   * 先頭からの一致を見ているため、区切り文字( `_` )は必須でない
     * 例: `K` なら `Key~` にもマッチする
   * リストのどれにもマッチしないShape Keyはスキップされる

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/reorder/1.png", alt="screenshot", position="center", style="width: 50vw") }}

##### Shape Keysの転写

1. 3D Viewの `ShaKe` エリアにある `Shape Keys Selector` , `Transcribe Shape Keys` を開く
1. `Shape Keys Selector` でプロジェクトか、選択したObjectに存在するShape Keysのうち、他Objectにも作成したいものを選択する
   * 日本語UIで設定の `インターフェイス >> 新規データ` にチェックが入っている場合、Object指定のオプションは存在しない
     * 日本語を含む（正確にはNon-ASCII）場合に正常に処理されないため
1. `Transcribe Shape Keys` でShape Keysを転写（作成）したいObjectを選択する
1. `transcribe` を押すと選択したObjectに対象のShape Keysが作成される

{{ image(src="https://fs.arch4e.com/blog/blender_addon_usage/shake/transcribe/1.png", alt="screenshot", position="center", style="width: 50vw") }}

### stree

> Download: [GitHub](https://github.com/arch4e/stree) / [Booth](arch4e.booth.pm/items/4694745)

#### 主な機能

* Objectを別コレクションにバックアップする
* バックアップのプレビュー・復元を簡単に行う

#### 使用方法

不特定多数の利用を想定していないため省略

## お問い合わせ、アドオン作成・エラー対応のご依頼について

リリース済みアドオンに関するご質問や、アドオン作成の制作依頼、技術支援（他アドオンのエラーに関するヒアリング・簡単なパッチ作成など）のご相談は[本ページトップ](https://arch4e.com)に記載しているSNSにてご連絡ください。
アドオン作成や技術支援については対象のアドオンや予算等も併せてご提示ください。

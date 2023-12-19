+++
title = "[Unity/VRM] VRM設定補助用Unity Editor拡張 使用方法"
date = 2023-12-18

[taxonomies]
tags = ["3DCG", "VRM", "Unity"]
+++

## 概要

VRM 0.X系のセットアップ補助を目的としたUnity Editor拡張であるUtil4の試用方法を解説します。
問い合わせやツール作成のご依頼については記事末尾をご確認ください。

## 導入方法（汎用）

1. `Assets` フォルダ内に `Editor` フォルダを作成する
2. GitHub等からファイルをダウンロードし、 `Editor` フォルダに配置する
3. メニューの `Tools` から導入したツールのウィンドウを開く

## 使用方法

### Util4

> Download: [GitHub](https://github.com/arch4e/util4_for_unity)

軽い気持ちでポンっと出したら想像以上にリアクションをいただいておりおどろいています。
機能が増えてきたらツール名を考え直すかもしれません……

#### 主な機能

* モデルデータに存在するBlend Shapeを読み取り、Blend Shape Clipを自動作成する

#### 使用方法

> 動作検証: Unity 2021.3.32f1 / [UniVRM v0.115.0](https://github.com/vrm-c/UniVRM/releases)
> v0.1.1ではUnity 2019.4.31.f1&UniVRM v0.99.0でも動作することを確認しています。

デモ: https://x.com/arch4e_N/status/1736706173951496411

1. 各フィールドにGame Objectをセットする
   * Avatar Prefab: Blend Shape Clipを作成するAvatarのPrefab
   * Source Mesh: `Blend Shape Source` オプションを `Mesh` にしている場合に使用する（詳細後述）
   * Blend Shape Object: Blend Shapeの管理オブジェクトらしきもの↓（変更していなければ `BlendShape` ）
     {{ image(src="https://fs.arch4e.com/blog/vrm_setup_utility_usage/util4/blend_shape_object.png", alt="screenshot", position="center", style="width: 30vw") }}
   * Save Folder: Blend Shape Clipの保存先
1. `Create Blend Shape Clips` ボタンを押すとAvatarに存在するBlend Shapeと同じ名前でBlend Shape Clipが生成され、対応するBlend Shapeを100にする
   {{ image(src="https://fs.arch4e.com/blog/vrm_setup_utility_usage/util4/field.png", alt="screenshot", position="center", style="width: 30vw") }}

#### オプション

* Source Mesh
  * Prefab: Prefabに存在するすべてのMeshで存在するBlend ShapeをClip作成対象とします
  * Mesh: 指定したMeshのみに存在するBlend ShapeをClip作成対象とします
* Exist Blend Shape Clip
  * Skip: 同じ名前のBlend Shape Clipが作成済みの場合、読み飛ばして処理を進めます
  * Set Weight to 1: 同じ名前のBlend Shape Clipが作成済みかつBlend Shape未設定かWeightが0の場合、Weightを100にセットします
    * 0以外の値が設定されている場合、意図して編集したものとみなし変更を加えません

## お問い合わせ、エディタ拡張作成・エラー対応のご依頼について

リリース済みのツールに関するご質問は[本ページトップ](https://arch4e.com)に記載しているSNSにてご連絡ください。
有償でのツール作成も受け付けておりますので、ご希望の方は上記の方法にてご相談ください。

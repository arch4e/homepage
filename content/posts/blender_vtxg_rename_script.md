+++
title = "[Blender] Vertex Groupの名前をまとめて変更する"
date = 2022-05-23

[taxonomies]
tags = ["3DCG", "Blender", "Python"]
+++

Vertex Groupは `Batch Rename` の対象に出てこないため今後のためにメモ。

> === 2023/09/18 追記 ===  
> 本ブログの機能は執筆者のアドオン [lab04](https://github.com/arch4e/lab04) に組み込みました。  
> 上記のアドオンでは"Tool"タブ内のパネルから本記事の機能を使用できるようになっています。


## 使い方

1. "Scripting"タブを開く
1. コンソールウィンドウにソースコードをコピーして貼り付ける
1. (… ではなく) >>> と表示されるまでEnterを押下
1. Vertex Groupの名前を変更したいオブジェクトを選択
1. rename_vtx_group(r'変更前', '変更後') の形式で入力しEnterを押下


## ソースコード

```python
import re

def rename_vtx_group(rexex, text):
    for obj in bpy.context.selected_objects:
        for vtx in obj.vertex_groups.values():
            vtx.name = re.sub(rexex, text, vtx.name)
```


## 実行例

```
rename_vtx_group(r'^Left', '')  : "<行頭>Left" を空白に置換
rename_vtx_group(r'$', '.Left') : "<行末>" を ".Left" に置換 ※つまり文字列後ろに ".Left" を追加
```
```
PYTHON INTERACTIVE CONSOLE 3.10.2 (main, Jan 26 2022, 15:26:42) [Clang 13.0.0 (clang-1300.0.29.30)]

Builtin Modules:       bpy, bpy.data, bpy.ops, bpy.props, bpy.types, bpy.context, bpy.utils, bgl, blf, mathutils
Convenience Imports:   from mathutils import *; from math import *
Convenience Variables: C = bpy.context, D = bpy.data

>>> import re
>>> 
>>> def rename_vtx_group(rexex, text):
...     for obj in context.selected_objects:
...         for vtx in obj.vertex_groups.values():
...             vtx.name = re.sub(rexex, text, vtx.name)
...             
>>> rename_vtx_group(r'^Left', '')
>>> rename_vtx_group(r'$', '.Left')
>>> 
```


## 動かしている様子

※動画内のコードと本記事のコードが若干異なり、本記事のコードが正しい。  
ただし、 `context = bpy.context` と実行しておけば動画内のコードでも動く。

[YouTube](https://www.youtube.com/embed/aWqJBLuWBOI)


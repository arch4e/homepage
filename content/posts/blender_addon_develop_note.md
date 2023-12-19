+++
title = "[Blender] アドオン開発個人メモ"
date = 2023-11-11

[taxonomies]
tags = ["Blender", "Python"]
+++


Blenderアドオン開発に関する個人的なメモです。
気になる箇所があればトップページに記載しているSNS等でご指摘ください。


## 命名規則


基本的に[PEP8](https://peps.python.org/pep-0008/)に従うが、クラス名など一部の命名は完全準拠というわけではなさそう。
PEP8の[A Foolish Consistency is the Hobgoblin of Little Minds](https://peps.python.org/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds)に次の一文があるため、Blenderのお作法に合わせるのが適切と判断した。

> In particular: do not break backwards compatibility just to comply with this PEP!

### クラス

標準アドオンのコードや[perplexityを用いた検索結果](https://www.perplexity.ai/search/874cae7e-24c8-458a-83bf-762d921a51e8?s=u)から次の規則で命名することにした。
Property Groupは `HT` , `MT` のようなキーワードが見つからず、[Blender Python APIのexamples](https://projects.blender.org/blender/blender/src/commit/8b4c9b294aff7038812cd7ee04d36a94a6413e67/doc/python_api/examples/bpy.types.PropertyGroup.py)では `MyPropertyGroup` となっている。
しかし、以下の理由から `PG` をキーワードとして使用することにした。

- Property GroupだけPrefixが存在しない場合、register/unregisterの対象とすべきクラスとそうでないクラスが名前から判別できない
- 適切な命名規則がわかった時に置換すべきクラスを見つけやすい
- 上記のメリットとBlenderのお作法から外れることを天秤にかけたとき、前者を重要視した

その他、 `register` 実行時にBlenderコンソールに出力されるwarningも考慮している。

```python
# Header: HT
class <ADDON_NAME>_HT_<header>_<name>(bpy.types.Header):
    bl_idname = '<ADDON_NAME>_HT_<header>_<name>'
    # example: https://docs.blender.org/api/current/bpy.types.Header.html

# Menu: MT
class <ADDON_NAME>_MT_<menu>_<name>(bpy.types.Menu):
    bl_idname = '<ADDON_NAME>_MT_<menu>_<name>'
    # example: https://docs.blender.org/api/current/bpy.types.Menu.html

# Operator: OT
class <ADDON_NAME>_OT_<operator>_<name>(bpy.types.Operator):
    bl_idname = '<ADDON_NAME>.<operator>_<name>'
    # example: https://docs.blender.org/api/current/bpy.types.Operator.html

# Panel: PT
class <ADDON_NAME>_PT_<panel>_<name>(bpy.types.Panel):
    bl_idname = 'VIEW3D_PT_<ADDON_NAME>_<panel>_<name>'
    # example: https://docs.blender.org/api/current/bpy.types.Panel.html

# UI List: UL
class UI_UL_<ADDON_NAME>_<ui>_<list>_<name>(bpy.types.UIList):
    # example: https://docs.blender.org/api/current/bpy.types.UIList.html

# Property Group: PG
class <ADDON_NAME>_PG_<property>_<group>_<name>():
    # example: https://docs.blender.org/api/current/bpy.types.PropertyGroup.html
```


## ファイル分割

`operator` , `property` , `ui` で分割している。
`__init__.py` でクラスを登録していくが、その順番がパネルの表示順になるようだ。

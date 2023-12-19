+++
title = "[Blender] numpyのエラーでプライグインを有効化できない場合の対処方法"
date = 2022-05-23

[taxonomies]
tags = ["3DCG", "Blender", "Python"]
+++


## 概要

Object: Grease Pencil Tools などのプラグインを有効化しようとした場合にnumpyのエラーが出る場合の対処を解説する。  
※検索用にエラー全文をテキストで貼っているが、読み飛ばしてよい

{{ image(src="https://fs.arch4e.com/blog/blender_numpy_error/error.png", alt="screenshot - blender", position="center", style="height: 30vh") }}

```python
Traceback (most recent call last):
  File "C:\Users\0x04\AppData\Roaming\Blender Foundation\Blender\3.1\scripts\modules\numpy\core\__init__.py", line 22, in <module>
    from . import multiarray
  File "C:\Users\0x04\AppData\Roaming\Blender Foundation\Blender\3.1\scripts\modules\numpy\core\multiarray.py", line 12, in <module>
    from . import overrides
  File "C:\Users\0x04\AppData\Roaming\Blender Foundation\Blender\3.1\scripts\modules\numpy\core\overrides.py", line 7, in <module>
    from numpy.core._multiarray_umath import (
ModuleNotFoundError: No module named 'numpy.core._multiarray_umath'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<blender_console>", line 1, in <module>
  File "C:\Users\0x04\AppData\Roaming\Blender Foundation\Blender\3.1\scripts\modules\numpy\__init__.py", line 150, in <module>
    from . import core
  File "C:\Users\0x04\AppData\Roaming\Blender Foundation\Blender\3.1\scripts\modules\numpy\core\__init__.py", line 48, in <module>
    raise ImportError(msg)
ImportError: 

IMPORTANT: PLEASE READ THIS FOR ADVICE ON HOW TO SOLVE THIS ISSUE!

Importing the numpy C-extensions failed. This error can happen for
many reasons, often due to issues with your setup or how NumPy was
installed.

We have compiled some common reasons and troubleshooting tips at:

    https://numpy.org/devdocs/user/troubleshooting-importerror.html

Please note and check the following:

Please note and check the following:

  * The Python version is: Python3.10 from "C:\Users\0x04\Downloads\blender-3.1.2-windows-x64\3.1\python\bin\python.EXE"
  * The NumPy version is: "1.21.5"

and make sure that they are the versions you expect.
Please carefully study the documentation linked above for further help.

Original error was: No module named 'numpy.core._multiarray_umath'
```


## 結論

次のフォルダに移動し、 numpy と名のつくフォルダを全て削除してBlenderを再起動すればよい。  
スクリーンショットの例では numpy 及び numpy-1.21.5.dist.info が該当する。

なお、パスはWindows版のものを記述している。  
mac版やLinux版を利用している場合は公式ドキュメントを参照して読み替えてほしい。  
参考: [Blender’s Directory Layout](https://docs.blender.org/manual/en/latest/advanced/blender_directory_layout.html)

```
C:\Users\<user name>\AppData\Roaming\Blender Foundation\Blender\<version>\scripts\modules
※<user name> : ユーザー名に置き換え
※<version>   : 使用しているBlenderのバージョン
```

{{ image(src="https://fs.arch4e.com/blog/blender_numpy_error/modules.png", alt="screenshot - explorer", position="center", style="height: 30vh") }}


## 解説


### エラーの内容

今回のエラーはnumpy(※1)のバージョンが古い場合に表示されるため、numpyのバージョンを上げれば解決する。
ただし、BlenderはPython本体とライブラリを内包しておりPCに直接numpyをインストールしても解決しない。

※1 numpy とはPythonで数値計算を扱うライブラリである。  
　 BlenderはPythonを使用して開発されているためしばしば名前が出てくる。


### 消したファイルの内容

`%USERPROFILE%\AppData\Roaming\Blender Foundation\Blender\` には、過去にインストールしていたBlenderの設定ファイルなどが配置されている。

このフォルダの存在によってBlenderのアップデート後も旧バージョンから設定を引き継いで使用することができるが、  
今回はここに古いnumpyが残ってしまっていたためプライグインの有効化でエラーとなっていた。

古いファイルを消すことで新しいバージョンのnumpyを参照するようになり、プラグインが有効化できるようになる。  
ただし、古いnumpyでしか使用できないプラグインを使用している場合はそちらが動かなくなるケースも想定されるため、  
使用しているプラグインが多い場合はファイル削除ではなく名前変更などで対処し、問題が起きないことを確認してから削除するようにしたほうがよい。


### 最後に

前述の通り `%USERPROFILE%\AppData\Roaming\Blender Foundation\Blender\` には過去のバージョンから引き継がれている  
ファイル群が保存されており、このフォルダはBlenderを再インストールしても消えない。

そのため、Blenderを再インストールしても解決しないような問題に遭遇した場合はこのフォルダの存在を思い出してみると解決策が見つかるかもしれない。


+++
title = "[writeup] CpawCTF Level 1"
date = 2020-11-08

[taxonomies]
tags = ["CTF", "writeup"]
+++

---

* [CpawCTF Level 1](https://arch4e.com/posts/writeup-cpawctf-level1/)
* [CpawCTF Level 2](https://arch4e.com/posts/writeup-cpawctf-level2/)
* [CpawCTF Level 3](https://arch4e.com/posts/writeup-cpawctf-level3/)

---


## [Misc] Test Problem

```
[ Challenge ]
この問題の答え（FLAG）は、cpaw{this_is_Cpaw_CTF} です。
下の入力欄にFLAGを入力してSubmitボタンを押して、答えを送信しましょう！

Enjoy CpawCTF!!!!
```

=> `cpaw{this_is_Cpaw_CTF}`


## [Crypto] Classical Cipher

```
[ Challenge ]
暗号には大きく分けて、古典暗号と現代暗号の2種類があります。
特に古典暗号では、古代ローマの軍事的指導者ガイウス・ユリウス・カエサル（英語読みでシーザー）が初めて使ったことから、名称がついたシーザー暗号が有名です。これは3文字分アルファベットをずらすという単一換字式暗号の一つです。
次の暗号文は、このシーザー暗号を用いて暗号化しました。暗号文を解読してフラグを手にいれましょう。

暗号文: fsdz{Fdhvdu_flskhu_lv_fodvvlfdo_flskhu}
```

`cpaw~` で始まる様に文字をずらして解読する。
[CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,23)&input=ZnNkentGZGh2ZHVfZmxza2h1X2x2X2ZvZHZ2bGZkb19mbHNraHV9) などが役にたつ。

=> `cpaw{Caesar_cipher_is_classical_cipher}`


## [Reversing] Can you execute ?

```
[ Challenge ]
拡張子がないファイルを貰ってこのファイルを実行しろと言われたが、どうしたら実行出来るのだろうか。
この場合、UnixやLinuxのとあるコマンドを使ってファイルの種類を調べて、適切なOSで実行するのが一般的らしいが…
問題ファイル：exec_me
```

ファイルをDLし、実行権限を付与して走らせる。

```
❯ file exec_me
exec_me: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=663a3e0e5a079fddd0de92474688cd6812d3b550, not stripped

❯ chmod +x exec_me

❯ ./exec_me
cpaw{Do_you_know_ELF_file?}
```

=> `cpaw{Do_you_know_ELF_file?}`


## [Misc] Can you open this file ?

```
[ Challenge ]
このファイルを開きたいが拡張子がないので、どのような種類のファイルで、どのアプリケーションで開けば良いかわからない。
どうにかして、この拡張子がないこのファイルの種類を特定し、どのアプリケーションで開くか調べてくれ。
問題ファイル：open_me
```

`file` コマンドで確認するとWordファイルだとわかる。
LibreOfficeやPages(macOS)などの適当なソフトで読み込めばOK。

```
❯ file open_me
open_me: Composite Document File V2 Document, Little Endian, Os: Windows, Version 10.0, Code page: 932, Author: v, Template: Normal.dotm, Last Saved By: v, Revision Number: 1, Name of Creating Application: Microsoft Office Word, Total Editing Time: 28:00, Create Time/Date: Mon Oct 12 04:27:00 2015, Last Saved Time/Date: Mon Oct 12 04:55:00 2015, Number of Pages: 1, Number of Words: 3, Number of Characters: 23, Security: 0
```

```
❯ mv open_me open_me.docx
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/can_you_open_this_file/flag.png", alt="screenshot", position="center", style="width: 40vw") }}

=> `cpaw{Th1s_f1le_c0uld_be_0p3n3d}`


## [Web] HTML Page

```
[ Challenge ]
HTML(Hyper Text Markup Language)は、Webサイトを記述するための言語です。
ページに表示されている部分以外にも、ページをより良くみせるためのデータが含まれています。
次のWebサイトからフラグを探して下さい。
http://q9.ctf.cpaw.site
```

curlで取ってきたコードをgrepする。

```
❯ curl http://q9.ctf.cpaw.site/ | grep cpaw
<meta name="description" content="flag is cpaw{9216ddf84851f15a46662eb04759d2bebacac666}">
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/html_page/site.png", alt="screenshot", position="center", style="width: 40vw") }}

=> `cpaw{9216ddf84851f15a46662eb04759d2bebacac666}`


## [Forensics] River

```
[ Challenge ]
JPEGという画像ファイルのフォーマットでは、撮影時の日時、使われたカメラ、位置情報など様々な情報(Exif情報)が付加されることがあるらしい。
この情報から、写真に写っている川の名前を特定して欲しい。
問題ファイル： river.jpg
FLAGの形式は、"cpaw{river_name}"
例：隅田川 → cpaw{sumidagawa}
```

exiftool(https://exiftool.org/) を使用してGPS Positionを確認し、対応する川を調べる。

```
❯ exiftool river.jpg
ExifTool Version Number         : 11.88
File Name                       : river.jpg
Directory                       : .
File Size                       : 414 kB
File Modification Date/Time     : 2020:11:07 23:13:40+09:00
File Access Date/Time           : 2020:11:07 23:15:11+09:00
File Inode Change Date/Time     : 2020:11:07 23:13:40+09:00
File Permissions                : rwxrwxrwx
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Big-endian (Motorola, MM)
Make                            : Sony
Camera Model Name               : SO-01G

...

Create Date                     : 2015:09:14 12:50:38.190234
Date/Time Original              : 2015:09:14 12:50:38.190234
Modify Date                     : 2015:09:14 12:50:38.190234
GPS Latitude                    : 31 deg 35' 2.76" N
GPS Longitude                   : 130 deg 32' 51.73" E
Date/Time Created               : 2015:09:14 12:50:38
Digital Creation Date/Time      : 2015:09:14 12:50:38
Focal Length                    : 4.6 mm
GPS Position                    : 31 deg 35' 2.76" N, 130 deg 32' 51.73" E
Light Value                     : 14.0
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/river/river_GoogleMap.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{koutsukigawa}`


## [Network] pcap

```
[ Challenge ]
ネットワークを流れているデータはパケットというデータの塊です。
それを保存したのがpcapファイルです。
pcapファイルを開いて、ネットワークにふれてみましょう！
pcapファイル
```

[WireShark](https://www.wireshark.org/) などのツールでファイルを開く。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/pcap/pcap.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{gochi_usa_kami}`


## [Crypto] HashHashHash!

```
[ Challenge ]
ハッシュ関数とは、値を入れたら絶対にもとに戻せないハッシュ値と呼ばれる値が返ってくる関数です。
ですが、レインボーテーブルなどでいくつかのハッシュ関数は元に戻せてしまう時代になってしまいました。
以下のSHA1というハッシュ関数で作られたハッシュ値を元に戻してみてください！（ヒント：googleで検索）
e4c6bced9edff99746401bd077afa92860f83de3
フラグは
cpaw{ハッシュを戻した値}
です。
```

[Hash Toolkit]https://hashtoolkit.com/) で元に戻す。
ちなみに、検索エンジンにて `e4c6bced9edff99746401bd077afa92860f83de3 hash` と検索しても類似ツールによるでコード結果が複数件ヒットした。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/hashhashhash/hash.png", alt="screenshot", position="center") }}

=> `cpaw{Shal}`


## [PPC] 並べ替えろ!

```
[ Challenge ]
下にある配列の中身を大きい順に並べ替えて、くっつけてcpaw{並べ替えた後の値}をフラグとして提出してください。
例：もし配列｛1,5,3,2｝っていう配列があったら、大きい順に並べ替えると｛5,3,2,1}となります。
そして、フラグはcpaw{5321}となります。
同じようにやってみましょう（ただし量が多いので、ソートするプログラムを書いたほうがいいですよ！）
[15,1,93,52,66,31,87,0,42,77,46,24,99,10,19,36,27,4,58,76,2,81,50,102,33,94,20,14,80,82,49,41,12,143,121,7,111,100,60,55,108,34,150,103,109,130,25,54,57,159,136,110,3,167,119,72,18,151,105,171,160,144,85,201,193,188,190,146,210,211,63,207]
```

```
❯ python3
Python 3.8.5
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> list = [15,1,93,52,66,31,87,0,42,77,46,24,99,10,19,36,27,4,58,76,2,81,50,102,33,94,20,14,80,82,49,41,12,143,121,7,111,100,60,55,108,34,150,103,109,130,25,54,57,159,136,110,3,167,119,72,18,151,105,171,160,144,85,201,193,188,190,146,210,211,63,207]
>>> list.sort(reverse=True)
>>> ''.join(map(str, list))
'2112102072011931901881711671601591511501461441431361301211191111101091081051031021009994938785828180777672666360585755545250494642413634333127252420191815141210743210'
```

=> `cpaw{2112102072011931901881711671601591511501461441431361301211191111101091081051031021009994938785828180777672666360585755545250494642413634333127252420191815141210743210`


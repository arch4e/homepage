+++
title = "[writeup] CpawCTF Level 2"
date = 2020-11-08

[taxonomies]
tags = ["CTF", "writeup"]
+++

---

* [CpawCTF Level 1](https://arch4e.com/posts/writeup-cpawctf-level1/)
* [CpawCTF Level 2](https://arch4e.com/posts/writeup-cpawctf-level2/)
* [CpawCTF Level 3](https://arch4e.com/posts/writeup-cpawctf-level3/)

---


## [Stego] 隠されたフラグ

```
[ Challenge ]
以下の画像には、実はフラグが隠されています。
目を凝らして画像を見てみると、すみっこに何かが…!!!!
フラグの形式はcpaw{***}です。フラグは小文字にしてください。
stego100.jpg
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/hidden_flag/steg10.jpg", alt="screenshot", position="center") }}

画像の左上、右下に点と線がプロットされている。
これらをモールス信号として扱い、 [Morse Decoder](https://morsedecoder.com/) などのツールでデコードする。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/hidden_flag/sign_ul.png", alt="screenshot", position="center", style="padding-bottom: 1rem;") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/hidden_flag/sign_lr.png", alt="screenshot", position="center") }}

```
// Upper Left
-.-. .--. .- .-- .... .. -.. -.. . -. ..--.-

// Lower Right
-- . ... ... .- --. . ---... -.--.-

// Full Code
-.-. .--. .- .-- .... .. -.. -.. . -. ..--.- -- . ... ... .- --. . ---... -.--.-
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/hidden_flag/morse_decoder.png", alt="screenshot", position="center") }}

=> `cpaw{hidden_message:)}`


## [Web] Redirect

```
[ Challenge ]
このURLにアクセスすると、他のページにリダイレクトされてしまうらしい。
果たしてリダイレクトはどのようにされているのだろうか…
http://q15.ctf.cpaw.site
```

`http://q15.ctf.cpaw.site/` にアクセスすると `http://q9.ctf.cpaw.site/` に転送される。
`curl` でヘッダを見ると `X-Flag` ヘッダにflagがある。

```
❯  curl -v http://q15.ctf.cpaw.site
*   Trying 118.27.110.77:80...
* Connected to q15.ctf.cpaw.site (118.27.110.77) port 80 (#0)
> GET / HTTP/1.1
> Host: q15.ctf.cpaw.site
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 302 Found
< Server: nginx
< Date: Sun, 24 Sep 2023 03:16:16 GMT
< Content-Type: text/html; charset=UTF-8
< Content-Length: 0
< Connection: keep-alive
< X-Flag: cpaw{4re_y0u_1ook1ng_http_h3ader?}
< Location: http://q9.ctf.cpaw.site
<
* Connection #0 to host q15.ctf.cpaw.site left intact
```

=> `cpaw{4re_y0u_1ook1ng_http_h3ader?}`


## [Network+Forensic] HTTP Traffic

```
[ Challenge ]
HTTPはWebページを閲覧する時に使われるネットワークプロトコルである。
ここに、とあるWebページを見た時のパケットキャプチャファイルがある。
このファイルから、見ていたページを復元して欲しい。
http_traffic.pcap
```

WireSharkでデータを回収し、ブラウザで開く。
ボタンをクリックするとフラグを得られる。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/http_traffic/pcap.png", alt="screenshot", position="center", style="height: 40vh") }}

```
❯ tree q16 
q16
├── img
│   └── image.jpg
├── js
│   └── button2.js
└── network100.html
2 directories, 3 files
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/http_traffic/network100.png", alt="screenshot", position="center", style="height: 40vh; padding-bottom: 1rem;") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/http_traffic/flag.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{Y0u_r3st0r3d_7his_p4ge}}`


## [Recon] Who am I ?

```
[ Challenge ]
僕(twitter:@porisuteru)はスペシャルフォース2をプレイしています。
とても面白いゲームです。
このゲームでは、僕は何と言う名前でプレイしているでしょう！
フラグはcpaw{僕のゲームアカウント名}です。
```

[Google](https://www.google.com/search?q=%E3%82%B9%E3%83%9A%E3%82%B7%E3%83%A3%E3%83%AB%E3%83%95%E3%82%A9%E3%83%BC%E3%82%B92+%40porisuteru&oq=%E3%82%B9%E3%83%9A%E3%82%B7%E3%83%A3%E3%83%AB%E3%83%95%E3%82%A9%E3%83%BC%E3%82%B92) でTwitterの投稿を確認し回答。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/who_am_i/tweet.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{parock}`


## [Forensic] leaf in forest

```
[ Challenge ]
このファイルの中にはフラグがあります。探してください。
フラグはすべて小文字です！

file
```

`file` コマンドではpcapとなっているが、中身はテキストファイル。
`lovelive!` という文字が続いており、途中で別の文字が混ざっている箇所がある。

```
❯ file misc100
misc100: pcap capture file, microsecond ts (little-endian) - version 0.0 (linktype#1768711542, capture length 1869357413)

❯ cat misc100
lovelive!lovelive!lovelive!lovelive!lovelive!lovelive!lovelive!...
```

ひとまず `lovelive!` 以外の文字列を飛ばしてファイルを眺めるとフラグに関係がありそうな文字の塊がある。
それだけを取り出して小文字に直すとflagを得られる。

```
ÔÃ²¡^@^@^@^@^@^@^@<81><93>^A^B^Ce!CCCelive!lovelivPPPovelive!loveAAAe!lovWWWve!{{{elive!loveliMMMelive!lovelGGG!lovelivRRRovelive!lEEE    live!PPPelive!}}}
```

```
:%s/lovelive!//g
```

```
CCCPPPAAAWWW{{{MMMGGGRRREEEPPP}}}
```

```
cpaw{mgrep}
```

=> `cpaw{mgrep}`


## [Misc] Image!

```
[ Challenge ]
Find the flag in this zip file.
file
```

LibreOfficeなどで開きマスクを外す。

```
❯ file misc100.zip
misc100.zip: OpenDocument Drawing
```

=> `cpaw{It_is_fun__isn't_it?}`


## [Crypto] Block Cipher

```
[ Challenge ]
与えられたC言語のソースコードを読み解いて復号してフラグを手にれましょう。

暗号文：cpaw{ruoYced_ehpigniriks_i_llrg_stae}

crypto100.c
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(int argc, char* argv[]){
  int i;
  int j;
  int key = atoi(argv[2]);
  const char* flag = argv[1];
  printf("cpaw{");
  for(i = key - 1; i <= strlen(flag); i+=key) for(j = i; j>= i - key + 1; j--) printf("%c", flag[j]);
  printf("}");
  return 0;
}
```

引数が複合処理のブロックサイズのようです。
おそらく最初は `Your` だと思われるので、`key=4` で実行するとflagが出力されました。

```
❯ gcc crypto100.c

❯ ./a.out ruoYced_ehpigniriks_i_llrg_stae 4
cpaw{Your_deciphering_skill_is_great}
```

=> `cpaw{Your_deciphering_skill_is_great}`


## [Reversing] reversing easy!

```
[ Challenge ]
フラグを出す実行ファイルがあるのだが、プログラム(elfファイル)作成者が出力する関数を書き忘れてしまったらしい…
reverse100
```

```
❯ r2 rev100
 -- Select your character: RBin Wizard, Master Anal Paladin, or Assembly Warrior
[0x080483a0]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for vtables
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x080483a0]> v
```

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/reversing_easy/r2.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{yakiniku!}`


## [Web] Baby's SQLi - Stage 1-

```
[ Challenge ]
困ったな……どうしよう……．
ぱろっく先生がキャッシュカードをなくしてしまったショックからデータベースに逃げ込んでしまったんだ．
うーん，君SQL書くのうまそうだね．ちょっと僕もWeb問作らなきゃいけないから，連れ戻すのを任せてもいいかな？
多分，ぱろっく先生はそこまで深いところまで逃げ込んで居ないと思うんだけども……．
とりあえず，逃げ込んだ先は以下のURLだよ．
一応，報酬としてフラグを用意してるからよろしくね！
https://ctf.spica.bz/baby_sql/
Caution
・sandbox.spica.bzの80,443番ポート以外への攻撃は絶対にしないようにお願いします．
・あなたが利用しているネットワークへの配慮のためhttpsでの通信を推奨します．
```

`SELECT` で抜いてflagを得る。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/babys_sqli_stage1/flag.png", alt="screenshot", position="center", style="height: 40vh") }}

=> `cpaw{palloc_escape_from_stage1;(}`


## [Network] Can you login？

```
[ Challenge ]
古くから存在するネットワークプロトコルを使った通信では、セキュリティを意識していなかったこともあり、様々な情報が暗号化されていないことが多い。そのため、パケットキャプチャをすることでその情報が簡単に見られてしまう可能性がある。
次のパケットを読んで、FLAGを探せ！
network100.pcap
```

WireSharkでTCP Streamを見るとFTPユーザの情報がわかる。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/can_you_login/tcp_stream.png", alt="screenshot", position="center", style="height: 40vh") }}

```
USER cpaw_user
PASS 5f4dcc3b5aa765d61d8327deb882cf99
```

```
❯ ftp
ftp> open 157.7.52.186
Connected to 157.7.52.186.
220 Welcome to Cpaw CTF FTP service.
Name (157.7.52.186:crow): cpaw_user
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> passive
Passive mode on.

ftp> pwd
257 "/"

ftp> ls -a
227 Entering Passive Mode (157,7,52,186,234,119).
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp            42 Jun 18  2019 .
drwxr-xr-x    2 ftp      ftp            42 Jun 18  2019 ..
-rw-r--r--    1 ftp      ftp            39 Sep 01  2017 .hidden_flag_file
-rw-r--r--    1 ftp      ftp            36 Sep 01  2017 dummy
226 Directory send OK.

ftp> get .hidden_flag_file
local: .hidden_flag_file remote: .hidden_flag_file
227 Entering Passive Mode (157,7,52,186,234,125).
150 Opening BINARY mode data connection for .hidden_flag_file (39 bytes).
226 Transfer complete.
39 bytes received in 0.00 secs (149.9446 kB/s)

ftp> exit
221 Goodbye.
```

```
❯ cat .hidden_flag_file 
cpaw{f4p_sh0u1d_b3_us3d_in_3ncryp4i0n}
```

=> `cpaw{f4p_sh0u1d_b3_us3d_in_3ncryp4i0n}`


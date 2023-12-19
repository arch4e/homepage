+++
title = "[writeup] ナゾトキxCTF 入社試験からの脱出"
date = 2022-07-18

[taxonomies]
tags = ["CTF", "writeup"]
+++


{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/share/nazotokiCTF.png", alt="screenshot - nazotokiCTF", position="center", style="height: 30vh") }}


## はじめに

`2022/07/17 13:00 JST ~ 2022/07/18 13:00 JST` で開催されていた[ナゾトキxCTF 入社試験からの脱出](https://ctf.nazotoki.tech/)に参加しました。  
Jeopardy形式のCTFをベースに謎解き要素が盛り込まれており、普段のCTFとは少し違った感覚で楽しめました。

一般的なCTFでは過度な推測(いわゆる"guessing")は排除された作りになっていることが多いですが、nazotokiCTFではその名の通り推理・推測はそれなりに必要でした。  
個人的に推測要素は苦手としています(ひらめきに時間がかかるという意味)が、細かなフォローが多かったためほどよいバランスになっていたと思います。 (半日分の時間を吸い込んだ羊はゆるさん)

全体を通しての感想は記事の最後に記載します。


## Misc - Water elements


### かに座

> 仲間はずれを探せ  
> file: cancer.zip

zipファイルを展開すると複数のファイルが保存されています。

```
~/Downloads/cancer
❯ ls -al
total 104
 480 Jul 17 14:06 .
 256 Jul 17 14:06 ..
2208 Jul  6 17:23 0961db32a59b8a83c1996498f9d1d80e
2276 Jul  6 17:28 397cbf6db9d7ae6906ae420aedc5346c
2036 Jul  6 17:26 44ca0844398b2d010d8cd4a31ddb023d
2268 Jul  6 17:28 4de447a391e32baeb5a52c55aa14467b
2024 Jul  6 17:27 550eadb88a230018bf043d1b6ad15863
2132 Jul  6 17:23 635cbc8a5dc1a528c3b5cb9eecdc1086
2084 Jul  6 17:24 7463543d784aa59ca86359a50ef58c8e
2164 Jul  6 17:26 766cc4dd4d5005652e8514e3513683f8
2212 Jul  6 17:26 7c70e2cb2b4a13c4590f6b15c30385fd
2436 Jul  6 17:27 a0678bcea04dbd6852c219062ab2bb3c
2204 Jul  6 17:26 b9c94e8a87e3647c5a0fa4ff358ecc65
2171 Jul  7 07:10 f0525aafa095ed2665d03681537a70ea
2280 Jul  6 17:27 f8a5c386478fa64f118056b82acc31d2
```

"file"コマンドで見てみるとテキストファイルが1つあり、他はpcapファイルでした。

```
~/Downloads/cancer
❯ file *
0961db32a59b8a83c1996498f9d1d80e: pcapng capture file - version 1.0
397cbf6db9d7ae6906ae420aedc5346c: pcapng capture file - version 1.0
44ca0844398b2d010d8cd4a31ddb023d: pcapng capture file - version 1.0
4de447a391e32baeb5a52c55aa14467b: pcapng capture file - version 1.0
550eadb88a230018bf043d1b6ad15863: pcapng capture file - version 1.0
635cbc8a5dc1a528c3b5cb9eecdc1086: pcapng capture file - version 1.0
7463543d784aa59ca86359a50ef58c8e: pcapng capture file - version 1.0
766cc4dd4d5005652e8514e3513683f8: pcapng capture file - version 1.0
7c70e2cb2b4a13c4590f6b15c30385fd: pcapng capture file - version 1.0
a0678bcea04dbd6852c219062ab2bb3c: pcapng capture file - version 1.0
b9c94e8a87e3647c5a0fa4ff358ecc65: pcapng capture file - version 1.0
f0525aafa095ed2665d03681537a70ea: Unicode text, UTF-8 text
f8a5c386478fa64f118056b82acc31d2: pcapng capture file - version 1.0
```

テキストファイルを開くとフラグが見つかりました。  
=> `イイワケ`

```
~/Downloads/cancer
❯ cat f0525aafa095ed2665d03681537a70ea
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
...
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
Open in Notepad.Open in Notepad.Open in Notepad.Open in Notepad.
nazotokiCTF{イイワケ}
```


### さそり座

> 世界一かわいいうちの犬を紹介します  
> file: scorpio.jpg

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/scorpio/scorpio.jpg", alt="screenshot - scorpio", position="center", style="height: 30vh") }}

犬の画像が添付されていました。(名前は"デイジー"というそうです。)  

ステガノグラフィかと"binwalk"でスキャンしたりフィルタをかけたりしましたが、目に映り込んでいるフラグを読み取るだけでした。  
=> `カクダイ`


### うお座

> みずがめ座からてんびん座に向かうとき、ひみつの鍵が手に入るだろう。水の中に浮かぶ真実を見定めよ。  
> file: password.enc, encrypt-pisces-new.zip

`encrypt-pisces-new.zip` にはパスワードが設定されており、 `password.enc` はエンコードされているためまずは鍵を探します。

この問題は後述する `Web/てんびん座` , `Web/みずがめ座` を利用して鍵を得るようです。  
どちらもWeb問題であり、特にてんびん座はHTTP Headerを利用する問題であることから利用できるパラメータを探します。

最初は `X-Forwarded-For` かなと思いましたが、 `referer` にみずがめ座のURLを指定することで鍵が手に入りました。

```
❯  https https://libra.ctf.nazotoki.tech/ referer:https://aquarius.ctf.nazotoki.tech/
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Length: 2162
Content-Type: text/html; charset=UTF-8
Date: Mon, 18 Jul 2022 01:48:18 GMT
Server: Apache/2.4.38 (Debian)
Vary: Accept-Encoding
X-Powered-By: PHP/7.4.8

<!DOCTYPE html>
<html lang="Ja">
  <head>
    <!-- Document Meta-->
    <meta charset="utf-8"/>
    <!-- Stylesheets-->
    <link href="assets/css/vendor.min.css" rel="stylesheet"/>
    <link href="assets/css/style.css" rel="stylesheet"/>
    <!--
    Document Title
    =============================================
    -->
    <title>Libra</title>
  </head>
  <body class="bg-gray">
  <section id="libra">
        <div class="container">
          <div class="row clearfix">
            <div class="col-12 col-md-12 offset-md-1 col-lg-8 offset-lg-1">
            <h2 class="heading--title">Libra</h2>
            <h6>リクエストヘッダー情報</h6>
            <p>
                フラグは<code>stardustChrome</code>という特殊なブラウザでしか閲覧できません。
            </p>
            <p>
                X-Forwarded-For:XXX.XXX.XXX.XXX<br>X-Forwarded-Proto:https<br>X-Forwarded-Port:443<br>Host:libra.ctf.nazotoki.tech<br>X-Amzn-Trace-Id:Root=1-62d4bbe2-2d47baa667c0a1d31414dd16<br>Accept-Encoding:gzip, deflate<br>Accept:*/*<br>User-Agent:HTTPie/3.2.1<br><br />遠いところをよくおいでくださいました。ひみつの鍵を差し上げます。
<pre>-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAunVG5a8SbXgsayNWhd4f9FYsWb8z57P2Ql8Yq+fQgq0Y2xcH
/HgHO0vZrgSbjFLxnpx4D9arOtvGdn06GLZcL3eU32jPvqVh8QhqmaQ8bdUDlEp8
yt45DMoleYflw8c9q2dDsRjLUoE2qhtMm2xR8B9U+mRq+vVgEJdvNMgmn+XtsmRx
A41n3nHTfTcOznZaNRxyAqZjooDOuUoVBStJVUqbd4a3EzMCBbdAzyQ2VdQEigT0
PVstPiMI0draaO9oKlZkfuGkJJ3Ftn8+A4cjIG8ycihsGqfEMSVUpLmUI+Etb0+C
3iD+B7P25v0CDNdD2odWIRipjdE8TmuTA+AsuQIDAQABAoIBAQCBCSw5Q4EzNNk4
g8oa9m+SvhgPO90F2mrv37PJM7H+3R+4byXduIr4pDNO1G15HOWNaKdF/r+dCf88
fMk51OnTB6SFP5mVTAqNrc9n6FrRf3rsoufd1RASI8rvYfbGGBo7hkk4Q/phbH6S
FjZb0QibbnN2nQvUBP+oO8R/+IuSV3NgxvtQ3CggRnqTiwx99kHO3gEXJVHozq09
mceaWIQeT6jAf5nMsNKl2YlBxomMkeXZ8jEcCJQcAPuHJjZ2SJmLpbADIffg/c95
oaLejnoEWUflnN/QPw7shHE2F50uKyEYAh5uNCjVHRQE15jUOQZBr5aLMtpyCkfJ
3bPrzFcBAoGBAOyEMyBBwAh2Kuk+QS9OCsFcxjTQPV/jkHnk2nU8I7xrO0MqONOL
/ily+GcbTrY4bx8zkIKuYwEAp/5Mybd+C/kRsjeSGZVrNxNDNG32JYTn49Z1miKv
jNMGUJsfOoKI6G1gKnI3j5WlP2E+xXtQkJUMwqj5vdafCTVJhWrucFyxAoGBAMnR
ave7rOChNxVEoS7YmwMUSEk/PlM4MLkv7WkOPdNATxZ2u5fj8RrHfI4pGt//QXUX
pI0dE6Y8ndM24YhynvqGYr8cyygBj+BpMZFXSFmf2ozRTawFbo2IgqqsZ6AszQVb
EUuq5k5mx5Mg+ilMMzTmxjL4AD5GRy/2ofYK5DKJAoGACdhW6HjULYX9s0fMHtP4
zqO1/GzOoTcvxGMqVMb0FdvA08LmKqghJEiM3n3cgOlIdtwGn+nyZRBJ7eP0YZb1
mKCL8pQ6TGXyHPMnM4yTczzT1xF+IQN9sSsKH+rk3JomUqc2HRsC9w+x27JpNgDc
g9fMIoyCwnRMRdORoinas4ECgYAausnYFdtHxRJulrBia/3b4ovQZ7fxfbe2T0q6
Z1B48kOHTiJ6c44zZchxa7BLips4zvDUX82CbvTYTKSCVewIclQRy9Z8bfiIWGZg
QZcrh6iCjhYjenSx+iqUQFFZPZXJ583an7/xElvMeMmpPpZpo0cM6RvfI5+6EohQ
9hBTQQKBgQCBiu+qiElJXJUTrwr3XESHUEsCPz27VWViYlj/n6GsY80QSf4Y1F0d
ynDDCJ4zE0FL1WfFFGMqaE86wvQZwvv9NCqSkpxhfbYhf/lKUmyNIyqoQYkV7kAK
NjQ0gdNlw4NLioSXny6/0k5G4OIzwh8QNf38sZGKhm7FMVQ4G8r1Yw==
-----END RSA PRIVATE KEY-----</pre>
                                    </p>
            <a href="/" onclick="window.location.reload();">再読み込み</a>
            </div>
          </div>
        </div>
      </section>
  </body>
</html>
```

秘密鍵を得ることができたため、これを利用してパスワードを `password.enc` を複合します。  
複合には[CyberChef](https://cyberchef.org)を利用しました(いつもの)。

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/pisces/decode.png", alt="screenshot - pisces/decode", position="center", style="height: 30vh") }}

入手したパスワード( `fomalhaut` )で `encrypt-pisces-new.zip` を展開するとJPEGファイルが出てきます。

```
❯  unzip encrypt-pisces-new.zip
Archive:  encrypt-pisces-new.zip
[encrypt-pisces-new.zip] pisces.jpg password:
  inflating: pisces.jpg
```

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/pisces/pisces.jpg", alt="screenshot - pisces/pisces", position="center", style="width: 50vw") }}

"strings"コマンドや"binwalk"、"青い空を見上げればいつもそこに白い猫"で調べても有力な情報はなく、グッと睨んでも何も見えてきません。

ところで、ここまで触れていませんでしたがnazotokiCTFにはヒントが用意されています。  
ヒントを使うと点数が減りますが、どうにも解決策が見えないためヒントを見ました。

> そろそろ目が疲れてきたころかと思って涼し気な画像をご用意しました。ちょっと休憩してぼんやり眺めてみてはいかがでしょうか。  
> もしあなたが若者なら、年上の人に見せてみるとピンとくる人がいるかもしれません。

……なるほど？  
筆者は若者ですので()厳しそうだということがわかりました。

しかし困ったことに年上の人はおろか誰も近くにおりません。  
こういうときはGoogle長老の知恵を借ります。

Google画像検索に `pisces.jpg` を渡すと"Imaginary CTF Round 12"のwriteupがヒットしました。

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/pisces/google.png", alt="screenshot - pisces/search result", position="center", style="width: 50vw") }}

writeup内で使用されていた[Magic Eye Solver / Viewer](https://magiceye.ecksdee.co.uk/)を通すことでフラグを見つけることができました。  
=> `ケッパク`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/pisces/magic_eye_solver.png", alt="screenshot - pisces/magic eye solver", position="center", style="width: 50vw") }}


## Web - Air elemtnts


### ふたご座

> Webサイトにアクセスしフラグを探せ  
> https://gemini.ctf.nazotoki.tech 

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/gemini/gemini.png", alt="screenshot - gemini/web app", position="center", style="width: 50vw") }}

パスワードは `dioskouroi` であることがわかっているがフォームに入力できません。  
インスペクターから"realPassword"の"value"を書き換えてsubmitすることでフラグを得ました。
=> `ナイーブ`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/gemini/input.png", alt="screenshot - gemini/input", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/gemini/flag.png", alt="screenshot - gemini/flag", position="center", style="width: 50vw") }}


### てんびん座

> Webサイトにアクセスしフラグを探せ  
> https://libra.ctf.nazotoki.tech

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/libra/libra.png", alt="screenshot - libra/probrem", position="center", style="width: 50vw") }}

"user-agent"で指定ブラウザ( `stardustChrome` )に偽装するとフラグを得られます。

```
❯ https libra.ctf.nazotoki.tech user-agent:stardustChrome
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Length: 757
Content-Type: text/html; charset=UTF-8
Date: Sun, 17 Jul 2022 04:36:48 GMT
Server: Apache/2.4.38 (Debian)
Vary: Accept-Encoding
X-Powered-By: PHP/7.4.8

<!DOCTYPE html>
<html lang="Ja">
  <head>
    <!-- Document Meta-->
    <meta charset="utf-8"/>
    <!-- Stylesheets-->
    <link href="assets/css/vendor.min.css" rel="stylesheet"/>
    <link href="assets/css/style.css" rel="stylesheet"/>
    <!--
    Document Title
    =============================================
    -->
    <title>Libra</title>
  </head>
  <body class="bg-gray">
  <section id="libra">
        <div class="container">
          <div class="row clearfix">
            <div class="col-12 col-md-12 offset-md-1 col-lg-8 offset-lg-1">
            <h2 class="heading--title">Libra</h2>
            <h6>リクエストヘッダー情報</h6>
            <p>
                フラグは<code>stardustChrome</code>という特殊なブラウザでしか閲覧できません。
            </p>
            <p>
                X-Forwarded-For:XXX.XXX.XXX.XXX<br>X-Forwarded-Proto:https<br>X-Forwarded-Port:443<br>Host:libra.ctf.nazotoki.tech<br>X-Amzn-Trace-Id:Root=1-62d391e0-6ba1eb6e72b3da5569cc18e1<br>Accept-Encoding:gzip, deflate<br>Accept:*/*<br><br />確かにstardustChromeを使ってるね。フラグをどうぞ！ <br />nazotokiCTF{<code>クローン</code>} <br />
            </p>
            <a href="/" onclick="window.location.reload();">再読み込み</a>
            </div>
          </div>
        </div>
      </section>
  </body>
</html>
```


### みずがめ座

https://aquarius.ctf.nazotoki.tech

> Webサイトにアクセスしフラグを探せ  
> https://aquarius.ctf.nazotoki.tech 

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/aquarius/aquarius.png", alt="screenshot - aquarius/probrem", position="center", style="width: 50vw") }}

適当に0や1を入れたりしていましたが思いつかなかったためヒントを見ました。

> passwordを入力することで指定した社員情報を閲覧できます  
> と書かれています。 スターダストセキュリティの社員の中で、一人だけ社員ナンバーを知っている人がいませんか？ その人の社員ナンバーを使って、「password」と入力すれば検索ができそうです。

言われてみればとイントロダクションで社員ナンバーを確認してパスワード( `password` )を入力すると、社員ナンバー `9999` がフラグを持っているとわかります。  
(余談ですが、一般的なCTFでは推測要素を排除する作りになっていることが多いため、このあたりはnazotokiCTFらしさが強く出ていると思いました。)

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/aquarius/ai-intro.png", alt="screenshot - aquarius/ai-intro", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/aquarius/number_99.png", alt="screenshot - aquarius/number_99", position="center", style="width: 50vw") }}

いつもの( `1' OR 1 == 1; -- ` )ではエラーとなりましたが、入力をそのままSQLに引き渡していることがわかったためエラーを元に入力を組み替えて( `' OR TRUE OR '` )フラグを得ました。  
=> `タカハシ`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/aquarius/flag.png", alt="screenshot - aquarius/flag", position="center", style="width: 50vw") }}


## Knowledge - Earth elemtnts

知識問題のため簡単に書きます。


### おうし座

> 2021年に行われた、コンピューターウイルス「emotet」のテイクダウン作戦の名前を日本語で言うと？  
> 「○○○○ムシ作戦」

`テントウ`

```
Emotetのインフラは2021年1月27日に「Operation LadyBird」によってテイクダウンされました。

https://blogs.jpcert.or.jp/ja/2021/02/emotet-notice.html#2
```


### おとめ座

> Webアプリケーションのセキュリティ分野の研究・ガイドラインの作成・脆弱性診断ツールの開発・イベント開催などの活動をしているオープンソースソフトウェアコミュニティの名称の読み方をカタカナ4文字で答えよ。

`オワスプ` ( https://owasp.org/ )


### やぎ座

> ある数 x を数 b のべき乗bᵖとして表した場合のpのこと。logとも呼ばれるこの数を日本語で何というか？カタカナ4文字で答えよ。

`タイスウ`


## Riddle - Fire elements


### おひつじ座

> あなたが目指しているものの間を読め

プロローグで `セキュリティ業界の星を目指すのデス!` と書かれていて、星の絵もあるため、イントロダクションのどこかに答えがあるのだろうと考えましたがわからず。  
日付が変わった後、諦めてヒントを開けても `プロローグをよく読んでね` `あなたが目指してるものは「セキュリティ業界の星」 つまり…「★」` と答え合わせができただけで詰んだと思いましたが、星の間を縦読みでフラグになりました。
=> `ハンドル`

行が揃っておらず不自然だなと縦読みはしていましたが、(星に気づく前だったこともあり)右上から半分くらい見て諦めていました。  
もう少し粘ればよかった……

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/aries/flag.png", alt="screenshot - aries/flag", position="center", style="width: 50vw") }}


### しし座

> ルールを守れ

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/leo/leo.png", alt="screenshot - leo/probrem", position="center", style="width: 50vw") }}

"大切なルール"でたぬきすることでフラグになります。  
=> `チーター`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/leo/rule.png", alt="screenshot - leo/rule", position="center", style="width: 50vw") }}


### いて座

> (画像のみ)

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/sagittarius/sagittarius.png", alt="screenshot - sagittarius/probrem", position="center", style="width: 50vw") }}

> hint: 身の回りで4つ種類があるものと言えば？

> hint: 地図とかで見かけます

ヒントより `トウ` , `ザイ` , `ナン` , `ボク` に対応して `イースト`


## 2nd Challenge


### へびつかい座

> 最終問題に進むためのフラグを導く魔法の解答用紙をご用意しました（謎解きによくあるやつ）

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/ophiuchus/answersheet.png", alt="screenshot - ophiuchus/answersheet", position="center", style="height: 40vh") }}

今まで手に入れたフラグを、プロローグにある画像の順番で並べると問題文が現れます。  
=> `ハテナイチオクカイタタケ`

```
おひつじ座: ハンドル
おうし座　: テントウ
ふたご座　: ナイーブ
かに座　　: イイワケ
しし座　　: チーター
おとめ座　: オワスプ
てんびん座: クローン
さそり座　: カクダイ
いて座　　: イースト
やぎ座　　: タイスウ
みずがめ座: タカハシ
うお座　　: ケッパク
```

"ハテナ"とはメニュー左のマークのことでしょう。  

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/ophiuchus/hatena.png", alt="screenshot - ophiuchus/hatena", position="center", style="width: 50vw") }}

`onclick` に `hatenaClick()` が指定されているため、スクリプト内からフラグを読みました。  
=> `ポラリス`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/ophiuchus/onclick.png", alt="screenshot - ophiuchus/onclick", position="center", style="width: 50vw") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/ophiuchus/function.png", alt="screenshot - ophiuchus/function", position="center", style="width: 50vw") }}


## Last Challenge

> パスワードを解け  
> http://polaris.ctf.nazotoki.tech 

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/polaris/polaris.png", alt="screenshot - polaris/probrem", position="center", style="width: 50vw") }}

"パスワードのヒントは愛の星座の中"を元に調べ、愛の神と関連があるらしいうお座を示していると考えましたが、その先がわからずヒントを見ると全く違うことがわかりました。

> ここまで来れたなら、フラグを導くために必要な情報の全ては既にあなたの前に提示されているはずです。  
> 愛とはもちろん採用担当アイちゃんのことです。アイの星座はなんでしょうか？ 彼女の誕生日がわかれば星座もわかりそうですね。どこかで彼女の誕生日を見ませんでしたか？

イントロダクションから7月18日の星座を調べるとかに座であり、かに座の問題には使わないpcapファイルがあったことを思い出します。  
テキスト以外の12ファイルを見ると、FTPで星座の名前が書かれたPNGファイルを送受信しているものだとわかります。  

フラグの数とpcapファイルの数は一致するため、何らかの順で並べ替えればよいと考えましたが、このとき開催終了まで1時間を切っていたためヒントをみました。

> アイちゃんの誕生日は7月18日、つまりかに座です（ちなみに彼女が持ってるノートパソコンのマークもかに座です、気がつきました？）
> さて、かに座の問題の仲間外れ以外のデータは何のデータだったでしょうか？その中に順番を示す物はなかったでしょうか？

> かに座の問題にはpcapファイルが12個ありました。  
> パケットには前のパケットを示す Sequence number と後ろのパケットを示す Acknowledgment number いう情報が付与されており、それを見るとバラバラの12個はもともとひとつのストリームのなかでつながったパケットであったことがわかります。
> パケット同士の前後関係を確認し、流れに従って並び替えると
> 1. うお座
> 1. やぎ座
> 1. かに座
> 1. てんびん座
> 1. おひつじ座
> 1. みずがめ座
> 1. おとめ座
> 1. しし座
> 1. ふたご座
> 1. さそり座
> 1. いて座
> 1. おうし座 の順番になるようです。

並べ替えた結果が記載されていたため、その通りに並べるとパスワードが見えてきます。
=> `stardust`

```
うお座　　: ケッパク
やぎ座　　: タイスウ
かに座　　: イイワケ
てんびん座: クローン
おひつじ座: ハンドル
みずがめ座: タカハシ
おとめ座　: オワスプ
しし座　　: チーター
ふたご座　: ナイーブ
さそり座　: カクダイ
いて座　　: イースト
おうし座　: テントウ
```

パスワードを入力するとエピローグ画面が表示されます。

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/polaris/epilogue.png", alt="screenshot - polaris/epilogue", position="center", style="width: 50vw") }}

ページ内のストーリーも読みながら余韻に浸っていたところ、少しスクロールしたところにフラグが書かれていました。  
そういえばまだフラグは入力していませんでしたね。取りこぼすところでした。  
=> `オールト`

{{ image(src="https://fs.arch4e.com/blog/writeup_nazotokiCTF_2022/polaris/flag.png", alt="screenshot - polaris/flag", position="center", style="width: 50vw") }}


## さいごに

謎解き要素を盛り込んだCTFということもあり、提示されている情報から推理したり技術的な要素を用いたりと普段のCTFとは違った楽しみがありました。  
ヒントが充実していたところも親切で、ほどよく沼にハマりながら進めることができました。

nazotokiCTF作者の[@nomizooone](https://twitter.com/nomizooone)さんと、協賛のシステムガーディアン株式会社様への感謝を述べwriteupの締めといたします。

nazotokiCTFの開催ありがとうございました m(_ _)m


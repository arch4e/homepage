+++
title = "[writeup] CpawCTF Level 3"
date = 2020-11-08

[taxonomies]
tags = ["CTF", "writeup"]
+++

---

* [CpawCTF Level 1](https://arch4e.com/posts/writeup-cpawctf-level1/)
* [CpawCTF Level 2](https://arch4e.com/posts/writeup-cpawctf-level2/)
* [CpawCTF Level 3](https://arch4e.com/posts/writeup-cpawctf-level3/)

---


## [Reversing] またやらかした！

```
[ Challenge ]
またprintf（）をし忘れたプログラムが見つかった。
とある暗号を解くプログラムらしい…
reversing200
```

radare2でファイルを読み込むと `zixnbo|kwxt88d` と `0x19` をXORをしている。

```python3
❯ python3
Python 3.8.5
[GCC 9.3.0] on linux
>>> _str = 'zixnbo|kwxt88d'
>>> ''.join([chr(ord(tmp)^0x19) for tmp in _str])
'cpaw{vernam!!}'
```

=> `cpaw{vernam!!}`


## [Web] Baby's SQLi - Stage 2 -

```
[ Challenge ]
うーん，ぱろっく先生深くまで逃げ込んでたか．
そこまで難しくは無いと思うんだけども……．

えっ？何の話か分からない？
さてはStage 1をクリアしてないな．

待っているから，先にStage 1をクリアしてからもう一度来てね．
Caution: sandbox.spica.bzの80,443番ポート以外への攻撃は絶対にしないようにお願いします．
```

"Baby's SQLi - Stage 1 -" で入手したURLにアクセスする。

```
https://ctf.spica.bz/baby_sql/stage2_7b20a808e61c8573461cf92b1fe63b3f/index.php
```

ユーザ名は指定のもの、パスワードに `' OR 1 == 1` を指定してflagを入手。

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/babys_sqli_stage2/stage2.png", alt="screenshot", position="center", style="width: 45vw; padding-bottom: 1rem") }}

{{ image(src="https://fs.arch4e.com/blog/writeup_cpawctf/babys_sqli_stage2/flag.png", alt="screenshot", position="center", style="width: 45vw") }}


=> `cpaw{p@ll0c_1n_j@1l3:)}`


## [PPC] Remainder theorem

```
[ Challenge ]
うーん，ぱろっく先生深くまで逃げ込んでたか．
そこまで難しくは無いと思うんだけども……．

えっ？何の話か分からない？
さてはStage 1をクリアしてないな．
待っているから，先にStage 1をクリアしてからもう一度来てね．

Caution: sandbox.spica.bzの80,443番ポート以外への攻撃は絶対にしないようにお願いします．
```

```
x ≡ 32134 (mod 1584891)
x ≡ 193127 (mod 3438478)

x = ?

フラグはcpaw{xの値}です！
```

`x % 1584891 = 32134` と `x % 3438478 = 193127` を満たす `x` を求める。

```python
if __name__ == '__main__':
    divisor_1   = 1584891
    remainder_1 = 32134

    divisor_2   = 3438478
    remainder_2 = 193127

    n = 1
    while True:
        x = divisor_1 * n + remainder_1
  
        if x % divisor_2 == remainder_2:
            print(f'x = {x}')
            exit(0)
        else:
            n += 1
```

```
❯ python3 solver.py
x = 35430270439
```

=> `cpaw{35430270439}`


## [Crypto] Common World

```
[ Challenge ]
Cpaw君は,以下の公開鍵を用いて暗号化された暗号文Cを受け取りました．しかしCpaw君は秘密鍵を忘れてしまいました.Cpaw君のために暗号文を解読してあげましょう.

(e, N) = (11, 236934049743116267137999082243372631809789567482083918717832642810097363305512293474568071369055296264199854438630820352634325357252399203160052660683745421710174826323192475870497319105418435646820494864987787286941817224659073497212768480618387152477878449603008187097148599534206055318807657902493850180695091646575878916531742076951110529004783428260456713315007812112632429296257313525506207087475539303737022587194108436132757979273391594299137176227924904126161234005321583720836733205639052615538054399452669637400105028428545751844036229657412844469034970807562336527158965779903175305550570647732255961850364080642984562893392375273054434538280546913977098212083374336482279710348958536764229803743404325258229707314844255917497531735251105389366176228741806064378293682890877558325834873371615135474627913981994123692172918524625407966731238257519603614744577)

暗号文: 80265690974140286785447882525076768851800986505783169077080797677035805215248640465159446426193422263912423067392651719120282968933314718780685629466284745121303594495759721471318134122366715904

[これは…？](https://ctf.cpaw.site/download.php?param=8a53ea3e483e88ac5a8b45787f2df9bd)

フラグは以下のシンタックスです.
cpaw{復号した値}

※復号した値はそのままで良いですが，実は意味があります，余力がある人は考えてみてください.
```

問題文で指定されている(e, N)は、RSA暗号の公開鍵を表す。
平文を `m` 、暗号文を `c` 、公開鍵を `(e, N)` 、秘密鍵を `d` とおくと、RSA暗号は次の式で表される。

```
N = pq
c = m^e mod N
d = e^-1 mod (p - 1)(q - 1)
m = c^d mod N
```

`e = 11` 、 `e = 13` のときの `c` が既知のため、そこから `m` を計算できる。

```
c1 = m^11 mod N
c2 = m^13 mod N

c2/c1 = m^2 mod N
c2/c1 = 180040032052240663195289808096837316
m = √(c2/c1)
  = 424311244315114354
```

=> `cpaw{424311244315114354}`


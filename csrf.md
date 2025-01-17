# CSRF(シーサーフ)攻撃の説明と対策方法について

## はじめに

こんにちは！
<br>今回はタイトルにもあるように、<br>CSRF攻撃についての説明および対策方法に関する記事です！
<br>CSRFについて学ぶ機会があったため、備忘録として残そうと思い投稿しました。
<br>今回の記事が初投稿です！

### 対象読者

- プログラミング初学者の方
- CSRF攻撃についての基本的な知識を知りたい方
- CSRF攻撃の基本的な対策方法について知りたい方
- Webアプリケーションの脆弱性について学びたい方
  <br>
  <br>

### 記事を読むメリット

- CSRF攻撃の基礎的な内容、実例について学ぶことができる
- CSRF攻撃の基本的な対策を知ることができる
  <br>
  <br>

## １. CSRF(Cross Site Request Forgery)攻撃とは

CSRF（シーサーフ）攻撃とは、Webアプリケーションの脆弱性を利用し、ユーザーが意図しない形で処理を行うサイバー攻撃のことです。

### CSRF攻撃による不正な操作の例

- ユーザーのパスワードを攻撃者が設定したものに変更
- ユーザーの銀行口座から攻撃者への送金

まずは、どのようにCSRF攻撃が行われるかを解説します。

## ２. CSRF攻撃はどのように行われるか

hoge銀行の口座を持つユーザーに100万円を送金させる<br>
CSRF攻撃を想定して解説します。

### hoge銀行口座から攻撃者に送金させるCSRF攻撃

1. ユーザーがhoge銀行のサイトにログインする
2. ログイン状態で、攻撃者が用意しているWebサイトに入る
3. 攻撃者のWebサイトには、hoge銀行のサイトにリクエストを送るフォームが仕掛けられている。
4. ユーザーがWebサイトのフォームボタンをクリックすると、<br>hoge銀行のサイトに攻撃者が設定したリクエストが送信されてしまう。
5. hoge銀行のサイトは、攻撃者のリクエストをユーザーのリクエストと誤認してしまい、処理を実行してしまう。
6. ユーザーから攻撃者に送金される。

## 3. CSRF攻撃の仕組み

第2項で取り扱った「hoge銀行口座から攻撃者に送金させるCSRF攻撃」<br>を元に、CSRF攻撃の仕組みについて解説します。

### 1. ユーザーがhoge銀行のサイトにログインする
CSRF攻撃は、<br>
**ユーザーのログイン情報=Cookieに保存されているセッションID**<br>
を利用して行われます。
1
### 補足:Cookieとは
Cookieとは、**Webサイトがユーザーのブラウザに保存する小さなデータ**のことです。
サイトにログインした時、サイトは「この人はログイン済みだよ」という情報（セッションID）をクッキーとして**ブラウザに保存します。**<br>これにより、次にサイトを訪れたときでも、再度ログインをする必要がなくなります。
### 2. ログイン状態で、攻撃者が用意しているWebサイトに移動する
ログイン状態＝CookieとしてセッションIDが保存されている状態。
### 3. 攻撃者のWebサイトには、hoge銀行のサイトにリクエストを送るフォームが仕掛けられている。

### 4. ユーザーがWebサイトのフォームボタンをクリックすると、<br>hoge銀行のサイトに攻撃者が設定したリクエストが送信されてしまう。
フォームボタンをクリックすると、**リクエストと同時にブラウザに保存されているCookieのデータも送信される。**（Cookieの仕様）

### 5. hoge銀行のサイトは、攻撃者のリクエストをユーザーのリクエストと誤認してしまい、処理を実行してしまう。
Cookieのログイン情報を参照するため、ユーザーからの正当なリクエストと誤認してしまう。その結果、攻撃者が設定したリクエストを実行してしまう。
### 6. ユーザーから攻撃者に送金される。
## 4. CSRF攻撃が成功してしまう原因
hoge銀行のWebサイトがなぜCSRF攻撃を防げなかったのか。それは、リクエストの判別方法が単純だったということに原因があります。
### 解説
hoge銀行のWebサイトの場合、
ユーザーのリクエストか否かを確認するために参照した情報が、Cookieとして保存されているセッションIDのみでした。
そのため、悪質なWebサイトからのリクエストであっても、ユーザーのセッションIDさえ一致していれば、正当なリクエストとして処理をされてしまいました。これを改善するためには、**CookieのセッションID+（それ以外の情報）**を参照することが重要です。



## 5. CSRF攻撃の対策方法




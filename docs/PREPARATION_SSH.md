# Git/GitHub利用環境整備

所要時間　60分

## 作業内容
- 1.GitHubの利用環境準備
- 2.Gitの利用環境準備

## 前提条件
- Windows(10以降)を利用していること
- SMSが受信可能でMFA用のアプリケーションのインストールが可能なスマートフォン等の端末を保持していること
- 今までにSSHのキーペアを生成したことがないこと

## 用語解説
|名称|意味|
|--|--|
|Git Bash|Gitを利用するためのツール|
|Source Tree|GitをGUIから操作するためのツール(事前にGit Bashのインストールが必要)|
|SSH|暗号化した通信手段の一つ(Secure Shell)。秘密鍵と呼ばれるファイルをクライアントPCに、<br>公開鍵と呼ばれるファイルをGitHub上に保存することで機能する。|
|MFA|多要素認証(Multi Factor Authentication)。アカウントの乗っ取りなどを防ぐために設定する必要がある。|

## 1. GitHubの利用環境準備

- 1.1 GitHubアカウントの作成・・・省略
- 1.2 MFAの設定・・・各自要設定

### 1.1 GitHubアカウントの作成

GitHubアカウントは作成済みのため省略

<!--
下記サイトにアクセスしGitHubアカウントを作成します。

[GitHub Japan | GitHub](https://github.co.jp/)

「GitHubに登録する」をクリックします。

![step 1.1.1](/img/1.1/1.png)

ユーザ情報を入力します。
**※UsernameはGitHub内で一意の値となります。判別しやすい名前を登録するようにしてください。**

![step 1.1.2](/img/1.1/2.png)

アカウントの検証を行います。画面の指示に従ってください。
(指示は人によって異なります。)

![step 1.1.3](/img/1.1/3.png)

検証が完了すると「Create account」をクリックできるようになるのでクリックします。

![step 1.1.4](/img/1.1/4.png)

番号の入力を求める画面が表示されます。
アカウント作成時に登録したメールアドレス宛に番号が送信されるので確認の上入力してください。

![step 1.1.5](/img/1.1/5.png)

参考)確認番号を知らせるメール

![step 1.1.6](/img/1.1/6.png)

確認番号の入力が完了すると下記画面が表示されます。
「Continue」ボタンをクリックします。

![step 1.1.7](/img/1.1/7.png)

下記画面が表示されます。どの項目にもチェックを入れず「Continue」をクリックします。

![step 1.1.8](/img/1.1/8.png)

下記画面が表示されます。「Continue for free」をクリックします。

![step 1.1.9](/img/1.1/9.png)

以下のような画面が表示されることを確認します。

![step 1.1.10](/img/1.1/10.png)

ここまで完了したら岡野まで、ご自身のアカウント名を通知ください。

次にMFAの設定を行います。

-->

### 1.2 MFAの設定

下記リンクを参考にMFAの設定を実施します。
MFA用にデバイスにインストールするアプリケーションについては任意のものを選択してください。

https://docs.github.com/ja/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication

次にGitの設定を行います。

## 2. Gitの利用環境準備

- 2.1 Git Bashのインストール
- 2.2 Gitの初期設定、疎通確認
- 2.3 Source Treeのインストール
- 2.3 Source Treeの初期設定、疎通確認

### 2.1 Git Bashのインストール

以下のページからGit for Windowsをインストールします。

https://gitforwindows.org/

インストール手順は以下を参考にします。
(開発環境そのものがWindowsのみ(CRLFのみ)の環境を想定しているため、基本デフォルト設定のまますすめればよい。
https://www.curict.com/item/60/60bfe0e.html

### 2.2 Gitの初期設定、疎通確認
Git Bashを起動します。

Windows画面左下の検索ボックスに「Git Bash」と入力すると
選択画面が表示されるので、「開く」をクリックします。

![step 1.2.1](/img/1.2/1.png)

まず、gitを利用するユーザ名を入力します。ユーザ名は任意です。

```
git config --global user.name "ユーザ名"
```

次にメールアドレスを設定しましょう。

```
git config --global user.email "メールアドレス"
```

### 2.3 SSH秘密鍵・公開鍵のペア作成と公開鍵のGitHubへの登録

Git Bashから以下のコマンドを実行
(レガシシステムも想定し、今回はRSA暗号方式を採用)

```
ssh-keygen -t rsa -b 4096 -C "メールアドレス"
```

パスワード入力を3回求められるが何も入力せずEnterを続けて押す。
(公開鍵暗号方式でパスワード認証は不要)

C:\Users\a00~\下に.sshというフォルダが作成されている。
このフォルダのid_rsa.pubというファイル(公開鍵)をメモ帳で開き、クリップボードにコピーする。

GitHubの個人設定画面のSSH and GPG keysという画面から「New SSH key」というボタンをクリックし、
クリップボードにコピーした公開鍵を貼り付ける。

プライベートリポジトリがcloneできるか確認する。Git Bashを起動し、任意のフォルダ上で
以下のコマンドを実行する。

```
git clone git@github.com:mes-s-biz-dev/phase2_dtc.git
```

以下のコマンドを実行して、phase2_dtcがあればok
```
ls
```

SourceTreeでの疎通確認が必要なため一旦ローカルリポジトリを削除
```
rm -rf phase2_dtc
```

Git Bashを閉じます。
```
exit
```

次にSource Treeのインストールを行います。

### 2.4 Source Treeのインストール

** IDEに組み込みのGit連携機能があり、そちらを利用したい場合は不要です。 **
下記リンクを参考に「環境設定」の項まで設定を行います。
途中「Mercurial」のインストールの有無を聞かれるのでチェックボックスを外すようにしてください。

https://yu-report.com/entry/sourcetree/

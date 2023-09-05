# Welcome to GitHub World!
GitHubはGitの仕組みを利用したバージョン管理用のプラットフォームサービスです。
このリポジトリでGitとはなにか、GitHubとは何かを学習しましょう。

参考サイト
- https://backlog.com/ja/git-tutorial/

環境構築用リンク
- [Git、GitHub利用環境準備(常石環境)](/docs/PREPARATION_SSH.md)
- [Git、GitHub利用環境準備(MESネット環境)](/docs/PREPARATION_HTTPS.md)

## 1. What is Git?
GitHubについて理解をするためにはまずGitについて理解する必要があります。Gitとは分散型のバージョン管理システムです。
まずはバージョン管理について考えてみましょう。

プログラムを複数人で開発する場合、各個人が開発したプログラムを共有しあう必要があります。
それだけなら共有フォルダなどで共有してしまえばよいのでは？と思うかもしれませんがそれだと以下のような問題があります。

- 最新化された情報が常に手に入るとは限らない
- プログラムの変更に対して5W1Hがわからない
- 巻き戻し作業が大変

以下の項からそれぞれ具体的にどのような問題なのか、またGitを使うことでどのように対処できるかを述べていきます。

### 1.1 最新化された情報が常に手に入るとは限らない

Aさんは共有フォルダから必要なソースコードをダウンロードして開発を行ったとします。
Aさんは犬についてのプログラムを書いているとして、犬の吠えるという機能を実装しようとしています。
苦労してプログラムを完成させ、いざ共有しようとするとすでに別のBさんという人が吠える機能についての
コードを完成させていました。これではAさんの努力は水の泡です。
Gitの仕組みを活用することで現在開発中のプログラムが常に最新の状態か確認することが可能です。
これでAさんの努力が今後無駄になることはないでしょう。

### 1.2 プログラムの変更に対して5W1Hがわからない

プログラムの変更に対して
- 何を(What)
- 誰が(Who)
- いつ(When)
- どこで(Where)
- なぜ(Why)
- どのように(How)

といった付属の情報がないと後から作業するユーザは何のことやらさっぱりわからなくなります。

例えばさっきの犬プログラムを考えてみましょう。新たに開発に参加したCさんはアメリカ在住です。
犬プログラム中に以下のような箇所を見つけました。

```
void shout() {
  std::string msg="わんわん";
  std::cout << msg << std::endl;
}
```

Cさんは「アメリカだったら"bow wow"と吠えるだろうからこの吠える機能については受け取ったメッセージを出力するように修正しよう。」と以下のように変更しました。

```
void shout(std::string msg) {
  std::cout << msg << std::endl;
}
```

しかしこの変更の意図を知らないAさんは何でこのような変更を行ったのか確認しようとします。
共有フォルダのアクセス履歴から誰が変更したのか確認しようとしますが、アクセス履歴をみるといろんな人がアクセスしているためなかなか特定できません。
やっとの思いでCさんが変更したことを突き止め、遠く離れたCさんに国際電話をかけ事情を聴いてやっと変更理由を理解できました。
大規模なプログラムの開発において毎回このようなやり取りを行うのは大変です。

Gitはアクセスや保存履歴ではなく「コミット」と呼ばれる単位でプログラムの変更を管理するのですが、それは各ユーザが任意のタイミングで行います。
コミットの際にはコミットメッセージを必ずつけるのがお約束となっており、コミット履歴をAさんが見ると以下のようになっています。

``` mermaid
%%{init: { 'theme': 'dark' ,'themeVariables': {
  'commitLabelColor': '#ffffff',
  'commitLabelBackground': '#ff0000'
} } }%%
gitGraph
  commit id: "プロジェクト作成。commited by A"
  commit id: "犬クラスを作成した。commited by A"
  commit id: "犬クラスに吠える機能実装した。 commited by A"
  commit id: "吠える機能を言語別に切替できるようにした commited by C"
```

### 1.3 巻き戻し作業が大変
Aさんは犬プログラムについて食べる機能を実装しました。
やっとの思いで完成させた食べる機能ですが、なんとそれを新人のDさんが全く異なるプログラムに変更してしまいました。
バグだらけで使い物になりません。Aさんは悲しくなります。
そんな時Gitを利用していれば巻き戻すのは簡単です。Gitは前述のコミットという単位ですべての変更履歴を保持しているため
Dさんが変更を加える前の状態のコミットまで巻き戻すことができます。
よかったね。Aさん。

``` mermaid
%%{init: { 'theme': 'dark' ,'themeVariables': {
  'commitLabelColor': '#ffffff',
  'commitLabelBackground': '#ff0000'
} } }%%
gitGraph
  commit id: "プロジェクト作成。commited by A"
  commit id: "犬クラスを作成した。commited by A"
  commit id: "犬クラスに吠える機能実装した。 commited by A"
  commit id: "吠える機能を言語別に切替できるようにした commited by C"
  commit id: "食べる機能を実装した。commited by A" type: HIGHLIGHT
  commit id: "食べる機能に飲む機能を包含した。commited by D" type: REVERSE
```

## 2. What is GitHub?
Gitが何なのか説明したところでGitHubについても説明しましょう。
GitHubについて理解するためにはまず、***リポジトリ***という用語について説明する必要があります。

### 2.1 リポジトリ
リポジトリとは端的に表現すればGitでバージョン管理を実施する対象のフォルダのことです。
このフォルダの中のファイルについてはすべてGitによって管理され、逆にフォルダ外については
Gitによる管理はできません。

### 2.2 ローカルリポジトリとリモートリポジトリ
リポジトリは大きく2種類に大別されそれぞれローカルリポジトリとリモートリポジトリと呼ばれます。
ローカルリポジトリとは各個人のPC内にあるGitのリポジトリのことを指し、リモートリポジトリは
インターネット上などにあるサーバー上にあるGitリポジトリのことを指します。
開発者どうしは開発作業は各個人のローカルリポジトリで実施し、コードの共有をリモートリポジトリで行います。
GitHubとは***リモートリポジトリを提供するためのプラットフォーム***であり、皆さんが開発するための
ソースコードはすべてGitHubに保存されている。というのが理想的な状態です。


``` mermaid
graph BT

  subgraph GitHub
    RemoteRepository
  end
  
  subgraph AさんのPC 
    LocalRepository1
  end
  
  subgraph BさんのPC
    LocalRepository2
  end
  
  subgraph CさんのPC
    LocalRepository3
  end
  
  subgraph DさんのPC
    LocalRepository4
  end
  
  LocalRepository1 --> RemoteRepository
  LocalRepository2 --> RemoteRepository
  LocalRepository3 --> RemoteRepository
  LocalRepository4 --> RemoteRepository
  
```

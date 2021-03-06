% イントロダクション
% Alex Honor; Greg Schueler
% November 20, 2010

## このマニュアルについて

Rundeck のユーザーマニュアルへようこそ。このマニュアルは Rundeck サーバー/ツールによりシステムの管理者を今すぐ生産的にするために書かれています。

## Rundeck とは？

Rundeck はデータセンターやクラウド環境におけるその場限りの作業やルーチンワークを自動化してくれる OSS です。これまで時間を浪費してきた単純作業を軽減したり、スクリプト化したものを簡単にスケールするための沢山の機能があります。

Rundeck は WebUI, CUI から多くのノードにタスクを実行させることができます。他に ACL やワークフロー設計, スケジューリング, ロギング, 外部データの統合といった機能もあります。

もう使いたくてたまりませんか？ [Rundeck のインストール](getting-started.html#installing-rundeck)に進んでしまいましょう。

### 誰が Rundeck を作っているの？
Rundeck は GitHub の [dtlabs/rundeck](https://github.com/dtolabs/rundeck) というプロジェクト名で開発されています。
多くのアイデアがこの分野のコンサル企業 [DTO Solutions](http://dtosolutions.com/) から提供されていますが、誰でもこのプロジェクトに参加して貢献することができます。

Rundeck はフリーソフトとして [Apache Software License] で配布されています。

[Apache Software License]: http://www.apache.org/licenses/LICENSE-2.0.html

## ヘルプを求める

*   Mailing list: [http://groups.google.com/group/rundeck-discuss](http://groups.google.com/group/rundeck-discuss)
*   IRC: irc://irc.freenode.net/rundeck
*   Unix manual pages: Rundeck をインストールすると shell ツールについての UNIX マニュアルページも付いてきます。`man -k rundeck` を実行してみましょう。  

## 9000m 上空から見た Rundeck

### Rundeck の機能

*   コマンドの分散実行
*   拡張可能な実行システム（デフォルトでは SSH）
*   マルチステップワークフロー
*   ジョブの定義と実行（即時 or スケジュール)
*   コマンドとジョブを実行するための GUI
*   ロールベースの ACL（LDAP/ActiveDirectory 連携可能）
*   履歴とログ監査
*   外部のホスト管理ツールとの統合(open integration)
*   CUI インタフェース
*   Web API

### 様々な状況下での Rundeck 

Rundeck は皆さんが既に使っているツール（Puppet, Chef, Rightscale など）を補完するためのもので、それらの連携操作の自動化を補助してくれます。もしあなたがターミナルからコマンドを走らせたりスクリプトを使ってサーバを管理しているなら、Rundeck はもっとユーザフレンドリーな代替手段になります。Excel や Wiki ページでノード一覧を管理してコマンド実行時にそれをコピペする代わりに、Rundeck はノードのフィルタリングをしてコマンドを並列実行するような、コントロールポータルになります。

Rundeck はローカルやクラウドプロパイダの仮想マシンでも問題なく動きます。コマンドディスパッチャが可能にするノードの抽象化が、そのような動的に管理される環境への対応をアシストしてくれます。

タスク自動化にあたって個々のツールの「境界」を意識する必要はありません。たとえば、ソフトウェアのデプロイやアプリケーションのメンテナンスをするとき、たいていは色んなツールの up/down を切り替えるなど一連の手順を実行しているでしょう。Rundeck はそういったマルチステップのワークフロー（パッケージマネージャだったり構成管理ツールだったり、システムユーティリティ、あるいはあなたのスクリプトなどを呼び出す部分）をシンプルにつくれるインタフェースをもっています。Rundeck は色んなツールの「接着」を助けるためのものなのです。そして最終的にそれを1ボタンで済む作業にして、誰でも実行できるようにしてくれるのです。

### Rundeck のアーキテクチャ

Rundeck は管理用のサーバにインストールするアプリケーションです。内部ではジョブ定義や実行ログを RDB に保存しています。コマンドやジョブの実行結果はローカルディスクに保存します。

Rundeck でのコマンド実行には鍵ベース認証の SSH 接続が利用されます。
そのため Rundeck のサーバ設定にはリモートホストでコマンドを実行可能なユーザ定義が含まれます。
なお、リモートマシンから Rundeck のサーバに SSH 接続できるようにする必要はありません。

![Rundeck architecture](../figures/fig0001.png)

Rundeck 自体は Java ベースの Web アプリケーションで、付属のサーブレットコンテナで動作します。
アプリケーションは GUI とネットワークインタフェースを提供します。
後者は Rundeck の Shell ツールからも利用されます。

Rundeck にアクセスするにはログイン ID とパスワードが必要です。
インストール直後の状態では単層ユーザディレクトリの中にデフォルトのログイン情報が入っています。
ログイン情報にはユーザ名とパスワード等と共に1つ以上のユーザグループが定義されています。
単層ユーザディレクトリの代替として LDAP を使うことも出来ます。
Rundeck 上でジョブ実行などの操作をする場合には必ず認証されたユーザでなければなりません。
これは管理者が定義するポリシーファイルベースのアクセスコントロール機能で制御されています。
特権はユーザの属するグループ単位でポリシーファイルによって付与されます。

2種類のインストール方法があります:

*   RPM: RPM はインストール管理のためのツールです。環境を統合するツール群、Man ページ、PATH 内へのシェルツール配備、起動管理スクリプトを提供します。
*   Launcher: Launcher はとにかく実行してみたい人向けにクイックセットアップするツールです。プロジェクトのブートストラップと新機能のおためしに最適です。

## フィードバック

もしなにか Rundeck のバグをみつけたり、質問やアイデアがあれば Rundeck のメーリングリスト [rundeck-discuss@groups.google.com](mailto:rundeck-discuss@groups.google.com) にメールを送ってください。

## 次は ?

ざっと概念的な概要を説明したので、次はインストールとセットアップに移ります。セットアップが終わったら分散コマンドディスパッチャの概要やどうやってアドホックコマンドを実行するのかを学びます。
最後に、ジョブのことやジョブワークフローを利用したマルチステッププロシージャ、オプションを用いてパラメータをどう制御するかについて学びます。


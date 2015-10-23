Getting Started
===========

このガイドは、DevOpsで簡単なアプリケーションを走らせるためのガイドです。

このガイドを読むことで

* VSOをつかってどうやってContinuous Deliveryの環境を作るか？
* Infrastructure as Codeの実際のサンプルをどのように見るか？
* ハッカソンの追加情報をどのように見つけるか

ということを理解できます。

1. 概要
-----------
　DevOpsは言語に依存しないコンセプトです。DevOpsのプラクティスの全体像を学ぶことは大変難しいことです。
しかし、DevOpsの実際のプロジェクトに参加すると理解できることでしょう。
このチュートリアルでは、継続的デリバリ / 継続的インテグレーションそして、Infrastructure as Codeを
Microsoft AzureやVisual Studio Onlineを使いながら理解できるようになっています。


### 1.1. アーキテクチャー

ハッカソンで既に作成されたアプリケーションが必要な場合は [PartsUnlimitedMRP](https://github.com/Microsoft/PartsUnlimitedMRP) を使うことができます。
このアプリケーションはWebアプリケーションと、APIサーバーを含んでいます。これらのアプリケーションを使って、ビルドサーバーとプロダクションサーバーを作っていきます。
これらはすべてLinux(Ubuntu)で動作するアプリケーションです。言語はJava / Javascriptが使われています。システム全体のアーキテクチャスタックは次のようになって
います。

![PartsUnlimitedMRP architecture](<media/PartsUnlimitedMRP-Overview.jpg>)

PartsUnlimitedMRPアプリケーションに変更を加えると、Visual Studio Onlineが自動的に変更を検知して、ビルドサーバー上で自動的にビルド/テストを行います。ビルドサーバーにはVSOエージェントがインストールされています。ビルドが終了すると、Visual Studio Onlineは、プロダクションサーバーに
アプリケーションをデプロイするようになっています。

.NETのアプリケーションが必要な場合は、[PartsUnlimited(日本語)](https://onedrive.live.com/view.aspx?cid=286f38e26945ae87&page=view&resid=286F38E26945AE87!2824&parId=286F38E26945AE87!2821&authkey=!AF94yD02BzMuf9g&app=Word)[PartsUnlimited(英語)](https://github.com/Microsoft/PartsUnlimited/blob/master/docs/GettingStarted.md)というアプリケーションを使うことができます。


2. チュートリアル
-----------

このチュートリアルでは、継続的デリバリ / 継続的インテグレーションと、Infrastructure as Codeを学ぶことができます。

### 2.1. 準備事項

このチュートリアルを始める前に、Azure / Visual Studio Online / Azure SDKを利用可能にしておく必要があります。
環境がまだ整っていない場合は下記の手順を実施してください。既にセットアップ済みの人はスキップしてかまいません。

#### 2.1.1. Azure Pass

Azure Passを使うと、Azureが使えるようになります。セットアップ手順は[AzurePass有効化手順](http://aka.ms/try-azurepass-jp)をご覧ください

#### 2.1.2. Visual Studio Online

Visual Studio Onlineを使うと、カンバンによるストーリやタスクの管理、CI/CDのビルド、リリースマネジメント（近日公開予定）、負荷テスト
等を実施することができます。チームでこれを使えるようにするためには、次の手順が参考になるでしょう[MicrosoftDevOpsHackathon_VSO](http://1drv.ms/1GlvgC1)


#### 2.1.3. Azure SDK(xPlat)

Azure SDK(xPlat)は、コマンドラインでAzureを操作するためのツールです。Azureを操作したい人や、Chef等のオープンソース系のプロビジョニングツールを
使う時に必要になってくるケースがあります。

手順に関しては [Azure CLIとChefを始めるためのSubscription関係まとめ](http://qiita.com/TsuyoshiUshio@github/items/27bc5e9d7e93214c01f0) のAzure SDK設定の手順までが参考になるでしょう。他には英語ですが [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-connect/) という記事もあります。

Azure SDKを使うためには、事前にAzureを使えるようにしたうえで、azure login / download / import コマンドを上記手順にしたがって使っていく必要があります。

### 2.2. Buildサーバのプロビジョニング

Linuxや、Macをビルドサーバとして使用したい場合は、VSO agentを使ってビルドサーバを構築する必要があります。

#### 2.2.1. 手動のセッティング方法

PartsUnlimitedMRPのアプリケーションを参考に、実際のビルドサーバを構築する手順は次のようになります。もし、自動で構築したいのなら、2.2.2.の手順を参考にしてください。
継続的デリバリや継続的インテグレーションで用いられるビルドサーバは一般的にはデプロイ先と同じようなライブラリ、ミドルウェアなどをインストール・セッティングし、その上でビルドやテストが走るようにします。

（英語のみ）
[Parts Unlimited MRP : HOL - Parts Unlimited MRP App Continuous Integration with Visual Studio Online Build](https://github.com/Microsoft/PartsUnlimitedMRP/blob/master/docs/HOL_Continuous-Integration-with-Visual-Studio-Online-Build/HOL_Continuous-Integration-with-Visual-Studio-Online-Build.md)

#### 2.2.2. Chef Provisionを用いたBuildサーバの自動化手順 (オプション)

手動でビルドサーバを構築するのが面倒な方は、私が作成したVSO agentを入れたビルドサーバの自動化スクリプトをご利用ください。
下記のURLに手順が記述されています。英語ですが、操作手順を追っていけばよいだけになっています。サーバはUbuntuを想定しています。
下記の手順にもありますが、この自動化スクリプトはWindowsでもMacでも動作します。手順はWindowsになっていますが、Azure SDK(xPlat)と
ChefDKがあれば動作するようになっています。

[Visual Studio Online Build Agent Machine for Ubuntu](https://github.com/TsuyoshiUshio/vsoagentserver)

### 2.3. Productionサーバのプロビジョニング

継続的にアプリケーションをデリバリするためには、Productionサーバが必要です。下記の手順で構築していきます。

#### 2.3.1. Productionサーバのプロビジョニング手順

この例ではAzure SDK(xPlat)をつかって、Productionサーバを作成していきます。既にAzure SDK(xPlat)が使用可能になっており、login/download/importが
完了していることを想定しています。お済でない方は2.1.3.をご参照ください。

[Parts　Unlimited MRP :　HOL - Parts Unlimited MRP App Continuous Deployment with Visual Studio Online Build ](https://github.com/Microsoft/PartsUnlimitedMRP/blob/master/docs/HOL_Continuous-Deployment-with-Visual-Studio-Online-Build/HOL_Continuous-Deployment-with-Visual-Studio-Online-Build.md)

残念ながら上記の手順では、継続的にデリバリできるようになっていません。それを可能にするためには、次移行の手順をご参照ください。

#### 2.3.2. SSHをパスワードなしで実行できるようにする

この例では、大変シンプルな方法で上記の継続的デリバリできない問題を解決しています。ここでは、Buildサーバから、Productionサーバに、コマンドを発行する方法を利用しています。
皆さんが実際に本番環境を構築される際はこのような手順ではなく、サーバからプル型でビルドを取得するプル型に変更されることをお勧めいたします。

この作戦を使うためには、SSHのログインをパスワード無しで実施できる必要があります。まずは、buildサーバにログインして次の手順でbuildサーバからproductionサーバにパスワード無しで
ログインできるようにしてみてください。下の例でpuldev5の部分は皆さんのサーバ名に変更してみてください。

```
$ ssh-keygen -t rsa
$ cat .ssh/id_rsa.pub | ssh azureuser@puldev5.cloudapp.net 'cat >> .ssh/authorized_keys'
```

次のテストです。上記のコマンドを発行したら、Productionサーバにパスワード無しでログインできるか試してみましょう。
```
$ ssh azureuser@puldev5.cloudapp.net
puldev5 $ exit
```
できましたね。最後にVSO agentをビルドサーバで起動してみましょう。

```
$ cd myagent
$ node agent/myagent
```

もし、あなたがソースコードに変更を加えたなら、自動的にビルドが走るようになっているはずです。
デプロイが終了したら、次のURLにアクセスしてみましょう。Webページが見れたら完成です。

```
http://<production_server_name>.cloudapp.net:9080/mrp
```


3. 補足
-------------------------

### 3.1. .NET application

.NETアプリケーションを使いたい場合は、PartsUnlimitedを試しましょう。このハッカソンでは、aspnet45ブランチを使うことをお勧めします。

[PartsUnlimited : Getting Started](https://github.com/Microsoft/PartsUnlimited/blob/master/docs/GettingStarted.md)
[PartsUnlimited(日本語)](https://onedrive.live.com/view.aspx?cid=286f38e26945ae87&page=view&resid=286F38E26945AE87!2824&parId=286F38E26945AE87!2821&authkey=!AF94yD02BzMuf9g&app=Word)

### 3.2. FAQ

もし、PartsUnlimitedMRPやPartsUnlimitedで困ったことがあったら、FAQを見ると解決するかもしれません。問題が起きて、サポータに何か聞いたら是非
このFAQに皆さんも追記してみてください。日本語でも英語でも構いません。

[DevOps hackathon FAQ](https://github.com/TsuyoshiUshio/DevOpsHackathon/wiki/FAQ)

[DevOps ハッカソン FAQ](https://github.com/TsuyoshiUshio/DevOpsHackathon/wiki/FAQ_JP)


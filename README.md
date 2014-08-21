# ベーシック サマーハッカソン2014 Webコース

## サーバ準備

ハッカソンで使用するサーバはAWSです。構築するサーバは、基本、チーム毎に1台です。チーム内で代表者がAWSのコントロールパネルを操作して、サーバを構築してください。複数台サーバを使用したい場合は、近くのメンターに聞いてください。

EC2のみで完結するApache + MySQL + PHPな構成を想定しています。他のAWSサービスを利用したい・理想の開発環境がある！という方も、近くのメンターに聞いてください。

### アカウント

- ログイン画面 : [http://aws.amazon.com/jp/](http://aws.amazon.com/jp/)
- ID : イベント会場でアナウンスします
- PW : イベント会場でアナウンスします

## EC2でサーバを立ち上げよう

VPC構成で作るため、手順がすごく多いです…！挫けず頑張りましょう。

### Step1

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_1.png)

コントロールパネルから、「EC2」を選択します。

### Step2

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_2.png)

「Launch Instance」ボタンを押下し、EC2インスタンスを作成します。

### Step3

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_3.png)

Amazon Linux 64bitを選択します。

### Step4

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_4.png)

t2.mediumを選択し、「Next: Configure Instance Details」を押下します。

### Step5

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_5.png)

「Create new VPC」を押下し、VPCを作成します。

### Step6

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_6.png)

- Name : 自分たちのものだと分かるように、適当に名前を入力してください
- CIDR Blocks : 192.168.0.0/16 と入力してください

### Step7

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_7.png)

先ほどの画面に戻り、作成したVPCを選択します。続いて、Subnetを新規作成します。

### Step8

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_8.png)

- Name : 自分たちのものだと分かるように、適当に名前を入力してください
- VPC : 先ほど作成したVPCを選択してください
- CIDR Blocks : 192.168.0.0/20 と入力してください

### Step9

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_9.png)

先ほどの画面に戻り、作成したSubnetを選択します。Auto-assign Public IPは「Enable」を選択してください。それ以外はデフォルトのままで大丈夫です。「Next: Add Storage」を押下し次に進みます。

### Step10

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_10.png)

ここでは、ストレージの設定をしますが、デフォルトのまま次へ進みます。

### Step11

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_11.png)

Valueに自分たちのサーバ名だと分かるように名前を入力してください。

### Step12

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_12.png)

セキュリティの設定をします。

- Security group nameに自分たちの設定だと分かるように入力します
- 「Add Rule」を押下し、Type「HTTP」を選択、右端のSourceは「Anywhere」を選択してください
- 「Review and Launch」を押下し、最終確認画面へ進みます

### Step13

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_13.png)

設定に問題がなければ、EC2インスタンスを起動します。

### Step14

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_14.png)

最後に、サーバにアクセスするための鍵を作成し、ダウンロードします。この鍵は必ずなくさないようにしてください。これで、EC2インスタンスの作成が完了しました。

### Step15

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_15.png)

続いて、VPCのデフォルトゲートウェイを作成します。VPCのページヘ移動し、作成したVPCを選択します。そして「Internet Gateways」を押下し、作成します。名前は適当にわかりやすいものをつけてください。

### Step16

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_16.png)

デフォルトゲートウェイを作成したら、VPCとのヒモ付を行います。作成したデフォルトゲートウェイを選択し「Attach to VPC」を押下し、自身が作成したVPCを選択します。

### Step17

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_17.png)

次にRoute Tableメニューを開きます。作成した自身のVPCのルートテーブルを選択します。下の詳細メニューにある「Routes」タブを開き、「Add another route」を押下しデフォルトゲートウェイを紐付けます。

- Destination : 0.0.0.0/0
- Target : クリックすると、ゲートウェイの選択肢が出ると思います

### Step18

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_18.png)

続いて、隣のタブの「Subnet Associations」を選択し、サブネットを割り当てます。単純にAssociateのチェックボックスを入れて保存するだけです。

以上で、EC2インスタンスとVPCの設定が完了しました。

## Webサーバの構成

今回は、Apache + PHP + MySQLの構成で作ってみましょう。

### サーバへのアクセス

ターミナルを開いて、SSHコマンドでサーバにアクセスをします。鍵認証を行うので、先ほどダウンロードした鍵ファイルを指定します。サーバのIPアドレスは、AWSコントロールパネルのEC2一覧から確認ができます。「Public IP」という項目がIPアドレスです。

```
# 秘密鍵のパーミッションを変更する
$ chmod 600 ~/Desktop/SampleKey.pem

# サーバへアクセスする
$ ssh ec2-user@サーバIPアドレス -i ~/Desktop/SampleKey.pem
```

### ミドルウェアのインストール

yumコマンドを用いて、apacheなどのミドルウェアをインストールしましょう。

```
$ sudo yum install -y apache php php-devel php-mbstring php-xml php-gd php-mysql mysql mysql-server
```

### Apcheの起動

インストールされたApacheを起動します。

```
$ sudo service httpd start
```

これで、Webサーバが出来上がりました。ブラウザを立ち上げて、サーバのIPアドレスにアクセスするとApacheの初期ページが表示されていると思います。表示されない場合は、EC2インスタンスのセキュリティグループで、「HTTP」が設定されているか確認してみましょう。

### MySQLの起動

続いて、データベースMySQLを起動します。

```
$ sudo service mysqld start
```

起動が完了しました。MySQLにアクセスしてみます。

```
$ mysql -u root
```

## Webサービス作成の準備

上記の手順によって、サーバ自体の準備は整いました。次は、Webサービスを作るための準備です。

### ファイルの配置場所

Apacheのデフォルト設定だと、HTMLや画像ファイルなどを置くドキュメントルートは`/var/www/html`です。アクセス権限がrootになっているので、ログインしているec2-user権限にchownコマンド使って変更します。

```
$ cd /var/www/
$ sudo chown ec2-user:ec2-user ./html
$ cd html
$ pwd
/var/www/html
```

続いて、トップページのファイルを適当に作りましょう。

```
$ vi index.php
<?php echo 'Hello'; ?>
```

保存後、再度ブラウザでサーバにアクセスしてみます。設定が正しければ、PHPが動いたページが表示されていると思います。viエディタの使い方がわからない人は、近くのメンターに聞いてください。

### データベースの作成

もし、データベースが必要であれば、データベースを作成しましょう。

```
$ mysql -u root

mysql> create database データベース名 default charset utf8;
Query OK, 1 row affected (0.00 sec)

mysql> use データベース名;
Database changed

mysql> show tables;
Empty set (0.00 sec)
```

これで、データベースが作成できました。
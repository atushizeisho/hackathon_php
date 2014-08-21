# ベーシック サマーハッカソン2014 Webコース

## サーバ準備

ハッカソンで使用するサーバはAWSです。構築するサーバは、基本、チーム毎に1台です。チーム内で代表者がAWSのコントロールパネルを操作して、サーバを構築してください。複数台サーバを使用したい場合は、近くのメンターに聞いてください。

EC2のみで完結するApache + MySQL + PHPな構成を想定しています。他のAWSサービスを利用したい・理想の開発環境がある！という方も、近くのメンターに聞いてください。

### アカウント

- ログイン画面 : [http://aws.amazon.com/jp/](http://aws.amazon.com/jp/)
- ID : イベント会場でアナウンスします
- PW : イベント会場でアナウンスします

## EC2でサーバを立ち上げよう

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_1.png)

コントロールパネルから、「EC2」を選択します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_2.png)

「Launch Instance」ボタンを押下し、EC2インスタンスを作成します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_3.png)

Amazon Linux 64bitを選択します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_4.png)

t2.mediumを選択し、「Next: Configure Instance Details」を押下します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_5.png)

「Create new VPC」を押下し、VPCを作成します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_6.png)

- Name : 自分たちのものだと分かるように、適当に名前を入力してください
- CIDR Blocks : 192.168.0.0/16 と入力してください

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_7.png)

先ほどの画面に戻り、作成したVPCを選択します。続いて、Subnetを新規作成します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_8.png)

- Name : 自分たちのものだと分かるように、適当に名前を入力してください
- VPC : 先ほど作成したVPCを選択してください
- CIDR Blocks : 192.168.0.0/20 と入力してください

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_9.png)

先ほどの画面に戻り、作成したSubnetを選択します。Auto-assign Public IPは「Enable」を選択してください。それ以外はデフォルトのままで大丈夫です。「Next: Add Storage」を押下し次に進みます。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_10.png)

ここでは、ストレージの設定をしますが、デフォルトのまま次へ進みます。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_11.png)

Valueに自分たちのサーバ名だと分かるように名前を入力してください。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_12.png)

セキュリティの設定をします。

- Security group nameに自分たちの設定だと分かるように入力します
- 「Add Rule」を押下し、Type「HTTP」を選択、右端のSourceは「Anywhere」を選択してください
- 「Review and Launch」を押下し、最終確認画面へ進みます

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_13.png)

設定に問題がなければ、EC2インスタンスを起動します。

![](https://raw.githubusercontent.com/basicinc/hackathon_php/master/aws_14.png)

最後に、サーバにアクセスするための鍵を作成し、ダウンロードします。この鍵は必ずなくさないようにしてください。
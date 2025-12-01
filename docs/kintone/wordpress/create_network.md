# ネットワークを構築する
以下のリソースを作成します。

- VPC
- インターネットゲートウェイ
- サブネット
    - Public Subnet x 2
    - Private Subnet x 2
- ルートテーブル
    - Public Route Table
    - Private Route Table
- セキュリティグループ
    - ALB 用
    - WordPress 用
    - MariaDB 用
    - Bastion 用

## VPC を作成する

### VPC インスタンスを起動する
VPC サービスに移動して、VPC を作成します。
以下の図の<span style="color: red; font-weight: bold;">赤丸</span>部分はデフォルトから変更する箇所です。

![](./img/create_vpc.png)

Private Subnet からインターネットへは通信できない状態になっています。

## Private Subnet のルーティングを設定する

### ルートテーブルにインターネットゲートウェイのルートを追加する
Private Subnet のルートテーブルの定義に、インターネットゲートウェイへのルートを追加します。
以下の図のように設定します。

![](./img/modify_private_route.png)

### 各 Private Subnet のルートテーブルを確認する
Private Subnet のルートテーブルは、Subnet ごとに用意されているので、2 つの Private Subnet のルートテーブルそれぞれに対して設定を行います。

## ネットワークリソースを確認する

### VPC リソースマップを表示する
VPC のリソースマップを表示し、Private Subnet からインターネットゲートウェイへの通信経路が設定されていることを確認します。

![](./img/created_vpc.png)

## セキュリティグループを作成する
以下のセキュリティグループを作成します。

### ALB 用セキュリティグループを作成する
- セキュリティグループ名: alb-sg
- 説明: for ALB
- VPC: 作成した VPC を選択
- インバウンドルール:
    - HTTPS(443) を Anywhere-IPv4 (0.0.0.0/0) からの通信を許可
- アウトバウンドルール: 変更なし（全て許可）

### WordPress 用セキュリティグループを作成する
- セキュリティグループ名: wordpress-sg
- 説明: for WordPress
- VPC: 作成した VPC を選択
- インバウンドルール:
    - HTTP(80) を ALB 用セキュリティグループからの通信を許可
- アウトバウンドルール: 変更なし（全て許可）

### MariaDB 用セキュリティグループを作成する
- セキュリティグループ名: mariadb-sg
- 説明: for MariaDB
- VPC: 作成した VPC を選択
- インバウンドルール:
    - MYSQL/Aurora(3306) を WordPress 用セキュリティグループからの通信を許可
- アウトバウンドルール: 変更なし（全て許可）

## セキュリティグループの設定を確認する

### ALB 用セキュリティグループ
<img src="./img/sg-alb.png" width="600">

### WordPress 用セキュリティグループ
<img src="./img/sg-wordpress.png" width="600">

### MariaDB 用セキュリティグループ
<img src="./img/sg-mariadb.png" width="600">

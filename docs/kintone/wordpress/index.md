# kintone と WordPress の連携
WordPress と kintone を連携させることで、kintone のデータを WordPress サイト上で表示したり、WordPress から kintone にデータを送信したりすることが可能になります。

ここでは、問い合わせフォームを kintone と連携させる方法について説明します。

## システム構成
今回作成するシステムの構成図は以下の通りです。
![](./img/system.drawio.svg)

## 構築手順
以下の手順でシステムを構築します。

1. ネットワークを構築する
2. bastion サーバーを構築する
3. MariaDB サーバーを構築する
4. WordPress サーバーを構築する
5. ALB を構築する
6. https 化する
7. kintone と連携する

### 1. ネットワークを構築する
- VPC
- インターネットゲートウェイ
- サブネット
    - Public Subnet x 2
    - Private Subnet x 2
- ルートテーブル
    - Public Route Table
    - Private Route Table
- セキュリティグループ
    - bastion 用
    - ALB 用
    - WordPress 用
    - MariaDB 用

### 2. bastion サーバーを構築する

### 3. MariaDB サーバーを構築する

### 4. WordPress サーバーを構築する

### 5. ALB を構築する

### 6. https 化する

### 7. kintone と連携する

$33.2
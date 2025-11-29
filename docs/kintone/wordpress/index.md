# kintone と WordPress の連携
WordPress と kintone を連携させることで、kintone のデータを WordPress サイト上で表示したり、WordPress から kintone にデータを送信したりすることが可能になります。

ここでは、問い合わせフォームを kintone と連携させる方法について説明します。

## システム構成
今回作成するシステムの構成図は以下の通りです。
![](./img/system.drawio.svg)

## 構築手順
以下の手順でシステムを構築します。

1. [ネットワークを構築する](./create_network.md)
2. [MariaDB サーバーを構築する](./create_mariadb.md)
3. [WordPress サーバーを構築する](./create_wordpress.md)
4. [ターゲットグループを作成する](./create_tg.md)
5. [証明書を発行する](./create_cert.md)
6. [ALB を構築する](./create_alb.md)
6. kintone と連携する

### 5. ALB を構築する

### 6. https 化する

### 7. kintone と連携する

$33.2
# MariaDB サーバーを構築する
EC2 サービスで MariaDB サーバー用の EC2 インスタンスを作成します。

1. EC2 インスタンスを起動する

    設定値は以下の通りです。

    - 名前とタグ
        - 名前: mariadb
    - アプリケーションおよび OS イメージ (Amazon マシンイメージ): 変更なし（Amazon Linux 2023）
    - インスタンスタイプ: 変更なし（t3.micro）
    - キーペア:
        - キーペア名: vockey を選択
    - ネットワーク設定: 「編集」を押して以下を設定する
        - VPC: 作成した VPC を選択
        - サブネット: Private Subnet のうちの 1 つを選択
        - パブリック IP の自動割り当て: 有効化を選択（）
        - ファイアウォール（セキュリティグループ）: 既存のセキュリティグループを選択し、MariaDB 用セキュリティグループを選択
    - ストレージを設定: 変更なし
    - 高度な詳細:
        - IAM インスタンスプロファイル: LabInstanceProfile を選択

2. MariaDB に接続する

    EC2 インスタンスの一覧から mariadb を選択し、「接続」ボタンを押してセッションマネージャーで接続します。

3. MariaDB をインストールする

    ```bash
    sudo dnf update -y
    ```

    ```
    sudo vi /etc/yum.repos.d/MariaDB.repo
    ```

    ```
    [mariadb]
    name = MariaDB
    baseurl = https://mirror.mariadb.org/yum/11.4/rhel9-amd64
    gpgkey=https://mirror.mariadb.org/yum/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    enabled=1
    ```

    ```bash
    sudo dnf clean all
    sudo dnf makecache
    sudo dnf install -y mariadb-server
    ```

    ```
    sudo systemctl start mariadb
    sudo systemctl enable mariadb
    ```

mariadb サーバをセットアップする

    ```bash
    sudo mariadb-secure-installation
    ```

    以下のように表示されたら、Enter キーを押して進みます。

    ```plaintext
    Enter current password for root (enter for none): 
    OK, successfully used password, moving on...

    Setting the root password or using the unix_socket ensures that nobody
    can log into the MariaDB root user without the proper authorisation.

    You already have your root account protected, so you can safely answer 'n'.

    Switch to unix_socket authentication [Y/n]
    Enabled successfully!
    Reloading privilege tables..
     ... Success!


    You already have your root account protected, so you can safely answer 'n'.

    Change the root password? [Y/n]
    New password:
    Re-enter new password:
    Password updated successfully!
    Reloading privilege tables..
     ... Success!

    By default, a MariaDB installation has an anonymous user, allowing anyone
    to log into MariaDB without having to have a user account created for
    them.  This is intended only for testing, and to make the installation
    go a bit smoother.  You should remove them before moving into a
    production environment.

    Remove anonymous users? [Y/n]
     ... Success!

    Normally, root should only be allowed to connect from 'localhost'.  This
    ensures that someone cannot guess at the root password from the network.

    Disallow root login remotely? [Y/n]
     ... Success!
    By default, MariaDB comes with a database named 'test' that anyone can
    access.  This is also intended only for testing, and should be removed
    before moving into a production environment.

    Remove test database and access to it? [Y/n]
    - Dropping test database...
    ... Success!
    - Removing privileges on test database...
    ... Success!

    Reloading the privilege tables will ensure that all changes made so far
    will take effect immediately.

    Reload privilege tables now? [Y/n]
    ... Success!

    Cleaning up...

    All done!  If you've completed all of the above steps, your MariaDB
    installation should now be secure.

    Thanks for using MariaDB!
    ```
### MariaDB に接続する  
`mysql -uroot -p` を実行すると `Enter password:` と表示されるので[mariadb をセキュアな状態に設定](#7-mariadb)で設定したパスワードを入力する

### データベースとデータベースに接続するユーザーを作成

データベースを作成  

```
create database `wordpress-db`;
```

データベースに接続するためのユーザーを作成  

```
create user 'hjuser'@'%' identified by 'password00';
```
    * ユーザーID: hjuser
    * パスワード: password00
    
作成したユーザーにデータベース (wordpress-db) へのアクセス権限を付与  

```
grant all privileges on `wordpress-db`.* to 'hjuser'@'%';
```

変更を有効にする  

```
flush privileges;
```

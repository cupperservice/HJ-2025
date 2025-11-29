# bastion サーバーを構築する
EC2 サービスで bastion サーバー用の EC2 インスタンスを作成します。

1. EC2 インスタンスを起動する

    設定値は以下の通りです。

    - 名前とタグ
        - 名前: bastion
    - アプリケーションおよび OS イメージ (Amazon マシンイメージ): 変更なし（Amazon Linux 2023）
    - インスタンスタイプ: 変更なし（t3.micro）
    - キーペア:
        - キーペア名: vockey を選択
    - ネットワーク設定: 「編集」を押して以下を設定する
        - VPC: 作成した VPC を選択
        - サブネット: Public Subnet のうちの 1 つを選択
        - パブリック IP の自動割り当て: 有効化を選択
        - ファイアウォール（セキュリティグループ）: 既存のセキュリティグループを選択し、bastion 用セキュリティグループを選択
    - ストレージを設定: 変更なし
    - 高度な詳細:
        - IAM インスタンスプロファイル: LabInstanceProfile を選択

2. セッションマネージャで bastion サーバーに接続する

    EC2 インスタンスの一覧から bastion を選択し、「接続」ボタンを押してセッションマネージャーで接続します。

    以下のようにコマンドプロンプトが表示されれば、OK です。  
    <img src="./img/ssm.png" width="600">

    「終了」を押して、セッションを終了します。

## 秘密鍵を用意する
bastion サーバ上に秘密鍵を用意する

1. bastion サーバにセッションマネージャーで接続する

2. Home ディレクトリに移動する

    ```bash
    cd /home/ec2-user
    ```

    pwd コマンドで /home/ec2-user にいることを確認してください。

    ```
    sh-5.2$ pwd
    /home/ssm-user
    ```

3. 秘密鍵を作成する
AWS Academy Learner Lab のページの AWS Details で SSH key の「Show」を押して秘密鍵を表示する

    <img src="./img/private_key.png" width="600">

    以下のように秘密鍵が表示されるので、クリップボードにコピーします。

    `-----BEGIN RSA PRIVATE KEY-----`
    から
    `-----END RSA PRIVATE KEY-----`
    までをすべてコピーします。

    ```
    -----BEGIN RSA PRIVATE KEY-----
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    ....
    -----END RSA PRIVATE KEY-----
    ```

4. bastion サーバ上で秘密鍵ファイルを作成する

    ```bash
    vi vockey.pem
    ```

    vi エディタが起動するので、`i` キーを押してインサートモードにして、先ほどコピーした秘密鍵を貼り付けます。

    貼り付けが終わったら、`Esc` キーを押してコマンドモードに戻り、`:wq` と入力して Enter キーを押して保存して終了します。

5. 秘密鍵ファイルのパーミッションを変更する

    ```bash
    chmod 400 vockey.pem
    ```

    パーミッションを変更しないと、SSH 接続できないので注意してください。

    秘密鍵ファイルのパーミッションの確認  
    以下のように Owner のみが読み取り可能になっていれば OK です。

    ```bash
    ls -l vockey.pem
    -r--------. 1 ssm-user ssm-user 1676 Nov 27 07:57 vockey.pem    ```
    ```
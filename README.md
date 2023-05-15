# server_setup
ubuntu server setup

# Server　使い方　Ubuntu:20.04
***
### 公開鍵をserver上に登録した状態を想定
## サーバに入る準備

1. `.ssh`以下に秘密鍵`id_ed*`を配置する
2. `.ssh`以下に`touch config`でファイルを作成し以下の内容を記述
    > StrictHostKeyChecking no  
     Host server  
    Hostname SeverIPアドレス
    user username  
    IdentityFile ~/.ssh/id_ed*

    これを記述しておくとログインの際のコマンドが楽になる

## サーバーに入る
1. terminal(端末)に`ssh`につづけてHostで指定したHost名を入力しサーバーに入る  
    今回の例だと`ssh server`と入力後パスワードを入力しサーバーに接続できる

## pycharmでローカル端末のファイルをサーバへ転送(同期)
1. pycharmを起動し`Ctrl + Alt + s`で設定を開く
2. `ビルド、実行、デプロイ > デプロイ`から+を押し`SFTP`を選択
3. 新規サーバー名を好みの名前に設定
4. SSH構成の`...`を選択しホスト名にserverのIPアドレス  
   ユーザ名,パスフレーズにはサーバー上に登録したものを入力する  
   認証タイプを鍵ペアに設定し,秘密鍵ファイルのパスを`.ssh`以下に配置した秘密鍵のパスに設定  
   入力が完了したら`OK`を選択
5. ルートパスを`/home/server上に登録したusername`にしておく
6. マッピングの欄からローカルパスをローカル環境の作業ディレクトリに設定  
   `/home/student/Programs`など
7. デプロイパスはサーバ上の同期させたいディレクトリを設定  
   サーバー上の`/home/username`にProgramというディレクトリを作成した場合  
   `/Programs`と入力する
8. 設定を閉じ,ツールからデプロイを選択.  
   サーバーとのファイルの操作が行える

## サーバー上でのdockerでエラー

サーバー上でnvidia ngcからimageをpullする際に以下のエラーが出たときにしたこと  
`Error response from daemon: Get https://nvcr.io/v2/: unauthorized: authentication required`

1. 端末上で`su user`と入力しuserに切り替える
2. `sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf`と入力し以下の内容を記述  
   >Environment=http_proxy="http://proxy.itc.kansai-u.ac.jp:8080/"  
    Environment=https_proxy="http://proxy.itc.kansai-u.ac.jp:8080/"  
   
    変更を保存し,閉じる
3. 次に以下のコマンドを入力  
   ```angular2html
    systemctl daemon-reload
    systemctl restart docker
   ```  
   ユーザーの選択を要求されるので,ユーザーを選択し適切なパスワードを入力

# 第五回の講義について  
  
  
* OS:Windows。ターミナルはTera Termを使用しています。  
  
## EC2へ環境構築  
1. 依存パッケージのインストール  
sudo yum update(install)  ｙで返答。  
2. パッケージ、ライブラリインストール  
sudo yum -y install gcc-c++ make patch git curl zlib-devel openssl-devel ImageMagick-devel readline-devel libcurl-devel libffi-devel libicu-devel libxml2-devel libxslt-devel  
3. Git Hubのインストール  
sudo yum install git -y  
git -v  
* サンプルアプリをクローンする。  
git clone　アプリのURL  
* README.mdのファイルをcatで参照。  
4.Node.jsインストール  
* AWSのリポジトリに追加。  
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -  
sudo yum install nodejs
* nvm -vコマンドでインストールできているか確認。  
認識されなかったので、以下のコマンド実行。  
source ~/.bash-profile  
.bash-profile にnvmコマンドを認識させる。  
-v を実行して  
* バージョン指定でインストール。  
nvm install 17.9.1  
-vで確認。  
5. rbenv インストール  
git clone https://github.com/rbenv/rbenv.git ~/.rbenv  
* rbenv -v 認識されなかったので以下のコマンド実行。
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile  
* rbenvのコマンドが使えるように登録。  
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile  
* シェルに定着するために、ログインシェルとして再起動。  
exec $SHELL -l  
* ruby-buildインストール。  
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build  
rbenvのプラグインとして動作する。  
rbenv rehash  
* インストールされたかの確認。  
rbenv -v  
6. Railsインストール  
gem install rails -v 7.1.3.2  
必要なバージョンを指定。  
rails -v
7. Bundlerインストール  
gem install bundler -v 2.3.14  
bundler -v  
8. yarn インストール  
yarn install  
* バージョンの指定。  
npm install --globalyarn@1.22.19  
yarn -v  
9. RDS(My SQL)インストール  
＊パッケージの更新。  
sudo yum update -y  
* mysqlのインストール。  
sudo yum install mysql  
* すでにクライアントがある場合、以下のRDSに接続。  
mysql -u admin -p -h データベースのエンドポイント  
* myspl クライアント開発ライブラリのインストール。  
sudo yum install -y mysql-devel  
* Ruby開発ヘッダのインストール。  
sudo yum install -y gcc ruby-devel  
* 特定バージョンのmysql2 gemをインストールする。  
gem install mysql2 -v '0.5.6'  
10. RDSへの接続  
* サンプルアプリに移動。  
cd raisetech-live8-sample-app  
bundle install  
* config/database.ymlファイルにて設定。  
username:<your_rds_username>  
password:<your_rds_password>  
host:<your_rds_endpoint>  
post:3306  
* Escの後、;wqにて保存。  
* サンプルアプリでデータベースを準備。  
bin/rails db:create db:migrate  
11. webpackとその依存関係のインストール  
yarn add webpack webpack-cli  
12. 組み込みサーバー(puma)でサンプルアプリを作動  
bin/setup  
bin/dev  
13. AWSサイトより、ec2についたセキュリティのインバウンドルール変更  
*ポート3000を追加。  
14. ブラウザからアクセス　　
http://IPアドレス:3000  
  
画像  
  
## 画像表示ツールのインストール  
1, vips(ファイル)をインストール  
wget URL vips-8.15.3 tar.xz  
2.  凍結していて中身が機能しないので解凍  
tar -xf vips -8.15.3 tar-xz  
* ls で確認。　
3. 依存パッケージをインストール  
cd vips -8.15.3 に移動。  
* mesonインストール。  
sudo pip3 install meson  
* PATHに追加。  
export PATH=$PATH:/usr/local/bin  
meson -v
* ninjaインストール。  
buildをインストールしているけれどうまくパスが通らなかったので、手動でインストール。  
wget URL ~~~~~v1.11.1/ninja-linux.zip
* 解凍  
unzip ninja -linux.zip  
* libvipsライブラリを認識させる。  
sudo Id config  
* PATH に含まれたか確認。  
echo "/usr/local/lib64"| sudo tee /etc/Id.so.conf.d/vips.conf  
vips -v  
* webpack をインストールしていたので、mini_magickをインストールでもよかったのですね！！  
## 画像をTera Termに入れる  
1. ec2に接続  
2. Tera Termを使って画像ファイルをアップロード  
* Tera Termのウィンドウ上部メニューから"ファイル"を選択。  
* "SSH SCP"を選択。  
3. 画像ファイルをアップロードする  
* SCPのダイアログが開いたら、送信元の欄に"果物"の画像ファイルを指定。  
* ドラッグ＆ドロップ使用可能。  
* 宛先の欄には、ファイルを保存するec2上のパスを指定。  
(例)　/home/ec2-user/  
* "send送信"ボタンをクリックすると、ファイル画像がec2にアップロードされる。  
4. ファイルの確認  
* ls 実行。選択した画像の名前.OIP.jpgと表示されたらOK！  
5. 画像ファイルをサンプルアプリのpublicディレクトリーに移動  
mv /home/ec2-user/画像ファイル.OIP.jpg ~/raisetech-live8-sample-app/public/  
6. 写真内容を記述  
* ディレクトリーに移動。  
cd raisetech-live8-sample-app  
cd app/views/fruits  
* show.html.slimに追加内容記述。  
p  
 strong Additional Image:  
 =image_tag('fruit.IOP.jpg',alt:'Fruits')  
7. アセットパイプラインに画像ファイルを追加  
cp path/to/fruit.OIP.jpg/home/ec2-user/raisetech-live8-sample-app/app/assets/images  
* ls で確認  
8. ブラウザで確認。  
  
## 組み込みサーバーとUNIXsocketを使ってrailsアプリの動作確認  
* コンピュータ内で動作しているプログラム同士がデータをやり取りするための方法の一つ。今回はpumaとネットワークを接続させるために使用。  
1. サンプルアプリに移動  
cd raisetech-live8-sample-app  
2. config/puma.rbのファイルに設定を追加  
bind "unix:///home/ec2-user/raisetech-live8-sample-app/tmp/sockets/puma.sock"  
3. サーバを起動  
rails server --binding unix://bind "unix://raisetech-live8-sample-app/tmp/sockets/puma.sock"
4. puma起動  
bundle exec puma -C config/puma.rb  
* Lisning 3000  
5. ブラウザで確認  
http://IPアドレス:3000  
  
画像  
  

  

  




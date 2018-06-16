﻿--------------------------------------------------------------------------------------
最新版ver2.1  
■更新履歴  
06/15：新着ツイートに画像アップ機能アップデート。出題時に一言コメント投稿可能に。秘密の部屋のタイトルを変更可能に。github対応。その他諸々修正。  
06/08：あなたの新着ブクマにコメントが出ないバグを修正。ミニメの日付の一部が表示されないバグを修正。  
06/04：出題時闇スープが反映しないバグを修正。「運営者の方へ.txt」を修正。  
06/01：OPENウミガメ2.0公開。  
  
--------------------------------------------------------------------------------------  
■phpのバージョンについて  
phpのバージョンは7以上にしてください。  
  
■導入方法  
サーバーに「open_umigame」フォルダの中身をアップロードし、下記の設定をすることで動作します。  
  
■導入時にやること１  
\app\Config\core.php  
の237～242行目にある、  
    Configure::write('Security.salt', 'ランダムな英数字を入力');  
    Configure::write('Security.cipherSeed', 'ランダムな数字を入力');  
を入力してください。(既に文字列が入ってますが、セキュリティのため必ず変えるようにしましょう)  
  
■導入時にやること２  
\app\Config\database.php  
の81～88行目にある、  
    public $default = array(  
        'datasource' => 'Database/Mysql',  
        'persistent' => false,  
        'host' => '',  
        'login' => '',  
        'password' => '',  
        'database' => '',  
        'prefix' => '',  
        'encoding' => 'utf8',  
    );  
に、予め設定したデータベースのサーバー情報を入力しましょう。  
  
■導入時にやること３  
\app\Config\tmp  
tmpフォルダ以下のパーミッションを７７７に変更しましょう。  
  
■導入時にやること４  
\app\Config\bootstrap.php  
にて定数を設定しましょう。  
デフォルトの値が設定されていますが、秘密の部屋のキー暗号は変えておきましょう。  
  
--------------------------------------------------------------------------------------  
  
※これで一応サイトは開けますが、cronの設定とtwitterの設定もしておきましょう。  
  
■cronの設定方法  
cronとは？  
レンタルサーバーで標準につくことの多い機能で、決まった時間にアクションを起こしてくれる機能です。  
OPENウミガメ2.0では、cronを使い、1日1回確認する機能・1時間に1回確認する機能などを設定しています。  
  
\app\Console\CommandlateShell.php  
にcronで動作する内容があります。(いじらなくてOKです。何か追加したい場合等)  
  
◎レンタルサーバー側の設定（こちらを設定するだけで動きます）  
例：xserverの場合  
0****  
/usr/bin/php（バージョン） /home/〇〇/ドメイン/public_html/app/Console/cake.php Late cron1hour  
00***  
/usr/bin/php（バージョン） /home/〇〇/ドメイン/public_html/app/Console/cake.php Late cron1day  
  
■twitter通知の設定方法  
https://yosiakatsuki.net/blog/create-twitter-application/  
コチラのサイトを参考にし、Twitterアプリケーション登録を行いましょう。  
そして登録すると出てくるキーを、それぞれ  
\app\Config\bootstrap.php  
の20～26行目に入力しましょう。  
  
※twitterの文面は  
\app\Controller\MondaiController.php  
の1038～1198行で設定できますが、twitterのツイート文字数が140を超えるとツイートしなくなるので注意しましょう。  
  
画像付きツイートをする場合は　webroot\font\　に各々ダウンロードしたfontを追加しましょう。  
サンプルサイトのらてらて鯖は以下のフォントを使っています。  
源ノ明朝　http://fontfree.me/2597  
--------------------------------------------------------------------------------------  
■ローカルルールと利用規約を設定しましょう。  
\app\View\Main\rule.ctp  
\app\View\Main\kiyaku.ctp  
こちらのふたつを設定しましょう。  
  
■称号を作りましょう。  
まず「★称号付与権」を作りましょう。名前を変えたい時は、\app\Config\bootstrap.phpから変えれます（同じ名前にしましょう）  
管理人称号に能力はついていません。  
  
■運営者権限の隠しステータス  
(Userテーブルのflgカラム)  
flgを2にすると運営者権限がつき、adminページに入ることができます。  
http://〇〇.com/admin　がURLです。  
そのほかにも問題ページで「出題者のみに表示チャット」が運営者権限で閲覧できます。  
  
■問題の非公開設定の隠しステータス  
(Mondaiテーブルのdeleteカラム)  
・deleteを2にすることで通常の「非公開」  
・deleteを3にすると「規約違反のため[強制非公開]設定」とタイトルに表示される。(出題者は操作不能)  
・deleteを4にすると出題者以外に一覧に表示されない[完全非公開]設定になります。(出題者は操作不能)  
※データベースから直接いじる事になります。  
  
■全ページをメンテナンス画面にする方法  
\app\Config\routes.php  
の32行目にある、  
//Router::connect('/*', array('controller' => 'main', 'action' => 'maintenance', 'home'));  
をコメントアウトしてください。トップページ以外はメンテ画面に切り替わります。  
  
■なんか動作しなくなった？  
\app\Config\core.php  
の34行目にある、  
Configure::write('debug', 0);  
を０→１にしてアップロード、動作したら０に戻しましょう。  
１にするとエラー文が表示されるので、検索などして解決してください。  
CAKEPHPで作っているので、「CAKEPHP　エラー文」と検索することで同じような症状が出るかと思います。  
  
■CSSを変えたいんだけど…  
OPENウミガメ2.0ではKoalaを使ったSCSSでCSSを設定しています。  
CSSフォルダのimport.cssを変えるのではなく、scssフォルダの中身を変え、import先をcssフォルダに設定しましょう。  
Koalaの使い方などはググってください（簡単かつ入れ子や変数が使えて便利です）  
  
--------------------------------------------------------------------------------------  
  
導入に関して分からないことは   
Twitterアカウントの「@suiheinet」にて聞いてください。  

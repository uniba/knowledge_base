pushやpullの度にUsernameとPasswordを聞かれるときの対処法
--------------------------------------------------------
ある時から、ターミナルでのpush/pullで毎回UsernameとPasswordを要求されるようになった。。  
Aptanaでも"Please provide HTTP authenticationなんたら..."というプロンプトが表示され続けるようになり、どうにもやりづらい。  
公開鍵の設定は問題なさそうだし、configかな？と思いgit config --global うんたらとするも、解決せず。  
  
注意深くググッてみると、どうやらGitHub for Macがいたずらをしているらしい。  
そう言えば、これを使う前は特に問題が起こらなかった気が。。。  
という訳で、ローカルの該当リポジトリをいったん全て削除したあと、ターミナルからgit cloneしなおす。  
その後push/pull、Aptanaでの作業を始めてみると、見事に解決していました。  
  
GitHub for Macだけで管理していくならあまり問題にならなさそうな事だけど、  
コンフリクト対応やもう少し込み入った作業をする際には結局ターミナルでの作業が必要になり結果煩わしい事になる。。  
  
ちなみにGitHub for Macのバージョンは1.0.6。新しいやつで治ってるかは知らない。  
  
hideyukisaito

=========

追記
--------

GitHub for Mac の .git/config の origin のリポジトリ URL が https プロトコルになっているのが原因っぽいです。
git プロトコルに変更するとコマンドライン経由でも鍵認証で push/pull 出来るようになりました。

http://null.ly/post/15822446389/github-for-mac-clone-repository-username?ref=nf

ちなみに現時点最新版の GitHub for Mac でも https で clone してしまうようです。

Seiya Konno

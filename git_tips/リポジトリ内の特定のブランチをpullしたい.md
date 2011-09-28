リポジトリ内の特定のブランチをpullしたい
----------------------------------------
git cloneした後、リポジトリ内の他のブランチをpullする時は

    $ git pull origin pullしたいリモートブランチ名:ローカルブランチ名

例えば、リモートにあるgh-pagesというブランチをローカルのtestというブランチに持ってきたいときはこう。

    $ git pull origin gh-pages:test



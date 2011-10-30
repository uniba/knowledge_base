# Mongoid の日付回りの扱いについて
そもそも、MongoDB は [PHP: MongoDate - Manual](http://php.net/manual/ja/class.mongodate.php) によると

> MongoDB は、日付データをエポックからの経過ミリ秒数で格納します。 つまり、日付にはタイムゾーンの情報が 含まれないということです。 タイムゾーンが必要なら、別のフィールドを用意する必要があります。 また、データベースとの間でドキュメントをやりとりすると、 ミリ秒より細かい単位の情報は失われてしまいます。
ということになっているらしい

[#878: Mongoid ignores Time.zone also with use_utc: true (2.0.1 and master) - Issues - mongoid/mongoid - GitHub](https://github.com/mongoid/mongoid/issues/878)

上記の情報を参照して、Mongoid の設定ファイル mongoid.yml に

    use_utc: false
    use_activesupport_time_zone: true

同様に、各環境の設定ファイルに

    config.time_zone = "Tokyo"

という記述をしてみたが、これだけでは充分ではない。下記 URL にある通り

[#1135: No timezone conversion for DateTime fields? - Issues - mongoid/mongoid - GitHub](https://github.com/mongoid/mongoid/issues/1135)

field のデータ型を指定をする際、下記の3カラムを選択する事になるが

* Date
* DateTime
* Time

このうち、DateTime を使うと TimeZone の設定が読まれないらしく、常に UTC でデータが帰ってくるようになっていた。

    class Version
      include Mongoid::Document
      include Mongoid::Timestamps

      # 時刻情報は失なわれるが、日付は TimeZone に従った値となる
      field :d_field, :type => Date
      
      # なぜか常に UTC 扱い
      field :dt_field, :type => DateTime

      # TimeZone に従った値となる
      field :t_field, :type => Time
    end

しかし、もう一つ落し穴があった

    ruby-1.9.2-p290 :008 > version[:published_at]
     => 2011-10-29 22:43:24 UTC
    ruby-1.9.2-p290 :009 > version.published_at
     => 2011-10-30 07:43:24 +0900

と、メソッドでアクセスした際には TimeZone の指定が効くが、Hash でアクセスしてしまうと UTC に戻ってしまう。
そのため、変数の値を元にしてカラムにアクセスするような場合には

    name_var = :published_at
    some_class[name_var]

とする事が多いが

    name_var = :published_at
    some_class.send(name_var)

のように、send メソッド経由でアクセスする必要がある。

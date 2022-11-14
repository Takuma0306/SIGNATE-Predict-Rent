# 賃貸物件の家賃予測
https://signate.jp/competitions/264

### 2022/11/02
nb001

今日から賃貸物件の家賃予測に挑戦。

まずは各カラムやデータ全体についての把握。

そこから軽く前処理に取り掛かった。

とりあえず築年数をint型にしておいた。

今日気付いたのは、表記揺れが大きいこと。

2階／3階建と書いてあるものもあれば、3階建のものもあれば、1階／3階建 (地下1階)と書いてあるものもある。

これはたぶん他のカラムにも言えることだと思う。

1つずつ丁寧に処理していく必要がありそう。

### 2022/11/03
nb001

今日はバイトの休憩中に設備ごとに備わっているかを表すカラムを作った。

専用バスがあるのか、専用トイレがあるのか、浴室乾燥機はついているのかなど。

「専用バスかどうか」と「共同バスかどうか」を表すカラムを両方作ったけど、片方いらないかも、、、

キッチンのところにも同じような処理をしていきたい。

今どんな設備があるのかを調べるところで、csvファイルに出力してから消していって確認するアナログな手法で確認してるからそこをどうにかしたい。


### 2022/11/04
nb001

今日はキッチンと放送・通信、室内設備にも2022/11/03と同様の処理をした。


### 2022/11/05
nb001

今日は築年数を何か月という単位に揃え、所在階、駐車場のカラムの処理をした。

後処理ができていないカラムは所在地、アクセス、間取り、方角、周辺環境、建物構造、契約期間。

だいぶ残っているので頑張って進めたい。

早めにモデルだけ組んで1日1回は提出するようにしたい。


### 2022/11/06
nb001

今日はアクセスから最寄り駅、近い駅3つまでの徒歩での時間を取り出した。

また、所在階に不備があったのでそこを修正した。

具体的には2階建などが住居階が2、建物が欠損値になってしまっていた。

夜に建物構造の数が少ない構造をその他にまとめる処理と、周辺にどのような施設があるかをまとめた。

残りは所在地、間取り、方角、契約期間。


### 2022/11/07
nb001

今日はレポートをやる時間が多くあまり進めることができなかったが、所在地、契約期間について特徴量生成に取り組んだ。

所在地は区ごとに緯度経度を与えた。

契約期間については大まかに同じものをまとめる処理をした。


### 2022/11/08
nb001

今日は実験 + ノートまとめが忙しかったのであまり時間がなかった。

kanariさんが1回目の提出を行っていたので、提出してみたくなってLightGBMの準備をした。

明日は初期パラメーターの設定、パラメーターチューニングに取り組む。

### 2022/11/09
nb001

遂に1回目のsubmitを終えた。

スコアが21370、順位が359位。

ここから特徴量作成やパラメーターチューニングなどに取り組んでいきたい。

間取りのところを修正して2回目のsubmit。

スコアが21099、順位が351位。

nb002

一部屋当たりの面積を求めるカラムを作って3回目のsubmit。

スコアが19440、順位が277位。


### 2022/11/10
nb002

今日は前処理部分を見やすくした。

また、所在地を区だけでなく、区以降も取り出した。


### 2022/11/11
nb003

今日はスクレイピングに挑戦して区ごとの平均地価を取得し、4回目のsubmit。

スコアが18458、順位が232位。

その後、高級住宅街からの距離、有名な建物からの距離を加え、5回目のsubmit。

スコアが17730、順位が203位。


### 2022/11/12
nb003

今日はまず、区ごとに一人当たりの個人都民税を求めた。

その後、区ごとの犯罪数を表すカラムを作成し、6回目のsubmit。

スコアが100197。

なぜか誤差がすごく大きくなった。

犯罪率を表すカラムを消して、7回目のsubmit。

スコアが100280。

個人都民税を表すカラムを消して、外れ値を削除し、8回目のsubmit。

スコアが21486。

個人都民税を表すカラム、犯罪数を表すカラムを入れると誤差が大きくなる。

また、外れ値も触らないほうが良い。

ちなみになぜか手元と評価関数だと過去最高スコアが出ている。。。。



### 2022/11/13
nb003

カラム名を変更し忘れていたので、変更して9回目のsubmit。

スコアが17668、順位が200位。


### 2022/11/14
nb003

おんなじ建物でも欠損値になっていたり、なってなかったり、、、

それを解決するためにgroupbyを用いて同じ建物と思われる部屋の設備でそれぞれの最大値をとって代入して10回目のsubmit。

スコアが102715。

やはり、trainデータとtestデータで区に偏りがある。

nb004

区ごとのモデルを作成して、最後に結果を結合させて、11回目のsubmit。

スコアが22973。

そこから区、地域、最寄り、間取り、建物構造別の平均面積、平均総階数、平均築年数を出して、(面積)÷(区別の平均面積)と(面積)-(区別の平均面積)をそれぞれ出した。

これにより、相場よりも少し狭いから家賃を安めにしておこうとか、相場より古いから安めにしておこうとなってくれることを願って12回目のsubmit。

スコアが105,000。

やはり場所に偏りがあるのか、、、

建物の構造や設備、周辺施設に注目する必要がありそう。




### 2022/11/15
nb005

区、地域にかかわるものを取り除き13回目のsubmit。

しかしスコアは102306。

リークしてるっぽい？？

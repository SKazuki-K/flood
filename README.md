# flood

Flood simulator on top of WebGL and Leaflet

数値標高モデル（DEM; Digital Elevation Model) で表現される地形上で水の移動を計算することで、
浸水状態からの水の移動・収束の状況をアニメーションとして可視化するアプリです。

![渋谷区周辺](https://user-images.githubusercontent.com/12029629/51830596-89941b80-2333-11e9-84dd-04c09f8c83a2.png)

# アプリの操作方法

## Step1

Web ブラウザで次の URL を開きます

* <https://frogcat.github.io/flood/>

**Note**

以下の環境で動作確認を行っています

- Google Chrome 71.0.3578.98 (Windows 10)
- Microsoft Edge (Windows 10)
- Google Chrome 71.0.3578.98 (Android 8.0.0)

アニメーションの実行速度（フレームレート）は実行環境のソフトウェア・ハードウェアに依存します。



## Step2

まずシミュレーションしたい場所に移動します。すると画像のように「Click to Run」の帯が表示されるのでこれをクリックするとシミュレーション開始です。

![スタート](https://user-images.githubusercontent.com/12029629/51830528-69fcf300-2333-11e9-945d-3c8d9fd939bb.png)

## Step3

最初は地図中の全面を均一に水が覆われた状態ですが

![初期](https://user-images.githubusercontent.com/12029629/51830550-74b78800-2333-11e9-80aa-98295debb3c4.png)

時間とともに地形の高低に応じて水が移動していきます

![中盤](https://user-images.githubusercontent.com/12029629/51830565-7bde9600-2333-11e9-899b-cd7b84790513.png)

![終盤](https://user-images.githubusercontent.com/12029629/51830596-89941b80-2333-11e9-84dd-04c09f8c83a2.png)

![収束](https://user-images.githubusercontent.com/12029629/51830603-8ef16600-2333-11e9-8f5a-0aef071fb273.png)


# アプリの説明

## 目的

豪雨、洪水、高潮、津波など、水にまつわる自然災害に対して、各自治体がハザードマップを作成して公開しています。
2018年7月の岡山県倉敷市真備町の豪雨災害では、洪水ハザードマップの想定浸水域と実際の浸水域がほぼ同等であったことは記憶に新しいです。

他方で、2018年9月の北海道胆振東部地震では多数の土砂崩れによって地形が劇的に変化する様も見せつけられました。

地形や降雨条件が変化するような条件下で、初期の浸水条件から収束に至るまでを動的に計算して可視化するアプリを提供することで、
他所で起きた豪雨を自分の身近な場所で再現して追体験したり、
また、地形に対して加工を行うことでさまざまなリスクを仮想体験する、といったように災害への理解の手助けにならないかと考えています。

## 機能

現在のデモンストレーションでは「地理院標高タイルPNGで表現される地形」「表示中の地図内に均一に水を配置」を初期条件として、
時間とともに水の移動を計算するまでが実装されています。

1. 表示している地図内の DEM を取得・結合して地形レイヤを作成します
2. クリックをトリガーとして水量レイヤを作成します（現状は全体に同量の水を配置）
3. 時間とともに地形レイヤと水量レイヤからつぎの水量レイヤを計算します
4. 時間とともに水量レイヤが更新されることで、水量の変化がアニメーションとして表示されます

使用しているデータは [地理院標高タイルPNG](https://maps.gsi.go.jp/development/demtile.html) です。

## 今後の展開

### 地形データの取得先と干渉

- [Mapbox Terrain-RGB](https://docs.mapbox.com/help/troubleshooting/access-elevation-data/) のような全世界を対象とした PNG DEM を使用することで、世界中で同様のシミュレーションを実施。ハザードマップが整備されていないようなエリアでの災害リスクの把握。
- 各自治体が整備しつつあるといわれている、高解像度 DEM データの利用。地理院標高タイルではおおむね 5m メッシュまでの標高データを公開しているが、1m メッシュなどのさらに高解像度のデータであれば、より局地的で身近なシミュレーションが可能になるのではないか。
- 計算に使用する地形レイヤに対して仮想的に穴を掘ったり土を盛ったりすることで、地形そのものに干渉する


### 雨量データの取得先と干渉

- 今回は使用に至らなかったが、気象庁数値データを使用した時系列での降雨を地図上で再現する仕組み。
- 現在の降雨データを水量レイヤの初期条件にする（リアルタイムの浸水リスクモニタリング）
- 過去の豪雨時の降雨データを水量レイヤの初期条件にする（ワーストケースからの浸水リスク追体験）
- 常時水が排出されるようなポイントを任意に作成できるようにする（事故のシミュレーション）
- 常時水が吸収されるようなポイントを任意に作成できるようにする（事故対応のシミュレーション）

## その他

- ハザードマップの作成に用いられるような計算式をもとにブラウザ上でより精密な計算ができないかというトライ

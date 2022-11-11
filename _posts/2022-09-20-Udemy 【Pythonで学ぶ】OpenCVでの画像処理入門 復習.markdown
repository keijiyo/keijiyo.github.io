---
layout: post
title:  "Udemy 【Pythonで学ぶ】OpenCVでの画像処理入門 復習"
date:   2022-09-20 15:30:00 +0900
categories: jekyll update
---
<script src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

以前、受講したOpenCV入門の内容について復習メモ、参考リンクを残します。

画像処理分野はやってみると、視覚的に変化していくので面白いです。

＜講座情報＞  
<a href="https://www.udemy.com/course/pythonopencv/" target="_blank"> 【Pythonで学ぶ】OpenCVでの画像処理入門</a>

### 画像処理の基礎
- 画像処理共通の約束事：原点は左上
- OpenCVの約束事: (B, G, R)の順番で格納
- OpenCVの約束事2: numpyアレイには(y, x, color)の順番で格納

### 色空間・グレースケール
- RGBの他にHSVなど様々な色の表し方がある。
- HSV  
  - H(Hue): 色相 0-359
  - S(Saturation): 彩度 0-100%
  - V(Value): 明度 0-100%

※注意点

OpenCVでは以下の定義域。
- H(Hue): 色相 0-179（通常の定義域を1/2で表現。255までを利用するため。）
- S(Saturation): 彩度 0-255
- V(Value): 明度 0-255

### γ変換
$$\gamma$$変換とは画像の明るさの変換方法

$$
y = 255 \Bigl( \frac{x}{255} \Bigr) ^{1 / \gamma}
$$

- $$\gamma$$が1より小さい: 画像が暗くなる
- $$\gamma$$が1より大きい: 画像が明るくなる

### アファイン変換
アファイン変換とは回転・平行移動などの線形変換

### 畳み込み
フィルタで重みをつけながら周囲の情報を使って自分の画素を更新すること。

畳み込み後の画像
- ぼけている
- ノイズが消えている

画像の微分や平滑化など様々なフィルターがある（以下、参考リンク）
- <a href="https://www.mitani-visual.jp/mivlog/imageprocessing/filter-summary.php" target="_blank">MiVLog 平滑化フィルタ</a>
- <a href="https://www.mitani-visual.jp/mivlog/imageprocessing/sobel001.php" target="_blank">MiVLog Sobelフィルタ</a>
- <a href="https://www.mitani-visual.jp/mivlog/imageprocessing/edgefilter001.php" target="_blank">MiVLog 1次微分フィルタ Prewitt(プレヴィット)フィルタ</a>

### 画像の微分
- 曲線の傾き

$$
傾き = \frac{f(x + dx) - f(x)}{dx}
$$

- 画像処理での微分

$$
傾き = 隣の画素値 - 自分の画素値
$$

### エッジの検出(Canny)
Cannyはノイズを上手く取り除き、エッジを検出する

Cannyのアルゴリズム
1. ガウシアンフィルタでぼかす
2. Sobelフィルターで微分する
3. 極大点を探す
4. 2段階の閾値処理でエッジを残す

（参考）

<a href="https://imagingsolution.net/imaging/canny-edge-detector/" target="_blank">イメージングソリューション Canny edge detectionの処理アルゴリズム</a>

### 直線・円の検出
Hough(ハフ)変換を使う。

直線検出の方法

- 直線の式  
$$
y = ax + b
$$

- $$\rho$$と$$\theta$$で表した直線  

$$
\rho: 原点から直線までの距離
$$

$$
\theta: 直線の法線と横軸の成す角度
$$

$$
\rho = x \cos\theta + y \sin\theta
$$

$$
y = - \frac{\cos\theta}{\sin\theta} + \frac{\rho}{\sin\theta}
$$

![Hough](https://keijiyo.github.io/assets/images/Hough.png)

全ての点の$$\rho$$と$$\theta$$を集め、一番多かったパラメータを直線とする。

（参考）

<a href="http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/py_tutorials/py_imgproc/py_houghlines/py_houghlines.html" target="_blank"> OpenCV-Pythonチュートリアル ハフ変換による直線検出</a>

### モルフォロジー演算
morphology: 一般に、形態、構造をいう。

収縮、膨張によりノイズを消すことなどができる。

（参考）

<a href="http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html" target="_blank"> OpenCV-Pythonチュートリアル モルフォロジー変換</a>

### 特徴抽出
- フラット: 固有値の大きいものがない
- エッジ: 特有の固有ベクトルの固有値だけ大きくなる
- コーナー: エッジが2つあるので2つの大きな固有ベクトルがある。

特徴抽出・記述器には様々な種類のものがある。

Harris, SIFT, SURF, ORB, KAZE, AKAZE

### オプティカルフロー

オプティカルフローとは特徴点を見つけて、追いかけていく画像処理

オプティカルフローの流れ
1. 特徴点を見つける
2. 特徴点とその周りは同じ方向を向くとして、流れを出す
3. 「1.」と「2.」を繰り返す

### MeanShift／Camshiftの理論
同画像中に写る物体を発見・追跡するためのアルゴリズム

MeanShift/CamShiftはピクセルの密度最大の場所を追いかける。

MeanShiftのアルゴリズム
1. ある場所から探索窓が出発する（ユーザーが指定）
2. 探索窓内の画素値の重心を計算する
3. 探索窓の中心を重心に移す
4. 「2.」「3.」を繰り返す

CamShiftは探索窓が可変になるところが異なる。

探索開始場所や窓の大きさのパラメーター次第で答えが変わる

（参考）

<a href="http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/py_tutorials/py_video/py_meanshift/py_meanshift.html" target="_blank"> OpenCV-Pythonチュートリアル Meanshift と Camshift</a>

### 背景差分
背景差分: 今のフレーム - 背景のフレーム = 背景と異なるところだけ抽出（動いている物体）

背景差分により、背景かそれ以外かを分離できる

### パーティクルフィルター
パーティクルフィルターとは乱数を用いた状態推定法

画像処理分野では物体追跡に利用

1.パーティクルをばらまく  
　乱数を使って、nこのパーティクルをばらまく

2.パーティクルの尤度を計算

$$
粒子iの尤度 = e^{\frac{(f(t)-y(i))^2}{2\sigma^2}}
$$

$$
f: 真の曲線
\\
y: パーティクル
$$

3.リサンプリング（尤度に応じて選ぶ）  
　乱数でnこの粒子を選びなおす。尤度が高い粒子ほど選ばれやすい。

4.パーティクルの状態を更新  
　パーティクルの座標をランダムウォークさせる。

（参考）

<a href="https://algorithm.joho.info/programming/python/opencv-particle-filter-py/" target="_blank"> 【Python/OpenCV】パーティクルフィルタで物体追跡</a>

### 尤度

尤度とは起きた事象の"もっともらしさ"の事


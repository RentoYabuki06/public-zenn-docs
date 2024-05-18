---
title: ""
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# はじめに

42tokyoの入学試験Piscineを2024.3に受験し、ちゃんとアルゴリズムの勉強をしたいという思いで本格的にAtcoderでのハイランクを目指すことを志し、紹介してもらった「[競技プログラミングの鉄則](https://amzn.asia/d/esAZNG9)」を1周してみることに。

最初は軽い気持ちで取り組み始めたアルゴリズムの勉強でしたが、新たな問題が次々と解けていくことや実際にAtcoderのレートが上がる快感があり、気づいたらのめり込んでいました。

アルゴリズムの勉強って人類の叡智を感じますよね()

約1ヶ月かけて全ての演習問題を解いたので、ここに記録を残しておこうと思います。

全てのコードをAC確認しているつもりですが、不備などありましたらコメントや編集リクエストをいただけると幸いです。

コードは[Github](https://github.com/RentoYabuki06/kyopro-tessoku-practice)でも公開しています。

本を執筆された米田さん公認ではないので、解き方や解説などに不適切なところがしばしばあること、ご了承ください。特に変数宣言がmain関数の中だったり外だったりします（見返して癖に気づきました）。

また、全ての問題で、以下の標準ライブラリのインクルードと名前空間の指定は省略しております。
```cpp
#include <bits/stdc++.h>
using namespace std; 
```

# 1章 アルゴリズムと計算量
## 1.0 アルゴリズムと計算量
そもそもアルゴリズムはなんぞや、計算量とはなんぞや、という基礎の部分から解説されています。

現状の理解だと、アルゴリズムは **「問題を解くための決まった手順」** であり、計算量は「」で、計算量を定義する目的は **「同じ環境で手順を進めた場合の処理の数の差」** を共通言語化したものであるという認識です。

家庭用PCの計算速度が毎秒10^9回程度らしく、これを超えるものはTLE（時間超過）となり正解にはならないそうです。（AtCoderのみなのか、世界共通なのかは不明）

## 1.1 導入問題
### A01 - The First Problem
一辺がNの長さの長方形の面積を求める問題。N*Nを出力するだけなので小学生でもできることですが、プログラムにすると難しい。入力の受け付け方、出力の仕方などを調べながら進みます。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_a

```cpp
int main()
{
	int N;
	cin >> N;
	cout << N * N << endl;
}
```

### B01 - A+B Problem
A+Bを出力するプログラム。これも小学生でもできますね()

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bz

```cpp
int main()
{
	int A, B;
	cin >> A >> B;
	cout << A + B << endl;
}
```


## 1.2 全探索（1）
### A02 - Linear Search
N個の整数の中にXが含まれるか判定する問題。「全探索」というワードと条件分岐が出てくるので一気に競技プログラミングっぽくなってきました。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_b

```cpp
int main()
{
	int N, X;
	cin >> N >> X;
	for (int i = 0; i < N; i++)
	{
		int A;
		cin >> A;
		if (A == X)
		{
			cout << "Yes" << endl;
			return 0;
		}	
	}
	cout << "No" << endl;
}
```

### B02 - Divisor Check

A以上B以下の整数に、100の役数は存在するかどうか調べる問題。A問題とやることは一緒ですね。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ca

```cpp
int main()
{
	int A, B;
	cin >> A >> B;
	for (int x = A; x <= B; x++)
	{
		if (100 % x == 0)
		{
			cout << "Yes" << endl;
			return 0;
		}
	}
	cout << "No" << endl;
	return 0;
}
```

## 1.3 全探索（2）
### A03 - Two Cards
整数が書かれた赤と青のカードがそれぞれN枚あり、それらには整数が書かれています。
赤と青のカードを一枚ずつ取った時に合計がKになる組み合わせは存在するか。

最大でも組み合わせは N^2 通りで、Nは100以下の正の整数という制限があるので、十分全探索で間に合いますね。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_c

```cpp
int N, K;
int P[110];
int Q[110];

int main()
{
	int N, K;
	cin >> N >> K;
	for (int i = 0; i < N; i++) cin >> P[i];
	for (int i = 0; i < N; i++) cin >> Q[i];
	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < N; j++)
		{
			if (P[i] + Q[j] == K)
			{
				cout << "Yes" << endl;
				return 0;
			}
		}
	}
	cout << "No" << endl;
	return 0;
}
```

### B03 - Supermarket 1

N個の商品のうち、異なる3つの商品を選んで価格を1000円にできるか調べる問題。

最大でも組み合わせは `N * (N-1) * (N - 2)`(ほぼ N^3 )通りで、Nは100以下の正の整数という制限があるので、十分全探索で間に合いますね。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cb

```cpp
int N;
int A[110];

int main()
{
	cin >> N;
	for (int i = 0; i < N; i++) cin >> A[i];
	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < N; j++)
		{
			for (int k = 0; k < N; k++)
			{
				if (A[i] + A[j] + A[k] == 1000 && i != j && i != k && j != k)
				{
					cout << "Yes" << endl;
					return 0;
				}
			}
		}
	}
	cout << "No" << endl;
	return 0;
}
```

## 1.4 2進法
### A04 - Binary Representation 1

10進数のNを2進数に変換して出力せよという問題。

`(1 << x)`の部分はビットシフトを用いて、2のn乗の桁にビットが立っているかどうかを判定しています。この辺りはコンピュータが2進数(0と1のビット)で情報を保存していること
を知っていないと少しハードルが高いかもしれません。

基数変換が絡んでくると、急に数学っぽくなってきますね。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_d

```cpp
int main()
{
	int N;
	cin >> N;
	for (int x = 9; x >= 0; x--)
	{
		int wari = (1 << x);
		cout << (N / wari) % 2;
	}
	cout << endl;
	return 0;
}
```

### B04 - Binary Representation 2

今度は逆に2進数を10進数に変換する問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cc

```cpp
int main()
{
	int N;
	cin >> N;
	int deci = 0;
	for (int i = 0; N > 0; i++)
	{
		deci += (N % 2) * (1 << i);
		N /= 10;
	}
	cout << deci << endl;
}
```

## 1.5 チャレンジ問題
### A05 - Three Cards

3枚のカードにN以下の正の整数を書いて、合計をKにする方法が何通りあるか調べる問題。

直感的には3枚のカードを1からNまで回し、組み合わせの全パターンを調べるという全探索が思いつきますが、計算量が O(N^3) となり制限のNが3000以下では10^9を超えてきて実行時間に間に合いません。

「2枚のカードの値が決まれば、あとの1枚の値が決まる」という性質に気がついて、3重ループを2重ループにして計算量を削減できるかというところが問題の肝でした。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_e

```cpp
int main()
{
	int N, K;
	cin >> N >> K;
	int count = 0;
	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < N; j++)
		{
			int tmp = K - (i + j + 2);
			if (tmp > 0 && tmp <= N) count++;
		}
	}
	cout << count << endl;
	return 0;
}
```


## コラム2 ビット演算とビット全探索

# 2章 累積和
## 2.0 累積和とは
毎回計算するより、前もって計算しておいた数字を組みあわることで計算量を大きく削減できることもある。

例えば、遊園地のl日からr日までの合計来場者数を求める問題が例に出されている。

l日からr日の合計来場者数1回求めるだけなら計算するだけでいいが、l-3日からr+1日は？とか、l+1日からr+12日は？など、いろんなパターンで知りたい時には毎回計算を行うよりも前もってn日までの累積の来場者（ **累積和** ）を計算しておくほうが効率的だ。


## 2.1 1次元の累積和（1）
### A06 - How Many Guests?
2.0で例として出てきた遊園地の来場者数を出力する問題。

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_ai

```cpp
int main()
{
	int N, Q;
	cin >> N >> Q;
	int A[1000009], L[1000009], R[1000009], cum[1000009];
	for (int i = 0; i < N; i++) cin >> A[i];
	for (int i = 0; i < Q; i++) cin >> L[i] >> R[i];
	cum[0] = A[0];
	for (int i = 1; i < N; i++) cum[i] = cum[i - 1] + A[i];
	for (int i = 0; i < Q; i++)
	{
		if (L[i] == 1)
		{
			cout << cum[R[i] - 1] << endl;
		}
		else
		{
			cout << cum[R[i] - 1] - cum[L[i] - 2] << endl;
		}
	}
	return 0;
}
```

### B06 - Lottery 

あたり or ハズレ のくじを N回引いた。

L回目からR回目はあたりとはずれのどちらが多いか？という問題。

あたりとハズレの回数それぞれの累積和を計算しておく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ce

```cpp
int	N, A[100009], Q, L[100009], R[100009], hit[100009], miss[100009];

int main()
{
	cin >> N;
	for (int i = 0; i < N; i++) cin >> A[i];
	cin >> Q;
	for (int i = 0; i < Q; i++) cin >> L[i] >> R[i];
	if (A[0] == 0)
	{
		hit[0] = 0;
		miss[0] = 1;
	}
	else
	{
		hit[0] = 1;
		miss[0] = 0;
	}
	for (int i = 1; i < N; i++)
	{
		if (A[i] == 0)
		{
			hit[i] = hit[i - 1];
			miss[i] = miss[i - 1] + 1;
		}
		else
		{
			hit[i] = hit[i - 1] + 1;
			miss[i] = miss[i - 1];
		}
	}
	for (int i = 0; i < Q; i++)
	{
		if (L[i] == 1)
		{
			if (hit[R[i] - 1] > miss[R[i] - 1])
			{
				cout << "win" << endl;
			}
			else if (hit[R[i] - 1] < miss[R[i] - 1])
			{
				cout << "lose" << endl;
			}
			else{
				cout << "draw" << endl;
			}
		}
		else
		{
			if (hit[R[i] - 1] - hit[L[i] - 2] > miss[R[i] - 1] - miss[L[i] - 2])
			{
				cout << "win" << endl;
			}
			else if (hit[R[i] - 1] - hit[L[i] - 2] < miss[R[i] - 1] - miss[L[i] - 2])
			{
				cout << "lose" << endl;
			}
			else{
				cout << "draw" << endl;
			}
		}
	}
}
```

## 2.2 1次元の累積和（2）
### A07 - Event Attendance

D日間行われるイベントに、N人が出席する。参加者それぞれがLi日からRi日まで参加する時、各日の出席者を出席せよ。

出席者の前日比を記録しておいて累積和を取ると効率的に求められる。

後から考えたら当然だが、初めてみた時には画期的だなと思った。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_g

```cpp
int D, N, L[100009], R[100009], change[100009], total[100009];

int main()
{
	cin >> D >> N;
	for (int i = 0; i < N; i++) cin >> L[i] >> R[i];
	for (int i = 0; i < N; i++)
	{
		change[L[i] - 1]++;
		change[R[i]]--;
	}
	total[0] = change[0];
	for (int i = 1; i < D; i++)
	{
		total[i] = total[i - 1] + change[i];
	}
	for (int i = 0; i < D; i++) cout << total[i] << endl;
}
```

### B07 - Convenience Store 2

T時間開店するコンビニに、時刻LiからRiまで出勤する従業員がN人いる。時刻t+0.5には何人の従業者が働いているか求めよ。

A問題の前日比を前の時刻からの従業員の増減比に置き換えて記録する。

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_al

```cpp
int T, N, L[500009], R[500009], change[500009], total[500009];

int main()
{
	cin >> T >> N;
	for(int i = 0; i < N; i++) cin >> L[i] >> R[i];
	for(int i = 0; i < N; i++)
	{
		change[L[i]]++;
		change[R[i]]--;
	}
	total[0] = change[0];
	cout << total[0] << endl;
	for(int i = 1; i < T; i++)
	{
		total[i] = total[i - 1] + change[i];
		cout << total[i] << endl;
	}
	return 0;
}
```

## 2.3 2次元の累積和（1）
### A08 - Two Dimensional Sum

H*W のマスにそれぞれ整数が書かれている。二つマスが与えられ、それらのマスを左上と右下に配置した長方形の合計の数を出力せよ、という問題。

累積和を2次元に拡張する。横と縦にそれぞれ累積和を取るが、増減の重複が起きることに注意。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_h

```cpp
int H, W, Q;
int X[1509][1509], cum[1509][1509];
int A[1000009], B[1000009], C[1000009], D[1000009];

int main()
{
	cin >> H >> W;
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++)
		{
			cin >> X[i][j];
			cum[i][j] = 0;
		}
	}
	cin >> Q;
	for (int i = 1; i <= Q; i++) cin >> A[i] >> B[i] >> C[i] >> D[i];
	//横方向の累積和
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++)
		{
			cum[i][j] = cum[i][j - 1] + X[i][j];
		}
	}
	//縦方向の累積和
	for (int j = 1; j <= W; j++)
	{
		for (int i = 1; i <= H; i++)
		{
			cum[i][j] += cum[i - 1][j];
		}
	}
	for (int i = 1; i <= Q; i++)
	{
		int sum = 0;
		sum = cum[C[i]][D[i]] - cum[C[i]][B[i] - 1] - cum[A[i] - 1][D[i]] + cum[A[i] - 1][B[i] - 1];
		cout << sum << endl;
	}
	return 0;
}
```

### B08 - Counting Points

二次元平面上にN個の点がある。x座標がa以上c以下、y座標がb以上d以下の点はいくつあるか？という問題。

各座標にいくつ点があるかを二次元配列で記録する。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cg

```cpp
int N, Q;
int X[1000009], Y[1000009], a[1000009], b[1000009], c[1000009], d[1000009];
int dots[1509][1509];

int main()
{
	cin >> N;
	for (int i = 0; i <= 1505; i++)
	{
		for (int j = 1; j <= 1505; j++) dots[i][j] = 0;
	}
	for (int i = 1; i <= N; i++)
	{
		cin >> X[i] >> Y[i];
		dots[X[i]][Y[i]]++;
	}
	cin >> Q;
	for (int i = 1; i <= Q; i++) cin >> a[i] >> b[i] >> c[i] >> d[i];
	for (int i = 1; i <= 1505; i++)
	{
		for (int j = 1; j <= 1505; j++)
		{
			dots[i][j] += dots[i][j - 1];
		}
	}
	for (int j = 1; j <= 1505; j++)
	{
		for (int i = 1; i <= 1505; i++)
		{
			dots[i][j] += dots[i - 1][j];
		}
	}
	for (int i = 1; i <= Q; i++)
	{
		int sum = dots[c[i]][d[i]] - dots[c[i]][b[i] - 1] - dots[a[i] - 1][d[i]] + dots[a[i] - 1][b[i] - 1];
		cout << sum << endl;
	} 
	return 0;
}
```

## 2.4 2次元の累積和（2）
### A09 - Winter in ALGO Kingdom

H*Wのマスに雪が降る。二つマスがN回与えられ、それらのマスを左上と右下に配置した長方形の領域の積雪が1cmずつ増える。最終的に各マスの積雪を出力せよ、という問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_i

```cpp
int H, W, N;
int A[1000009], B[1000009], C[1000009], D[1000009];
int dots[1509][1509];

int main()
{
	cin >> H >> W >> N;
	for (int i = 0; i <= 1505; i++)
	{
		for (int j = 0; j <= 1505; j++) dots[i][j] = 0;
	}
	for (int i = 1; i <= N; i++)
	{
		cin >> A[i] >> B[i] >> C[i] >> D[i];
		dots[A[i]][B[i]]++;
		dots[A[i]][D[i] + 1]--;
		dots[C[i] + 1][B[i]]--;
		dots[C[i] + 1][D[i] + 1]++;
	}
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++) dots[i][j] += dots[i][j - 1];
	}
	for (int j = 1; j <= W; j++)
	{
		for (int i = 1; i <= H; i++) dots[i][j] += dots[i - 1][j];
	}
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++)
		{
			if (j > 1) cout << " ";
			cout << dots[i][j];
		}
		cout << endl;
	}
	return 0;
}
```

### B09 - Papers

2次元平面上にN枚の紙がある。二つマスがN回与えられ、N枚の紙はそれらのマスを左上と右下に配置した長方形の位置ある。1枚以上の紙が置かれている部分の面積を求めよ、という問題。

各座標に何枚の紙が重なっているかを二次元累積和で求めて、最終的に0でない部分の面積を足して出力する。


https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ch

```cpp
int N;
int A[1000009], B[1000009], C[1000009], D[1000009];
int dots[1509][1509];

int main()
{
	cin >> N;
	for (int i = 0; i <= 1505; i++)
	{
		for (int j = 0; j <= 1505; j++) dots[i][j] = 0;
	}
	for (int i = 1; i <= N; i++)
	{
		cin >> A[i] >> B[i] >> C[i] >> D[i];
		dots[A[i]][B[i]]++;
		dots[A[i]][D[i]]--;
		dots[C[i]][B[i]]--;
		dots[C[i]][D[i]]++;
	}
	for (int i = 0; i <= 1505; i++)
	{
		for (int j = 1; j <= 1505; j++) dots[i][j] += dots[i][j - 1];
	}
	for (int j = 0; j <= 1505; j++)
	{
		for (int i = 1; i <= 1505; i++) dots[i][j] += dots[i - 1][j];
	}
	int count = 0;
	for (int i = 0; i <= 1505; i++)
	{
		for (int j = 0; j <= 1505; j++)
		{
			if (dots[i][j] > 0) count++;
		}
	}
	cout << count << endl;
	return 0;
}
```

## 2.5 チャレンジ問題
### A10 - Resort Hotel

N個の部屋があり、i号室はAi人入れる。D日間工事が行われ、d日目にはLd号室からRd号室までの使用ができない。d日目に使用できる最も人数の入れる部屋は何人部屋であるか求めよ、という問題。

中間にある部屋が使用できなくなるので、右から一番大きな部屋と左から調べた時に一番大きな部屋を記録した配列を用意しておくと使用できなくなる部屋の左側と右側の一番大きな部屋同士を比較するだけになる。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_j

```cpp
int N, A[100009], D, L[100009], R[100009], left_maxroom[100009], right_maxroom[100009];

int main()
{
	cin >> N;
	for (int i = 0; i < N; i++) cin >> A[i];
	cin >> D;
	for (int i = 0; i < D; i++) cin >> L[i] >> R[i];
	left_maxroom[0] = A[0];
	right_maxroom[N - 1] = A[N - 1];
	for (int i = 1; i < N; i++)
	{
		if (A[i] > left_maxroom[i - 1])
		{
			left_maxroom[i] = A[i];
		}
		else
		{
			left_maxroom[i] = left_maxroom[i - 1];
		}
		if (A[N - 1 - i] > right_maxroom[N - i])
		{
			right_maxroom[N - 1 - i] = A[N - 1 - i];
		}
		else
		{
			right_maxroom[N - 1 - i] = right_maxroom[N - i];
		}
	}
	for (int i = 0; i < D; i++)
	{
		if (right_maxroom[R[i]] > left_maxroom[L[i] - 2])
		{
			cout << right_maxroom[R[i]] << endl;
		}
		else
		{
			cout << left_maxroom[L[i] - 2] << endl;
		}
	}
	return 0;
}
```

## コラム3 アルゴリズムで使う数学

# 3章 二分探索
## 3.0 二分探索とは
相手の年齢が気になったとする。1歳ですか？2歳ですか？...と1歳ずつ1歳ずつ1歳ずつインクリメントして聞くのもいいが、もっと早く解に辿り着ける。

人間が0歳から100歳までの間の年齢をとるとすると、

 - 50歳以下ですか？→ "yes"
 - 25歳以下ですか？→ "yes"
 - 12歳以下ですか？→ "no"
 - 19歳以下ですか？→ "no"
 - 22歳以下ですか？→ "no"
 - 23歳以下ですか？→ "yes"

とありうる値の半分で区切り続ける、という二分探索法が計算量 O(logN)で非常に効率的ということが知られている。

## 3.1 配列の二分探索
### A11 - Binary Search 1

小さい順に並べられた配列の何番目に該当要素があるかを求める問題。

ft_search()の部分で二分探索法を実装している。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_k

```cpp
int N, X;
int A[100009];

int	ft_search(int x)
{
	int L = 1;
	int R = N;
	while (L <= R)
	{
		int M = (L + R) / 2;
		if (x < A[M]) R = M - 1;
		if (x == A[M]) return M;
		if (x > A[M]) L = M + 1;
	}
	return -1;
}

int main()
{
	cin >> N >> X;
	for (int i = 1; i <= N; i++) cin >> A[i];
	int ans = ft_search(X);
	cout << ans << endl;
	return 0;
}
```

### B11 - Binary Search 2

与えられた配列Aの中にXより小さな数は幾つ存在するか？という質問にQ回答える問題。

二分探索は配列がソートされている状態でないと行えないので、sortしてから二分探索（lower_bound関数）で配列の中でXが小さい方から何番目か調べている。

lower_bound()はイテレータを返すので、 `lower_bound(A, A + N, X) - A` とすることで配列の前から何番目に位置するかのインデックスとして得ることができる。（indexは0から始まることに注意）

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cj

```cpp
int N, A[100009], Q, X;

int	main()
{
	cin >> N;
	for (int i = 0; i < N; i++) cin >> A[i];
	cin >> Q;
	sort(A, A + N);
	for (int i = 0; i < Q; i++)
	{
		cin >> X;
		int pos = lower_bound(A, A + N, X) - A;
		cout << pos << endl;
	}
	return 0;
}
```

## 3.2 答えで二分探索
### A12 - Printer
Nマイのプリンターがあり、それぞれAi秒間隔で1枚チラシを印刷する。すべてのプリンターのスイッチを同時に入れた時、K枚目のチラシが印刷されるのは何秒後か。

`ft_check(mid)`で答えがmid秒より前か後か判定するプログラムを組み込んだ二分探索で実装できる。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_l

```cpp
int N, K, A[100009];

bool	ft_check(long long x)
{
	long long sum = 0;
	for (int i = 1; i <= N; i++) sum += x / A[i];
	if (sum >= K) return true;
	return false;
}

int main()
{
	cin >> N >> K;
	for (int i = 1; i <= N; i++) cin >> A[i];
	long long L = 1;
	long long R = 1000000000;
	while (L <= R)
	{
		long long mid = (R + L) / 2;
		if (ft_check(mid)) R = mid - 1;
		else L = mid + 1;
	}
	cout << L << endl;
	return 0;
}
```

### B12 - Equation

`x^3 + x = N` を満たす正の実数xを出力せよ。誤差が1e3以下であればいい。

`x^3 + x`は単調増加の関数なため、答えがx以上か？で二分探索が可能。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ck

```cpp
int N;

int main()
{
	cin >> N;
	float L = 1;
	float R = 100;
	for (int i = 0; i < 10000; i++)
	{
		float mid = (L + R) / 2;
		if ((mid * mid * mid) + mid > N) R = mid;
		else L = mid;
	}
	cout << L << endl;
	return 0;
}
```

## 3.3 しゃくとり法
ABC353のC問題に出てきて解けず悔しい思いをしたのが記憶に新しい。
しゃくとり法は配列やリストの中で連続した部分を効率よく探す方法で、
二分探索のO(logN)よりも早いO(N)で実装できる。

https://atcoder.jp/contests/abc353/editorial/9957

### A13 - Close Pairs

N個の整数から二つ選ぶ。差がK以下となる選び方は何通りあるか。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_m

```cpp
int N, K, A[100009], R[100009];

int main()
{
	cin >> N >> K;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= N - 1; i++)
	{
		if (i == 1)  R[i] = 1;
		else R[i] = R[i - 1];
		while(R[i] < N && A[R[i] + 1] - A[i] <= K)
		{
			R[i] += 1;
		}
	}
	long long ans = 0;
	for (int i = 1; i <= N - 1; i++)
	{
		ans += (R[i] - i);
	}
	cout << ans << endl;
	return 0;
}
```

### B13 - Supermarket 2

N個の商品に1からNまでの番号がついている。隣り合った番号の商品を合計金額K円いないで購入する方法は何通りあるか。

ただ求めるだけであれば N*(N+1)/2 の全通りを調べる方法もありそうだが、計算量 O(N)で求めよという制約がありできない。

まず累積和を計算しておき、しゃくとり法を用いることで実装できる。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cl

```cpp
int N, K, A[100009], cum[100009], R[100009];
long long ans = 0;

int main()
{
	cin >> N >> K;
	for (int i = 1; i <= N; i++) cin >> A[i];
	cum[1] = A[1];
	for (int i = 2; i <= N; i++) cum[i] = cum[i - 1] + A[i];
	for (int i = 1; i <= N - 1; i++)
	{
		if (i == 1) R[i] = 0;
		else R[i] = R[i - 1];
		while (R[i] < N && cum[R[i] + 1] - cum[i - 1] <= K) R[i]++;
		ans += R[i] - i + 1;
	}
	if (A[N] <= K) ans++;
	cout << ans << endl;
	return 0;
}
```

## 3.4 半分前列挙
### A14 - Four Boxes

4つのボックスにそれぞれN枚の整数が書いてあるカードが入っている。それぞれの箱から1枚ずつカードを取り出し、合計がKとなる可能性があるかを判定せよ。

素直に4枚のカードの選び方を全探索すると、N^4通りとなり間に合いません。

そこで、2箱ずつに分けて、それぞれの箱から取り出した数の合計の全てのパターンを記録しておきます。そして、それらの全てのパターンの箱二つから一枚ずつ取り出して足し合わせた時に合計がKになるかどうかを調べていきます。計算量は前半がO(N^2)、後半では二分探索法をN^2回行うためO(N^2 * logN) となります。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_n

```cpp
int  N, K, A[1009], B[1009], C[1009], D[1009], AB[1000009], CD[1000009];

int main()
{
	cin >> N >> K;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= N; i++) cin >> B[i];
	for (int i = 1; i <= N; i++) cin >> C[i];
	for (int i = 1; i <= N; i++) cin >> D[i];
	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= N; j++)
		{
			AB[(i - 1) * N + j] = A[i] + B[j];
			CD[(i - 1) * N + j] = C[i] + D[j];
		}	
	}
	sort(CD + 1, CD + N * N + 1);
	for (int i = 1; i <= N * N; i++)
	{
		int pos = lower_bound(CD + 1, CD + N * N + 1, K - AB[i]) - CD;
		if (pos <= N * N && CD[pos] == K - AB[i])
		{
			cout << "Yes" << endl;
			return 0;
		}
	}
	cout << "No" << endl;
	return 0;
}
```

### B14 - Another Subset Sum

N枚のカードの中から合計がKになるような選び方は存在するか、という問題。

それぞれのカードを選ぶか選ばないかの2通りがN枚あるので、maxの計算量は2^Nとなる。

A問題を参考にN/2枚ずつに分けて可能性のある合計の数を記録して、最終的にKとなる組み合わせが存在するか、という調べ方だと実行制限時間に間に合う。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cm

```cpp
int N, K, A[33], first[1000009], second[1000009];

int main()
{
	cin >> N >> K;
	for (int i = 0; i < N; i++) cin >> A[i];
	//前半の組み合わせ全列挙
	for (int i = 0; i < (1 << (N / 2)); i++)
	{
		int sum = 0;
		for (int j = 0; j <= N / 2; j++)
		{
			if ((i >> j) & 1) sum += A[j];
		}
		first[i] = sum;
	}
	//後半の組み合わせ全列挙
	for (int i = 0; i < (1 << N - (N / 2)); i++)
	{
		int sum = 0;
		for (int j = 0; j <= N - (N / 2); j++)
		{
			if ((i >> j) & 1) sum += A[j + N / 2];
		}
		second[i] = sum;
	}
	sort(first, first + (1 << N / 2));
	for (int i = 1; i <= (1 << N - (N / 2)); i++)
	{
		int L = 0;
		int R =(1 << N / 2) - 1;
		while (L <= R)
		{
			int mid = (L + R) / 2;
			if (second[i] + first[mid] < K) L = mid + 1;
			else if (second[i] + first[mid] == K)
			{
				cout << "Yes" << endl;
				return 0;
			}
			else R = mid - 1;
		}
	}
	cout << "No" << endl;
	return 0;
}
```

## 3.5 チャレンジ問題
### A15 - Compression

言葉で説明がしづらいが、横の数との大小関係が入れ替わらない制限の元で、配列のそれぞれの要素をなるべく小さな整数に圧縮しなさい、という問題。座標圧縮と言われるような操作らしい。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_o

```cpp
int N, A[100009], B[100009];

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];
	vector<int> T;
	for (int i = 1; i <= N; i++) T.push_back(A[i]);
	sort(T.begin(), T.end());
	T.erase(unique(T.begin(), T.end()), T.end());

	for (int i = 1; i <= N; i++)
	{
		B[i] = lower_bound(T.begin(), T.end(), A[i]) - T.begin();
		B[i] += 1;
	}
	for (int i = 1; i <= N; i++)
	{
		if (i > 1) cout << " ";
		cout << B[i];
	}
	cout << endl;
	return 0;
}
```

# 4章 動的計画法
ここまでの章は3,4時間ほどでクリアしてこれたが、この章は重かった。正直Piscineで動的計画法の基礎には触れていたので、サクサクこなせると考えていたが甘かった。二日かけて何とか軽い理解ができたイメージ。

## 4.0 動的計画法とは
動的計画法は、より小さな問題の結果を利用して問題を解いていく方法。
計算の重複が起こらないため、全探索などよりも非常に効率的に解けるという理解をしている。

## 4.1 動的計画法の基本
### A16 - Dungeon 1

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_p

ダンジョンの最短移動時間を求める問題。n番目の部屋への最短移動時間は(n-1)番目と(n-2)番目の部屋への最短移動時間を利用して解いていく。

```cpp
int N, A[100009], B[100009], min_time[100009];

int main()
{
	cin >> N;
	for (int i = 2; i <= N; i++) cin >> A[i];
	for (int i = 3; i <= N; i++) cin >> B[i];
	min_time[1] = 0;
	min_time[2] = A[2];
	for (int i = 3; i <= N; i++)
	{
		min_time[i] = min(min_time[i - 1] + A[i], min_time[i - 2] + B[i]);
	}
	cout << min_time[N] << endl;
}
```

### B16 - Frog 1

移動コストの最小値を求める問題。上のダンジョンと同様に、n番目の足場への最小コストは(n-1)番目と(n-2)番目の足場への最小コストを利用して解いていく。

https://atcoder.jp/contests/tessoku-book/tasks/dp_a

```cpp
int N, h[100009], min_cost[100009];

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> h[i];
	min_cost[1] = 0;
	min_cost[2] = abs(h[2] - h[1]);
	for (int i = 3; i <= N; i++)
	{
		min_cost[i] = min(min_cost[i - 1] + abs(h[i] - h[i - 1]), min_cost[i - 2] + abs(h[i] - h[i - 2]));
 	}
	cout << min_cost[N] << endl;
	return 0;
}
```

## 4.2 動的計画法の復元
### A17 - Dungeon 2

先のダンジョン問題（A16）で求めた、最短移動時間を実現できる経路を出力せよ、という問題。

最短経路を先の方法で求め。その最短経路をゴールから遡りながら最短経路を記録していく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_q

```cpp
int N, A[100009], B[100009], min_time[100009];
vector<int> path;

int main()
{
	cin >> N;
	for (int i = 2; i <= N; i++) cin >> A[i];
	for (int i = 3; i <= N; i++) cin >> B[i];
	min_time[1] = 0;
	min_time[2] = A[2];
	for (int i = 3; i <= N; i++)
	{
		min_time[i] = min(min_time[i - 1] + A[i], min_time[i - 2] + B[i]);
	}
	int place  = N;
	while (1)
	{
		path.push_back(place);
		if (place == 1)
			break;
		if (min_time[place] == min_time[place - 1] + A[place])
			place -= 1;
		else
			place -= 2;
	}
	reverse(path.begin(), path.end());
	cout << path.size() << endl;
	for (int i = 0; i < path.size(); i++)
	{
		if (i > 0) cout << " ";
		cout << path[i];
	}
	cout << endl;
	return 0;
}
```

### B17 - Frog 1 with Restoration

B16で解いたカエルの移動コストを最小化する経路を出力してください、という問題。
こちらも先ほどと同様、最小コストを動的計画法で求め。その最小コストを実現するルートをゴールから遡りながら記録していく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cp

```cpp
int N, h[100009], min_cost[100009];
vector<int> path;

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> h[i];
	min_cost[1] = 0;
	min_cost[2] = abs(h[2] - h[1]);
	for (int i = 3; i <= N; i++)
	{
		min_cost[i] = min(min_cost[i - 1] + abs(h[i] - h[i - 1]), min_cost[i - 2] + abs(h[i] - h[i - 2]));
 	}
	int place = N;
	while(1)
	{
		path.push_back(place);
		if (place == 1)
			break;
		if (min_cost[place] == min_cost[place - 1] + abs(h[place] - h[place - 1]))
			place -= 1;
		else
			place -= 2;
	}
	reverse(path.begin(), path.end());
	cout << path.size() << endl;
	for(int i = 0; i < path.size(); i++)
	{
		if (i > 0) cout << " ";
		cout << path[i];
	}
	cout << endl;
	return 0;
}
```

## 4.3 二次元のDP（1）:部分和問題
コラム2においてビット全探索で解いた部分和問題をさらに効率的に動的計画法で解いていく。

### A18 - Subset Sum

カードを任意の枚数選択した時に、合計が〇〇に一致する可能性はあるか？という問題。
配列を設定し、カラム数が指定の合計値、row数がカードの選択枚数という二次元配列を用意して解いていく。これを最初に考えた人はすごいな。アルゴリズムを勉強していると人類の叡智を感じることができて好き。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_r

```cpp
int N, S, A[69];
bool dp[69][10009];

int main()
{
	cin >> N >> S;
	for (int i = 1; i <= N; i++) cin >> A[i];
	dp[0][0] = true;
	for (int i = 1; i <= S; i++) dp[0][i] = false;

	for (int i = 1; i <= N; i++)
	{
		for (int j = 0; j <= S; j++)
		{
			if (j >= A[i])
			{
				if (dp[i - 1][j] == true || dp[i - 1][j - A[i]] == true)
					dp[i][j] = true;
				else
					dp[i][j] = false;
			}
			else
			{
				if (dp[i - 1][j] == true)
					dp[i][j] = true;
				else
					dp[i][j] = false;
			}
		}
	}
	if (dp[N][S] == true) cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```

### B18 - Subset Sum with Restoration

これは先の問題の「カードを任意の枚数選択した時に、合計が〇〇に一致する可能性はあるか？」を調べた上で、もし可能性があるのであればそのカードの組み合わせを出力してください、という問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cq

```cpp
int N, S, A[69];
bool dp[69][10009];
vector<int> Answer;

int main()
{
	cin >> N >> S;
	for (int i = 1; i <= N; i++) cin >> A[i];
	dp[0][0] = true;
	for (int i = 1; i <= S; i++) dp[0][i] = false;

	for (int i = 1; i <= N; i++)
	{
		for (int j = 0; j <= S; j++)
		{
			if (j >= A[i])
			{
				if (dp[i - 1][j] == true || dp[i - 1][j - A[i]] == true)
					dp[i][j] = true;
				else
					dp[i][j] = false;
			}
			else
			{
				if (dp[i - 1][j] == true)
					dp[i][j] = true;
				else
					dp[i][j] = false;
			}
		}
	}
	if (dp[N][S] == true)
	{
		int place = N;
		int num = S;
		while (1)
		{
			if (place == 0)
				break;
			if (dp[place - 1][num] == true)
			{
				place -= 1;
			}
			else
			{
				Answer.push_back(place);
				num -= A[place];
				place -= 1;
			}
		}
		reverse(Answer.begin(), Answer.end());
		cout << Answer.size() << endl;
		for (int i = 0; i < Answer.size(); i++)
		{
			if (i > 0) cout << " ";
			cout << Answer[i];
		}
		cout << endl;
	}
	else cout << "-1" << endl;
	return 0;
}
```

## 4.4 二次元のDP（2）:ナップザック問題
### A19 - Knapsack 1

任意の重さ、任意の価値のN個の品物があり、それぞれの重さと価値が指定されるので、価値が最大値になるように選択した時に合計価値はいくらになりますか。ただし、ナップザックには重さWまでしか入りません、という問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_s

```cpp
long long N, W, w[109], v[109];
long long dp[109][100009];

int main()
{
	cin >> N >> W;
	for (int i = 1; i <= N; i++) cin >> w[i] >> v[i];
	for (int i = 0; i <= N; i++)
	{
		for (int i = 0; i <= W; i++) dp[N][W] = -1000000000000LL;
	}
	dp[0][0] = 0;
	for (int i = 1; i <= N; i++)
	{
		for (int j = 0; j <= W; j++)
		{
			if (j < w[i]) dp[i][j] = dp[i - 1][j];
			else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]]  + v[i]);
		}
	}
	long long ans = 0;
	for (int i = 0; i <= W; i++)
	{
		ans = max(ans, dp[N][i]);
	}
	cout << ans << endl;
	return 0;
}
```

### B19 - Knapsack 2

A19のナップザック問題のそれぞれの品物の価値が1000未満になる代わりに、Wが10の9乗までの範囲になるので、2秒以内に終わるプログラムを考えてください。という問題。
配列を一次元にすることと、横軸を重さではなく価値の総量に変更し、配列に入る値を「その価値を得るための最小の重さ」に変更することで正解になった。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cr

```cpp
int N, weight[109], value[109];
long long W;

int main()
{
	cin >> N >> W;
	int total_value = 0;
	for (int i = 1; i <= N; i++)
	{
		cin >> weight[i] >> value[i];
		total_value += value[i];
	}
	vector<long long> dp(total_value + 1, 1000000000000000000);
	dp[0] = 0;
	for (int i = 1; i <= N; i++)
	{
		for (int j = total_value - value[i]; j >= 0; j--)
		{
			dp[j + value[i]] = min(dp[j + value[i]], dp[j] + weight[i]); 
		}
	}
	int ans = 0;
	for (int i = 0; i <= total_value; i++)
	{
		if (dp[i] <= W)
			ans = max(ans, i);
	}
	cout << ans << endl;
	return 0;
}
```

## 4.5 二次元のDP（3）:最長共通部分列問題
### A20 - LCS

二つ文字列を与えられるので、最長の共通部分列を求める問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_t

```cpp
int S_len, T_len, dp[2009][2009];
string S, T;

int main()
{
	cin >> S >> T;
	S_len = S.size();
	T_len = T.size();

	dp[0][0] = 0;
	for (int i = 0; i <= S_len; i++)
	{
		for (int j = 0; j <= T_len; j++)
		{
			if (i > 0 && j > 0 && S[i - 1] == T[j - 1])
			{
				dp[i][j] = max({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1] + 1});
			}
			else if (i > 0 && j > 0)
			{
				dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
			}
			else if (i > 0)
			{
				dp[i][j] = dp[i - 1][j];
			}
			else if (j > 0)
			{
				dp[i][j] = dp[i][j - 1];
			}
		}
	}
	cout << dp[S_len][T_len] << endl;
	return 0;
}
```

### B20 - Edit Distance

[編集距離（レーベンシュタイン距離）の求め方](https://mathwords.net/hensyukyori)という記事を参考にしながら理解した。直感的にはわかりずらい。
最初以下のコードで提出したときはNGだった。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cs

```cpp
int S_len, T_len, dp[2009][2009];
string S, T;

int main()
{
    cin >> S >> T;
    S_len = S.size();
    T_len = T.size();
    dp[0][0] = 0;
    for (int i = 0; i <= S_len; i++)
    {
        for (int j = 0; j <= T_len; j++)
        {
            if (i > 0 && j > 0 && S[i - 1] == T[j - 1])
            {
                dp[i][j] = min(dp[i - 1][j], min(dp[i][j - 1], dp[i - 1][j - 1]));
            }
            else if (i > 0 && j > 0)
            {
                dp[i][j] = min(dp[i - 1][j], min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
            }
            else if (i > 0)
            {
                dp[i][j] = dp[i - 1][j] + 1;
            }
            else if (j > 0)
            {
                dp[i][j] = dp[i][j - 1] + 1;
            }
        }
    }
    cout << dp[S_len][T_len] << endl;
    return 0;
}

```

以下のプログラムに修正したらACだったのだが、正直上のコードの悪かった点がわかっていない、、

```cpp
int S_len, T_len, dp[2009][2009];
string S, T;

int main()
{
    cin >> S >> T;
    S_len = S.size();
    T_len = T.size();
    
    // DPテーブルの初期化
    for (int i = 0; i <= S_len; i++)
    {
        for (int j = 0; j <= T_len; j++)
        {
            if (i == 0)
            {
                dp[i][j] = j;
            }
            else if (j == 0)
            {
                dp[i][j] = i;
            }
            else if (S[i - 1] == T[j - 1])
            {
                dp[i][j] = dp[i - 1][j - 1];
            }
            else
            {
                dp[i][j] = min(min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
            }
        }
    }
    
    cout << dp[S_len][T_len] << endl;
    return 0;
}
```

## 4.6 二次元のDP（4）:区間DP
### A21 - Block Game

横一列に並んでいるブロックを得点が最大化するように取り除いた時の得点を出力するぷorグラム。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_u

```cpp
int N, P[2009], A[2009], dp[2009][2009];

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> P[i] >> A[i];
	
	//dpスタート位置は一番端から端まで生き残っているところ
	dp[1][N] = 0;
	for (int len = N - 2; len >= 0; len--)
	{
		for (int l = 1; l <= N - len; l++)
		{
			int r = l + len;
			int score_left = 0;
			int score_right = 0;
			if (P[l - 1] <= r && P[l - 1] >= l) score_left = A[l - 1];
			if (P[r + 1] <= r && P[r + 1] >= l) score_right = A[r + 1];
			if (l == 1)
			{
				dp[l][r] = dp[l][r + 1] + score_right;
			}
			else if (r == N)
			{
				dp[l][r] = dp[l - 1][r] + score_left;
			}
			else
			{
				dp[l][r] = max(dp[l][r + 1] + score_right, dp[l - 1][r] + score_left);
			}
		}
	}
	int ans = 0;
	for (int i = 1; i <= N; i++) ans = max(ans, dp[i][i]);
	cout << ans << endl;
	return 0;
}
```

### B21 - Longest Subpalindrome

与えられた文字列の中で、任意の文字を取り除いてもよい時、最長可能な回文の文字数は何文字か。という問題。

正直難しくてchatGPTを頼ってしまった。基本方針は中心拡張法を用いている。端の文字が同じ文字だったら2文字文追加していくというシンプルな実装なのだが、実装となるとなぜか動かないプログラムが発生してしまう。

この問題は自分の手で再実装できるかを再確認しなければ。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ct

```cpp
int main()
{
    int N;
    string S;
    
    cin >> N >> S;

    // dpテーブルの定義と初期化
    vector<vector<int>> dp(N, vector<int>(N, 0));

    // 1文字からなる回文は必ず作れる
    for (int i = 0; i < N; i++) 
        dp[i][i] = 1;

    // DPテーブルの更新
    for (int len = 2; len <= N; len++) //文字数が2文字からmaxのN文字まで増やす
    {
        for (int l = 0; l + len - 1 <= N - 1; l++) //左の文字を一つずつずらしていく
        {
            int r = l + len - 1;
            // もし左右の端の文字が一致した場合
            if (S[r] == S[l])
            {
                if (len == 2)
                {
					//文字列が2文字の時は必ず2文字の回文
                    dp[l][r] = 2;
                }
                else 
                {
					//dp[l][r]の方がdp[l + 1][r - 1]よりもに文字多い→必ず先に計算している
                    dp[l][r] = dp[l + 1][r - 1] + 2;
                }
            }
            // 一致しなかった場合は、どちらかの文字を取り除く
            else
            {
                dp[l][r] = max(dp[l + 1][r], dp[l][r - 1]);
            }
        }
    }
    // 最大の回文長は一番文字が長い時
    cout << dp[0][N - 1] << endl;
    return 0;
}
```

## 4.7 遷移形式の工夫
### A22 - Sugoroku

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_v

```cpp
int N, A[100009], B[100009];

long long dp[100009];

int main()
{
	cin >> N;
	for (int i = 1; i <= N - 1; i++) cin >> A[i];
	for (int i = 1; i <= N - 1; i++) cin >> B[i];
	for (int i = 2; i <= N; i++) dp[i] = -1000000000;
	dp[1] = 0;
	for (int i = 1; i <= N - 1; i++)
	{
		dp[A[i]] = max(dp[i] + 100, dp[A[i]]);
		dp[B[i]] = max(dp[i] + 150, dp[B[i]]);
	}
	cout << dp[N] << endl;
	return 0;
}
```

### B22 （A16 - Dungeon 1）

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_p

ダンジョンの最短移動時間を求める問題。n番目の部屋への最短移動時間は(n-1)番目と(n-2)番目の部屋への最短移動時間を利用して解いていく。

4.1で扱った問題を配る遷移方式で解いていく。

```cpp
int N, A[100009], B[100009];
long long dp[100009];

int main()
{
	cin >> N;
	for (int i = 2; i <= N; i++) cin >> A[i];
	for (int i = 3; i <= N; i++) cin >> B[i];
	for (int i = 0; i <= N; i++) dp[i] = 100000000000;
	dp[1] = 0;
	for (int i = 1; i <= N - 1; i++)
	{
		dp[i + 1] = min(dp[i] + A[i + 1] , dp[i + 1]);
		dp[i + 2] = min(dp[i] + B[i + 2], dp[i + 2]);
	}
	cout << dp[N] << endl;
	return 0;
}
```

## 4.8 ビットDP
### A23 - All Free

クーポンを最小何枚使えばN個の商品が無料になるか、という問題。特定の商品の組み合わせが、最小何枚のクーポンで無料になるかを調べ、その情報を元に、次のクーポン１枚を使うとどうなるかを調べていく。つまりこの問題も貰う遷移方式で解いていく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_w


```cpp
int N, M, A[109][19];
int dp[109][1024];

int main()
{
	cin >> N >> M;
	//入力
	for (int i = 1; i <= M; i++)
	{
		for (int j = 1; j <= N; j++) cin >> A[i][j];
	}
	//配列の初期化
	for (int i = 0; i <= M; i++)
	{
		for (int j = 0; j <= (1 << N); j++) dp[i][j] = 1000000000;
	}
	//動的計画法
	dp[0][0] = 0;
	//クーポン1枚目からM枚目まで探索
	for (int i = 1; i <= M; i++)
	{
		for (int j = 0; j <= (1 << N); j++)
		{
			//すでに無料になっているかどうか調べる
			int already[19];
			for (int k = 1; k <= N; k++)
			{
				if ((j / (1 << (k - 1)) % 2) == 0) already[k] = 0;
				else already[k] = 1;
			}
			//クーポン券のカウント
			int coupon = 0;
			for (int k = 1; k <= N; k++)
			{
				if (A[i][k] == 1 || already[k] == 1)
				{
					coupon += (1 << (k - 1));
				}
			}
			//遷移
			dp[i][j] = min(dp[i][j], dp[i - 1][j]);
			dp[i][coupon] = min(dp[i][coupon], dp[i - 1][j] + 1);
		}
	}
	if (dp[M][(1 << N) - 1] == 1000000000) cout << "-1" << endl;
	else cout << dp[M][(1 << N) - 1] << endl;
	return 0;
}
```

### B23 - Traveling Salesman Problem

解くのに3時間かかった問題。最小の距離になるようにすべての建物を回った時に何分かかるか調べなさい、という問題。**巡回セールスマン問題**と呼ばれているらしい。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cv

```cpp
int N, X[20], Y[20];
float dp[32768][20];

float ft_distance(int x1, int y1, int x2, int y2)
{
	return sqrt(pow(x1 - x2, 2) + pow(y1 - y2, 2));
}

int main()
{
	//入力
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> X[i] >> Y[i];
	
	//配列の初期化
	for (int i = 0; i < (1 << N); i++)
	{
		for (int j = 0; j <= N; j++) dp[i][j] = 100000000;
	}
	//初期値の代入
	dp[1][1] = 0;
	//動的計画法
	//1の都市からスタート、すべての都市を巡る直前まで探索
	for (int i = 1; i <= (1 << N) - 1; i++)
	{
		//今いる都市をj、次に移動する都市をkと置いて
		for (int j = 1; j <= N; j++)
		{
			if (!(i & (1 << (j - 1)))) continue;//未到達の都市はスキップ
			//次に移動する都市をkとおく
			for(int k = 2; k <= N; k++)
			{
				if (i & (1 << (k - 1))) continue;//すでに到達している都市はスキップ
				dp[i | (1 << (k - 1))][k] = min(dp[i | (1 << (k - 1))][k], dp[i][j] + ft_distance(X[j], Y[j], X[k], Y[k]));
			}
		}
	}
	float ans = 100000000;
	for (int i = 1; i <= N; i++)
	{
		ans = min(ans, dp[(1 << N) - 1][i] + ft_distance(X[i], Y[i], X[1], Y[1]));
	}

	cout << ans << endl;
	return 0;
}
```

## 4.9 最長増加部分列問題
### A24 - LIS

最初何も考えずに動的計画法で記述したら間に合わなかった。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_x

```cpp
//何も考えずに動的計画法したら間に合わなかった
int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= N; i++)
	{
		dp[i] = 1;
		for (int j = 1; j < i; j++)
		{
			if (A[i] > A[j]) dp[i] = max(dp[i], dp[j] + 1);
		}
	}
	int ans = 1;
	for (int i = 1; i <= N; i++) ans = max(ans, dp[i]);
	cout << ans << endl;
	return 0;
}
```

それぞれの数の部分列を作るのに最小のmax値を記憶しておく配列を作り、さらにA[i]ごとにどの部分を変更するかを二分探索法を用いて効率化したら通過できた。

```cpp
int N, A[100009];
int dp[100009];


int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];
	int top = 1;
	for (int i = 1; i <= N; i++)
	{
		if (i == 1)
		{
			dp[1] = A[1];
		}
		else
		{
			if (dp[top] < A[i])
			{
				dp[top + 1] = A[i];
				top += 1;
			}
			else
			{
				int l = 1, r = top;
				while (l < r)
				{
					int mid = (l + r) / 2;
					if (dp[mid] < A[i])
					{
						l = mid + 1;
					}
					else
					{
						r = mid;
					}
				}
				dp[l] = A[i];
			}
		}
	}
	cout << top << endl;
	return 0;
}
```

### B24 - Many Boxes

この問題は難しかった。5時間近くかかってしまった。。

単純な動的計画法ならすぐ実装できたが、半分くらいの想定ケースでTLEになってしまっていた。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cw

```cpp
// 比較関数
bool customSort(const pair<int, int>& a, const pair<int, int>& b) {
    if (a.first == b.first) { // first が同じ場合
        return a.second > b.second; // second を降順で比較
    }
    return a.first < b.first; // first を昇順で比較
}

int main() {
    int N;
    cin >> N;

    vector<pair<int, int>> boxes(N);
    for (int i = 0; i < N; i++) {
        cin >> boxes[i].first >> boxes[i].second;
    }

    // カスタムの比較関数を使ってソート
    sort(boxes.begin(), boxes.end(), customSort);

    int ans = 0;
    vector<int> height;
    for (int i = 0; i < N; i++) {
        auto it = lower_bound(height.begin(), height.end(), boxes[i].second);
        int index = it - height.begin();
        if (it == height.end() || *it != boxes[i].second) {
            if (index == height.size()) {
                height.push_back(boxes[i].second);
            } else {
                height[index] = boxes[i].second;
            }
        }
        ans = max(ans, index + 1);
    }

    cout << ans << endl;

    return 0;
}

```

## 4.10 チャレンジ問題

### A25 - Number of Routes

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_y

```cpp
long long H, W;
char c[39][39];
long long dp[39][39];

int main()
{
	cin >> H >> W;
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++) cin >> c[i][j];
	}
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++)
		{
			if (i == 1 && j == 1) dp[i][j] = 1;
			else
			{
				dp[i][j] = 0;
				if (i > 1 && c[i - 1][j] == '.') dp[i][j] += dp[i - 1][j];
				if (j > 1 && c[i][j - 1] == '.') dp[i][j] += dp[i][j - 1];
			}
		}
	}
	cout << dp[H][W] << endl;
	return 0;
}
```

# 5章 数学的問題
## 5.0 数学的問題について
## 5.1 素数判定

### 問題 A26 Prime Check

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_z

```cpp
int Q, X[10009];

int main ()
{
	cin >> Q;
	for (int i = 1; i <= Q; i++) cin >> X[i];
	for (int j = 1; j <= Q; j++)
	{
		if (X[j] < 2)
		{
			cout << "No" << endl;
		}
		else
		{
			int flag = 1;
			for (int i = 2; i <= X[j] / i; i++)
			{
				if (X[j] % i == 0)
				{
					cout << "No" << endl;
					flag = -1;
					break;
				}
			}
			if (flag == 1)
				cout << "Yes" << endl;
		}
	}
	return 0;
}
```

### B26 - Output Prime Numbers

エラストテネスのふるいを用いて効率的に記述する

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cy

```cpp
int N;
bool deleted[1000009];

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++) deleted[i] = false;
	for (int i = 2; i <= sqrt(N); i++)
	{
		if (deleted[i] == true)
			continue;
		for (int j = i * 2; j <= N; j += i) deleted[j] = true;
	}
	for (int i = 2; i <= N; i++)
	{
		if (deleted[i] == false) cout << i << endl;
	}
	return 0;
}
```

## 5.2 最大公約数
### 問題 A27 Calculate GCD

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_o

```cpp
int A, B, min_AB, ans;

int main()
{
	cin >> A >> B;
	min_AB = min(A, B);
	ans = 1;
	for (int i = 2; i <= min_AB; i++)
	{
		if (A % i == 0 && B % i == 0)
		{
			ans = i;
		}
	}
	cout << ans << endl;
	return 0;
}
```

### B27 - Calculate LCM

最大公約数を用いて最小公倍数を求める問題。
A問題と同様のやり方ではTLEになってしまう。ユークリッドの互除法を用いて最大公約数は効率的に計算できることを学んだ。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cz

```cpp
long long a, b, lcm;

long long ft_GCD(long long A, long long B)
{
	while(A > 0 && B > 0)
	{
		if (A >= B) A = A % B;
		else B = B % A;
	}
	if (A != 0) return A;
	return B;
}

int main()
{
	cin >> a >> b;
	lcm = a * b / ft_GCD(a, b);
	cout << lcm << endl;
	return 0;
}
```
## 5.3 あまりの計算（1）：基本
### 問題 A28 Black Board

全て足し合わせた余りを求める問題。律儀に全て足していたらlong long型を超えてしまうので、足し算の性質を活かして毎回の計算で余りを求めている。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ab


```cpp
int main()
{
	int N;
	cin >> N;

	long long A[100009];
	char T[100009];
	for (int i = 1; i <= N; i++) cin >> T[i] >> A[i];
	long long ans = 0;
	for (int i = 1; i <= N; i++)
	{
		if (T[i] == '+') ans += A[i];
		if (T[i] == '-') ans -= A[i];
		if (T[i] == '*') ans *= A[i];
		if (ans < 10000) ans += 10000;
		ans %= 10000;
		cout << ans % 10000 << endl;
	}
	return 0;
}
```

### B28 - Fibonacci Easy (mod 1000000007)

フィボナッチ数列の第N項を求める問題。A問題と同様、毎度計算のたびにあまりを求めている。

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_ap

```cpp
int main()
{
	int N;
	cin >> N;
	long long A[10000009];
	A[1] = 1;
	A[2] = 1;
	for (int i = 3; i <= N; i++)
	{
		A[i] = A[i - 1] + A[i - 2];
		A[i] %= 1000000007;
	}
	cout << A[N] % 1000000007 << endl;
	return 0;
} 
```

## 5.4 あまりの計算（2）：累乗
### A29 - Power

余りを先に計算しておいても計算結果が変わらないのは足し算も掛け算も同じ。毎度の計算で余りを求めて計算していく。早くコードのようなビットを使いなせるようになりたい。

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_aq

```cpp
long long ft_power(long long a, long long b, long long m)
{
	long long p = a;
	long long ans = 1;
	for (int i = 0; i <= 30; i++)
	{
		int wari = (1 << i);
		if ((b / wari) % 2 == 1)
		{
			ans = (ans * p) % m;
		}
		p = (p * p) % m;
	}
	return ans;
}

int main()
{
	int a, b;
	long long ans;
	cin >> a >> b;
	cout << ft_power(a, b, 1000000007) << endl;
	return 0;
}
```

### B29 - Power Hard

A問題はint型のwariという別の変数でbのどこにビットが立っているかを確認して行ったが、int型を超えてしまうからか、正常に動作しなかった。

そのため、bのビットを直接動かす形に書き換えたらACとなった。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_db

```cpp
unsigned long long ft_power(unsigned long long a, unsigned long long b, unsigned long long m)
{
	unsigned long long ans = 1;
	a %= m;
	while (b > 0)
	{
		if (b & 1)
			ans = (ans * a) % m;
		b >>= 1;
		a = (a * a) % m;
	}
	return ans ;
}

int main()
{
	unsigned long long a, b;
	cin >> a >> b;
	cout << ft_power(a, b, 1000000007) << endl;
	return 0;
}
```

## 5.5 あまりの計算（3）：割り算
ここまで足し算・掛け算の場合はうまくいきましたが、割り算となると話が変わります。

普通に毎度の余を求めて計算がうまくいかないの（試してみてください）ので、フェルマーの小定理を用いて式変形を行います。
フェルマーの小定理は[こちら](https://qiita.com/drken/items/6b4031ccbb2cab7436f3)のqiitaの記事がわかりやすいです。競技プログラミングにおける例題も挙げて頂いているので問題演習にもいいかもですね。

### A30 - Combination

組み合わせのが何通りあるか調べるnCrの計算。なぜか書籍に載っていた解答例だとACが出なかった。前述のft_power()関数を用いてr項分の掛け算をしていく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ad

```cpp
unsigned long long ft_division(unsigned long long a, unsigned long long b, unsigned long long m)
{
	return (a * ft_power(b , m - 2, m)) % m;
}


int main()
{
	unsigned long long m = 1000000007;
	unsigned long long n;
	unsigned long long r;
	cin >> n >> r;

	unsigned long long ans = 1;
	for (int i = 1; i <= r; i++)
	{
		ans = (ans * ft_division(n - i + 1, r - i + 1, m)) % m;
	}
	
	cout << ans << endl;
	return 0;
}
```

### B30 - Combination 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dc

```cpp
unsigned long long ft_power(unsigned long long a, unsigned long long b, unsigned long long m)
{
	unsigned long long ans = 1;
	a %= m;
	while (b > 0)
	{
		if (b & 1)
			ans = (ans * a) % m;
		b >>= 1;
		a = (a * a) % m;
	}
	return ans ;
}

unsigned long long ft_division(unsigned long long a, unsigned long long b, unsigned long long m)
{
	return (a * ft_power(b , m - 2, m)) % m;
}


int main()
{
	unsigned long long m = 1000000007;
	unsigned long long H;
	unsigned long long W;
	cin >> H >> W;

	unsigned long long ans = 1;
	unsigned long long n = H + W - 2;
	unsigned long long r = H - 1;
	for (int i = 1; i <= r; i++)
	{
		ans = (ans * ft_division(n - i + 1, r - i + 1, m)) % m;
	}
	
	cout << ans << endl;
	return 0;
}
```

## 5.6 包除原理
### A31 - Divisors

和集合の要素数を求める問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ae

```cpp
int main()
{
	long long N;
	cin >> N;

	long long d3 = N / 3;
	long long d5 = N / 5;
	long long d15 = N / 15;

	long long ans = d3 + d5 -d15;
	cout << ans << endl;
	return 0;
}
```

### B31 - Divisors Hard

こちらは3つの集合の和集合の要素数を求める問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dd

```cpp
int main()
{
	long long N;
	cin >> N;

	long long d3 = N / 3;
	long long d5 = N / 5;
	long long d7 = N / 7;
	long long d15 = N / 15;
	long long d35 = N / 35;
	long long d21 = N / 21;
	long long d105 = N / 105;

	long long ans = d3 + d5 + d7 -d15 - d21 -d35 + d105;
	cout << ans << endl;
	return 0;
}
```
## 5.7 ゲーム（1）:必勝法
### A32 - Game 1

山の石をお互いが最善手で取り合った時にどちらが勝つかを出力する問題。理解できたら現実世界でも仕掛けてみたい問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_af

```cpp
int main()
{
	int N, A, B;
	cin >> N >> A >> B;

	bool dp[100009];
	for (int i = 1; i <= N; i++)
	{
		if (i > A && dp[i - A] == false) dp[i] = true;
		else if (i > B && dp[i - B] == false) dp[i] = true;
		else dp[i] = false;
	}
	// 結果の出力
	if (dp[N] == true) cout << "First" << endl;
	else cout << "Second" << endl;
	return 0;
}
```

### B32 - Game 5


A問題の取れる選択肢がK個に増えたバージョン。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_de

```cpp
int main()
{
	int N, K;
	cin >> N >> K;
	int a[109];
	for (int i = 1; i <= K; i++) cin >> a[i];

	bool dp[100009];
	dp[0] == false;
	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= K; j++)
		{
			if (i >= a[j] && dp[i - a[j]] == false)
			{
				dp[i] = true;
				break ;
			}
			dp[i] = false;
		}
	}
	// 結果の出力
	if (dp[N] == true) cout << "First" << endl;
	else cout << "Second" << endl;
	return 0;
}
```

## 5.8 ゲーム（2）:ニム

### A33 - Game 2

n個の山から石を取っていき、石を取れなくなったら負けというゲームの勝者を求める。ニム和というものを求めればいいらしいが、理解はまだできていない、、


後から[この論文](https://www.xmath.ous.ac.jp/~shibata/semi/2019_Nim.pdf)を読もうと思うので備忘録として残しておく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ag


```cpp
int main()
{
	int N;
	int A[100009];
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];

	// ニム和を求める
	int xor_sum = A[1];
	for (int i = 2; i <= N; i++) xor_sum = (xor_sum ^ A[i]);

	//出力
	if (xor_sum == 0) cout << "Second" << endl;
	else cout << "First" << endl;
	return 0;
}
```

### B33 - Game 6

「H*Wのマス目の一つのコマを選美、左方向か上方向の1方向に1マス以上移動させる」ため、ゲームの終了条件としては、すべてのコマが左上に来ることである。

コマを動かすときは上か左の1方向しか動かせないので、縦と横の軸を別々でnim和をとると山の数が2Nのニムに帰結できる。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_df

```cpp
int main()
{
	int N, H , W;
	int A[100009];
	int B[100009];
	cin >> N >> H >> W;
	for (int i = 1; i <= N; i++)
	{
		cin >> A[i] >> B[i];
		A[i] -= 1;
		B[i] -= 1;
	}

	// ニム和を求める
	int xor_sum_a = A[1];
	for (int i = 2; i <= N; i++) xor_sum_a = (xor_sum_a ^ A[i]);
	int xor_sum_b = B[1];
	for (int i = 2; i <= N; i++) xor_sum_b = (xor_sum_b ^ B[i]);
	// 出力
	int xor_sum = xor_sum_a ^ xor_sum_b;
	if (xor_sum == 0) cout << "Second" << endl;
	else cout << "First" << endl;
	return 0;
}
```

## 5.9 ゲーム（3）:Grundy数
### A34 - Game 3

i個の石の場合のGrundy数を求めておいて、最後にニム和を求める問題。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ah

```cpp
int main()
{
	int N, X, Y;
	int A[100009];
	cin >> N >> X >> Y;
	for (int i = 1; i <= N; i++) cin >> A[i];

	// Grundy数を求める
	int grundy[100009];
	for (int i = 0; i <= 100000; i++)
	{
		bool Transit[3] = {false, false, false};
		if (i >= X) Transit[grundy[i-X]] = true;
		if (i >= Y) Transit[grundy[i-Y]] = true;
		if (Transit[0] == false) grundy[i] = 0;
		else if (Transit[1] == false) grundy[i] = 1;
		else grundy[i] = 2;
	}

	int xor_sum = 0;
	for (int i = 1; i <= N; i++) xor_sum = xor_sum ^ grundy[A[i]];
	if (xor_sum == 0) cout << "Second" << endl;
	else cout << "First" << endl;
	return 0;
}
```

### B34 - Game 7

上のA問題の制約が特殊なバージョン。山から削るのは2 or 3に縛られていて、山の個数が膨大な制約である。規則性を見つけれたらむしろA問題よりも簡単かもしれない。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dg

```cpp
int main()
{
	int N, X, Y;
	long long  A;
	cin >> N >> X >> Y;
	int xor_sum = 0;
	int grundy[5] = {0,0,1,1,2};
	for (int i = 1; i <= N; i++)
	{
		cin >> A;
		xor_sum = xor_sum ^ grundy[A % 5];
	}

	if (xor_sum == 0) cout << "Second" << endl;
	else cout << "First" << endl;
	return 0;
}
```

## 5.10 チャレンジ問題
### A35 - Game 4

なぜかもうすでに懐かしい動的計画法。問題は簡単dが、似たような考え方はMinimax法というリバーシAIなどにも使用されている概念らしい。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ai

```cpp
int main()
{
	int N;
	int A[2009];
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];

	int dp[2009][2009];
	// 最下層の数字を埋める
	for (int j = 1; j <= N; j++) dp[N][j] = A[j];
	//上に上に埋めていく
	for (int i = N - 1; i >= 1; i--)
	{
		for (int j = 1; j <= i; j++) 
		{
			if (i % 2 == 1)
			{
				dp[i][j] = max(dp[i + 1][j], dp[i + 1][j + 1]);
			}
			else
			{
				dp[i][j] = min(dp[i + 1][j], dp[i + 1][j + 1]);
			}
		}
	}
	cout << dp[1][1] << endl;
	return 0;
}
```


# 6章 考察テクニック
## 6.0 考察テクニック入門
競プロにはアルゴリズムだけでなく、数理パズルを解くような考察力・ひらめき力も必要になってくることが強調されていた。一見複雑で難しそうな問題を見方を変えて単純化できた時の喜びは計り知れない。

## 6.1 偶奇を考える
### A36 - Travel

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_aj

```cpp
int main()
{
	int N, K;
	cin >> N >> K;
	if ((2 * N - 2) <= K && ((2 * N - 2) - K) % 2 == 0)  cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```

### B36 - Switching Lights

偶奇に着目する問題。何度操作しても偶奇は入れ替わらない。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_di

```cpp
int main()
{
	int N, K;
	string S;
	cin >> N >> K;
	cin >> S;
	int count = 0;
	for(size_t i = 0; i < S.size();i++)
	{
		if (S[i] == '1') count++;
	}
	if (count % 2 == K % 2) cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```

## 6.2 足された回数を考える
### A37 - Travel 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ak

```cpp
int main()
{
	long long N, M, B;
	long long A[200009];
	long long C[200009];
	cin >> N >> M >> B;
	int sum_time_routeA = 0;
	int sum_time_routeC = 0;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= M; i++) cin >> C[i];
	for (int i = 1; i <= N; i++) sum_time_routeA += A[i];
	for (int i = 1; i <= M; i++) sum_time_routeC += C[i];
	long long total_time = sum_time_routeA * M + sum_time_routeC * N + B * N * M; 
	cout << total_time << endl;
	return 0;
}
```

### B37 - Sum of Digits

f(n)をnの各桁の数字を足し合わせた合計とした時、1からNまでの数字のf(n)の合計を出力せよという問題。k桁目が+1された場合にどのくらい数が増えるかを考える。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dj

```cpp
int main() {
    long long N;
    cin >> N;

    long long power = 1;
    long long ans = 0;
    long long accu = 0;

    while (power <= N) {
        long long digit = ((N % (power * 10)) - (N % power)) / power;
        if (digit > 0) {
            ans += accu * digit;
            ans += digit * (N % power + 1);
            while (digit > 1) {
                digit--;
                ans += digit * power;
            }
        }
        accu = accu * 10 + 45 * power;
        power *= 10;
    }

    cout << ans << endl;
    return 0;
}
```

## 6.3 上限値を考える
### A38 - Black Company 1

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_al

```cpp
int main()
{
	int D, N;
	int L[10009], R[10009], H[10009];
	int max_worktime[10009];
	cin >> D >> N;
	for (int i = 1; i <= N; i++) cin >> L[i] >> R[i] >> H[i];
	for (int i = 1; i <= D; i++) max_worktime[i] = 24;
	for (int i = 1; i <= N; i++)
	{
		for (int j = L[i]; j <= R[i]; j++)
		{
			max_worktime[j] = min(max_worktime[j], H[i]);
		}
	}
	int total_worktime = 0;
	for (int i = 1; i <= D; i++) total_worktime += max_worktime[i];
	cout << total_worktime << endl;
	return 0;
}
```

### B38 - Heights of Grass

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dk

```cpp
int main()
{
	int N;
	string S;
	cin >> N >> S;
	int min_grass[3009];
	// それぞれの草の高さの最小値を初期化
	min_grass[0] = 1;
	for (int i = 0; i < N - 1; i++)
	{
		if (S[i] == 'A')
		{
			min_grass[i + 1] = min_grass[i] + 1;
		} 
		if (S[i] == 'B')
		{
			min_grass[i + 1] = 1;
			// もし1かつ右肩下がりだったら左側の要素も1追加
			if (min_grass[i] == 1)
			{
				int j = 0;
				while (min_grass[i - j] == min_grass[i + 1 - j])
				{
					min_grass[i - j]++;
					j++;
				}
			}
		} 
	}
	int ans = 0;
	for (int i = 0; i < N; i++) ans += min_grass[i];
	cout << ans << endl;
	return 0;
}
```

## 6.4 一手先を考える
### A39 - Interval Scheduling Problem

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_bn

```cpp
int main()
{
	int N;
	int L[300009];
	int R[300009];
	vector<pair<int,int>> tmp;
	cin >> N;
	for (int i = 1; i <= N; i++)
	{
		cin >> L[i] >> R[i];
		tmp.push_back(make_pair(R[i], L[i]));
	}
	sort(tmp.begin(), tmp.end());
	for (int i = 1; i <= N; i++)
	{
		R[i] = tmp[i - 1].first;
		L[i] = tmp[i - 1].second;
	}


	int current = 0;
	int count = 0;
	for(int i = 1; i <= N; i++)
	{
		if (current <= L[i])
		{
			current = R[i];
			count++;
		}
	}
	cout << count << endl;
	return 0;
}
```

### B39 - Taro's Job

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dl

```cpp
int main()
{
	int N ,D, X[200009], Y[200009];
	vector<pair<int, int>> tmp;
	vector<int> work_available;
	cin >> N >> D;
	for (int i = 1; i <= N; i++)
	{
		cin >> X[i] >> Y[i];
		tmp.push_back(make_pair(X[i],Y[i]));
	}
	sort(tmp.begin(), tmp.end());
	for (int i = 1; i <= N; i++)
	{
		X[i] = tmp[i - 1].first;
		Y[i] = tmp[i - 1].second;
	}
	int j = 1;
	int total_money = 0;
	for (int i = 1; i <= D; i++)
	{
		while (i == X[j])
		{
			work_available.push_back(Y[j]);
			j++;
		}
		if (!work_available.empty())
		{
			auto max_elem = max_element(work_available.begin(), work_available.end());
			total_money += *max_elem;
			work_available.erase(max_elem);
		}
	}
	cout << total_money << endl;
	return 0;
}
```

## 6.5 個数を考える
### A40 - Triangl

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_an

```cpp
long long ft_choose(long long n)
{
	return  (n * (n - 1) * (n - 2)) / 6;
}

int main()
{
	long long N, A[109];
	cin >> N;
	for (int i = 1; i <= 100; i++) A[i] = 0;
	for (int i = 1; i <= N; i++)
	{
		int tmp;
		cin >> tmp;
		A[tmp]++;
	}
	long long ans = 0;
	for (int i = 1; i <= 100; i++)
	{
		if (A[i] >= 3)
			ans += ft_choose(A[i]);
	}
	cout << ans << endl;
	return 0;
}
```

### B40 - Divide by 100

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dm

```cpp
long long ft_choose(long long n)
{
	return  (n * (n - 1)) / 2;
}

long long ft_combi(long long a, long long b)
{
	return a * b;
}

int main()
{
	int N;
	long long A[109];
	cin >> N;
	for (int i = 0; i < 100; i++) A[i] = 0;
	for (int i = 1; i <= N; i++)
	{
		long long tmp;
		cin >> tmp;
		A[tmp % 100]++;
	}
	long long ans = 0;
	if (A[0] > 1)
		ans += ft_choose(A[0]);
	if (A[50] > 1)
		ans += ft_choose(A[50]);
	for (int i = 1; i < 50; i++)
	{
		ans += ft_combi(A[i], A[100 - i]);
	}
	cout << ans << endl;
	return 0;
}
```

## 6.6 後ろから考える
### A41 - Tile Coloring

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ao

```cpp
int main()
{
	int N;
	int count_blue = 0;
	int count_red = 0;
	char S;
	cin >> N;
	for (int i = 1; i <= N; i++)
	{
		cin >> S;
		if (S == 'B')
		{
			count_blue++;
			count_red = 0;
		}
		else
		{
			count_blue = 0;
			count_red++;
		}
		if (count_blue >= 3 || count_red >= 3)
		{
			cout << "Yes" << endl;
			return 0;
		}
	}
	cout << "No" << endl;
	return 0;
}
```

### B41 - Reverse of Euclid

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dn

```cpp
int main()
{
	long long X, Y;
	long long A[1000009], B[1000009];
	cin >> X >> Y;
	long long count = 0;
	while (X > 1 || Y > 1)
	{
		A[count] = X;
		B[count] = Y;
		if (X > Y) X -= Y;
		else Y -= X;
		count++;
	}
	cout << count << endl;
	if (count > 0)
	{
		for (int i = count - 1; i >= 0; i--)
		{
			cout << A[i] << " " << B[i] << endl;
		}
	}
}
```

## 6.7 固定して全探索
### A42 - Soccer

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ap


```cpp
int main()
{
	int N, K;
	int A[309], B[309];
	int max_member[109][109];
	cin >> N >> K;
	for (int i = 1; i <= N; i++) cin >> A[i] >> B[i];
	for (int i = 1; i <= 100; i++)
	{
		for (int j = 1; j <= 100; j++) max_member[i][j] = 0;
	}
	for (int i = 1; i <= 100 - K; i++)
	{
		for (int j = 1; j <= 100 - K; j++)
		{
			for (int k = 1; k <= N; k++)
			{
				if ((i <= A[k] && A[k] <= i + K) && (j <= B[k] && B[k] <= j + K))
					max_member[i][j]++;
			}
		}
	}
	int ans = 0;
	for (int i = 1; i <= 100 - K; i++)
	{
		for (int j = 1; j <= 100 - K; j++)
		{
			ans = max(ans, max_member[i][j]);
		}
	}
	cout << ans << endl;
	return 0;
}
```

### B42 - Two Faced Card

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_do

```cpp
int main()
{
	int N;
	long long A[100009], B[100009];
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i] >> B[i];
	long long truetrue = 0;
	long long truefalse = 0;
	long long falsetrue = 0;
	long long falsefalse = 0;
	for (int i = 1; i <= N; i++)
	{
		if ((A[i] >= 0 && B[i] >= 0) || (!(A[i] < 0 && B[i] < 0) && (A[i] + B[i]) > 0))
			truetrue += (A[i] + B[i]);
	}
	for (int i = 1; i <= N; i++)
	{
		if ((A[i] >= 0 && B[i] <= 0) || (!(A[i] < 0 && B[i] > 0) && (A[i] - B[i] > 0)))
			truefalse += (A[i] - B[i]);
	}
	for (int i = 1; i <= N; i++)
	{
		if ((A[i] <= 0 && B[i] >= 0) || (!(A[i] > 0 && B[i] < 0) && (B[i] - A[i]) > 0))
			falsetrue += (B[i] - A[i]);
	}
	for (int i = 1; i <= N; i++)
	{
		if ((A[i] <= 0 && B[i] <= 0) || (!(A[i] > 0 && B[i] > 0) && (A[i] + B[i]) < 0))
			falsefalse -= (A[i] + B[i]);
	}
	long long ans = max(truetrue, falsefalse);
	long long tmp = max(truefalse, falsetrue);
	ans = max(ans, tmp);
	cout << ans << endl;
	return 0;
}
```

## 6.8 問題を言い換える
### A43 - Travel 3

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_aq

```cpp
int main()
{
	int N, L;
	int A;
	char B;
	int ans = 0;
	cin >> N >> L;
	for (int i = 1; i <= N; i++)
	{
		cin >> A >> B;
		if (B == 'W') ans = max(ans, A);
		else ans = max(ans, L - A);
	}
	cout << ans << endl;
	return 0;
}
```

### B43 - Quiz Contest

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dp

```cpp
int main()
{
	int N, M;
	int student[200009];
	cin >> N >> M;
	for(int i = 1; i <= N; i++) student[i] = M;
	for(int i = 1; i <= M; i++)
	{
		int A;
		cin >> A;
		student[A]--;
	}
	for(int i = 1; i <= N; i++) cout << student[i] << endl;
	return 0;
}
```

## 6.9 データの持ち方を工夫する
### A44 - Change and Reverse

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ar

```cpp
int main()
{
	int N, Q;
	int A[200009];
	bool reverse = false;
	cin >> N >> Q;
	for (int i = 1; i<= N; i++) A[i] = i;
	for (int i = 1; i<= Q; i++)
	{
		int query;
		cin >> query;
		if (query == 1)
		{
			int x,y;
			cin >> x >> y;
			if (reverse == false)
				A[x] = y;
			else A[N + 1 - x] = y;
		}
		if (query == 2)
		{
			reverse = !reverse;
		}
		if (query == 3)
		{
			int x;
			cin >> x;
			if (reverse == false)
				cout << A[x] << endl;
			else
				cout << A[N + 1 - x] << endl;
		}
	}
	return 0;
}
```

### B44 - Grid Operations

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dq

```cpp
int main()
{
	int N, Q;
	int A[509][509];
	int row[509];
	// 入力
	cin >> N;
	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= N; j++) cin >> A[i][j];
		row[i] = i;
	}
	cin >> Q;
	for (int i = 1; i <= Q; i++)
	{
		int q, x, y;
		cin >> q >> x >> y;
		if (q == 1)
		{
			swap(row[x], row[y]);
		}
		if (q == 2)
		{
			cout << A[row[x]][y] << endl;
		}
	}
	return 0;
}
```

## 6.10 不変量に着目する
### A45 - Card Elimination

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_as

```cpp
int main()
{
	int N;
	char C;
	string S;
	cin >> N >> C >> S;
	int total = 0;
	for (int i = 0; i < S.size(); i++)
	{
		if (S[i] == 'B') total += 1; 
		if (S[i] == 'R') total += 2; 
	}
	if (C == 'W' && total % 3 == 0)
	{
		cout << "Yes" << endl;
		return 0;
	}
	if (C == 'B' && total % 3 == 1) 
	{
		cout << "Yes" << endl;
		return 0;
	}
	if (C == 'R' && total % 3 == 2) 
	{
		cout << "Yes" << endl;
		return 0;
	}
	cout << "No" << endl; 
	return 0;
}
```


### B45 - Blackboard 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dr

```cpp
int main()
{
	long long a, b, c;
	cin >> a >> b >> c;
	if (a + b + c == 0) cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```

# 7章 ヒューリスティック
## 7.0 ヒューリスティック系コンテストとは
## 7.1 貪欲法
### A46 - Heuristic 1

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_at


```cpp
int N;
int X[159], Y[159], route[159];
bool visited[159];

double ft_distance(int a, int b)
{
	return sqrt((X[a] - X[b]) *  (X[a] - X[b]) + (Y[a] - Y[b]) *  (Y[a] - Y[b]));
}

void	ft_donyoku()
{
	for (int i= 1; i <= N; i++) visited[i] = false;
	route[1] = 1;
	visited[1] = true;
	int current = 1;

	for (int i = 2; i <= N; i++)
	{
		double min_distance = 1000000.0;
		int next_place = -1;
		for (int j= 1; j <= N; j++)
		{
			if (visited[j] == true) continue;
			double new_distance = ft_distance(current, j);
			if (min_distance > new_distance)
			{
				min_distance = new_distance;
				next_place = j;
			}
		}
		visited[next_place] = true;
		route[i] = next_place;
		current = next_place;
	}
	route[N + 1] = 1;
}


int main()
{
	cin >> N;
	for (int i= 1; i <= N; i++) cin >> X[i] >> Y[i];

	ft_donyoku();
	for (int i= 1; i <= N + 1; i++) cout << route[i] << endl;
	return 0;
}
```

## 7.2 局所探索法
7.1と同様の問題を局所探索法（山登り法）で解く。

```cpp
int N;
int X[159], Y[159], route[159];
bool visited[159];

double ft_distance(int a, int b)
{
	return sqrt((X[a] - X[b]) *  (X[a] - X[b]) + (Y[a] - Y[b]) *  (Y[a] - Y[b]));
}

int ft_rand(int a, int b)
{
	return a + rand() % (b - a + 1);
}

double ft_get_score()
{
	double sum = 0;
	for (int i = 1; i <= N ; i++) sum += ft_distance(route[i], route[i + 1]);
	return sum;
}


void	ft_local_search()
{
	// 初期解の生成
	for (int i = 1; i <= N ; i++) route[i] = i;
	route[N + 1] = 1;
	double current_score = ft_get_score();

	for (int i = 0; i < 2000; i++)
	{
		// ランダムに右端と左端を決める
		int R = ft_rand(2, N);
		int L = ft_rand(2, N);
		if (R < L) swap(L, R);
		// 配列を逆にしてみる
		reverse(route + L, route + R + 1);
		double new_score = ft_get_score();
		// スコア改善したらそのまま、しなかったら戻す
		if (new_score <= current_score) current_score = new_score;
		else reverse(route + L, route + R + 1);
	}
}

int main()
{
	cin >> N;
	for (int i= 1; i <= N; i++) cin >> X[i] >> Y[i];

	ft_local_search();
	for (int i= 1; i <= N + 1; i++) cout << route[i] << endl;
	return 0;
}
```

## 7.3 焼きなまし法
関数の名前がannealingではなくlocal_searchのままになっているほど、7.2の山登り法とほとんど同じコードです。

```cpp
int N;
int X[159], Y[159], route[159];
bool visited[159];

void	ft_local_search()
{
	// 初期解の生成
	for (int i = 1; i <= N ; i++) route[i] = i;
	route[N + 1] = 1;
	double current_score = ft_get_score();

	for (int i = 0; i < 200000; i++)
	{
		// ランダムに右端と左端を決める
		int R = ft_rand(2, N);
		int L = ft_rand(2, N);
		if (R < L) swap(L, R);
		// 配列を逆にしてみる
		reverse(route + L, route + R + 1);
		double new_score = ft_get_score();

		// 焼きなまし法
		// 温度Tが大きいほど許す範囲も大きくなる（初期は大きく）
		double T = 30 - 30 * i / 200000;
		// currentscore < newscore となっていても、確率的に許す
		double probability = exp(min(0.0, (current_score - new_score) / T));
		// スコア改善したらそのまま、しなかったら戻す
		if (ft_rand_double() <= probability) current_score = new_score;
		else reverse(route + L, route + R + 1);
	}
}

int main()
{
	cin >> N;
	for (int i= 1; i <= N; i++) cin >> X[i] >> Y[i];

	ft_local_search();
	for (int i= 1; i <= N + 1; i++) cout << route[i] << endl;
	return 0;
}
```

## 7.4 ビームサーチ
### A49 - Heuristic 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_aw

```cpp
struct State
{
	int score;		// 暫定のスコア
	int X[29];		// 暫定のXの配列
	char LastMove;	// 最後の動き
	int LastPos;	// Beam@[i - 1][？]から遷移したか
};

bool	operator>(const State& a1, const State& a2)
{
	if (a1.score > a2.score) return true;
	else return false;
}

const int WIDTH = 10000; // ビーム幅
const int N = 20;
int T, P[109], Q[109], R[109];
int NumState[109];
State Beam[109][WIDTH];
char Answer[109];

void BeamSearch()
{
	// 初期状態の設定
	NumState[0] = 1;
	Beam[0][0].score = 0;
	for(int i= 1; i<= N; i++) Beam[0][0].X[i] = 0;

	// ビームサーチ
	for(int i = 1; i <= T; i++)
	{
		vector<State> Candidate;
		for(int j = 0; j <= NumState[i -1]; j++)
		{
			// 操作Aの場合
			State SousaA = Beam[i - 1][j];
			SousaA.LastMove = 'A';
			SousaA.LastPos = j;
			SousaA.X[P[i]] += 1;
			SousaA.X[Q[i]] += 1;
			SousaA.X[R[i]] += 1;
			for(int k = 1; k <= T; k++)
			{
				if (SousaA.X[k] == 0) SousaA.score += 1;
			}
			
			// 操作Bの場合
			State SousaB = Beam[i - 1][j];
			SousaB.LastMove = 'B';
			SousaB.LastPos = j;
			SousaB.X[P[i]] += 1;
			SousaB.X[Q[i]] += 1;
			SousaB.X[R[i]] += 1;
			for(int k = 1; k <= T; k++)
			{
				if (SousaB.X[k] == 0) SousaB.score += 1;
			}
			Candidate.push_back(SousaA);
			Candidate.push_back(SousaB);
		}
		sort(Candidate.begin(), Candidate.end(), greater<State>());
		NumState[i] = min(WIDTH, (int)Candidate.size());
		for(int j = 0; j <= NumState[i]; j++) Beam[i][j] = Candidate[j];
	}
}

int main()
{
	// 入力
	cin >> T;
	for (int i = 1; i <= T; i++) cin >> P[i] >> Q[i] >> R[i];
	// ビームサーチ
	BeamSearch();
	// ビームサーチの復元
	int current_place = 0;
	for (int i = T; i >= 1; i--)
	{
		Answer[i] = Beam[i][current_place].LastMove; 
		current_place = Beam[i][current_place].LastPos; 
	}
	// 出力
	for (int i = 1; i <= T; i++) cout << Answer[i] << endl;;
}
```

## 7.5 チャレンジ問題
### A50 - 山型足し算

実際の大会で出題された問題らしく、非常に歯応えがある問題。（難しい）

正直まだ理解できていないので、再実装必須。

https://atcoder.jp/contests/tessoku-book/tasks/future_contest_2018_qual_a

```cpp
#include <iostream>
#include <cmath>
#include <ctime>
#include <algorithm>
using namespace std;

int N = 100;
int Q = 1000;
int A[109][109], B[109][109];
int X[1009], Y[1009], H[1009];

// L 以上 R 以下のランダムな整数を返す関数
int RandInt(int L, int R) {
	return rand() % (R - L + 1) + L;
}

// 現在のスコアを返す関数
int GetScore() {
	int sum = 0;
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) sum += abs(A[i][j] - B[i][j]);
	}
	return 200000000 - sum;
}

// X[t]=x, Y[t]=y, H[t]=h に変更する関数
void Change(int t, int x, int y, int h) {
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			B[j][i] -= max(0, H[t] - abs(X[t] - i) - abs(Y[t] - j));
		}
	}
	X[t] = x;
	Y[t] = y;
	H[t] = h;
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			B[j][i] += max(0, H[t] - abs(X[t] - i) - abs(Y[t] - j));
		}
	}
}

void Yamanobori() {
	// 変数の設定（5.95 秒ループを回す）
	int TIMELIMIT = 5.95 * CLOCKS_PER_SEC;
	int CurrentScore = GetScore();
	int ti = clock();

	// 山登り法スタート
	while (clock() - ti < TIMELIMIT) {
		int t = RandInt(1, Q);
		int old_x = X[t], new_x = X[t] + RandInt(-9, 9);
		int old_y = Y[t], new_y = Y[t] + RandInt(-9, 9);
		int old_h = H[t], new_h = H[t] + RandInt(-19, 19);
		if (new_x < 0 || new_x >= N) continue;
		if (new_y < 0 || new_y >= N) continue;
		if (new_h <= 0 || new_h > N) continue;

		// とりあえず変更し、スコアを評価する
		Change(t, new_x, new_y, new_h);
		int NewScore = GetScore();

		// スコアに応じて採用／不採用を決める
		if (CurrentScore < NewScore) CurrentScore = NewScore;
		else Change(t, old_x, old_y, old_h);
	}
}

int main() {
	// 入力
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) cin >> A[i][j];
	}

	// 初期解を生成
	for (int i = 1; i <= 1000; i++) {
		X[i] = rand() % N; // 0 以上 N-1 以下のランダムな整数
		Y[i] = rand() % N; // 0 以上 N-1 以下のランダムな整数
		H[i] = 1;
		B[X[i]][Y[i]] += 1;
	}

	// 山登り法
	Yamanobori();

	// 出力
	cout << "1000" << endl;
	for (int i = 1; i <= 1000; i++) {
		cout << X[i] << " " << Y[i] << " " << H[i] << endl;
	}
	return 0;
}
```

## column4 再帰関数 

# 8章 データ構造とクエリ処理
## 8.0 データ構造とは
## 8.1 スタック
### A51 - Stack

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ay

```cpp
int Q;
int Query[100009];
string x[100009];
stack<string> S;


int main()
{
	cin >> Q;
	for (int i = 1; i <= Q; i++)
	{
		cin >> Query[i];
		if (Query[i] == 1)
			cin >> x[i]; 
	}
	// クエリ処理
	for (int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1) S.push(x[i]);
		if (Query[i] == 2) cout << S.top() << endl;
		if (Query[i] == 3) S.pop();
		
	}
	return 0;
}
```

### B51 - Bracket

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dx

```cpp
string S;
stack<int> brackets;

int main()
{
	cin >> S;
	for (size_t i = 0; i < S.size(); i++)
	{
		if (S[i] == '(')
			brackets.push(i);
		else
		{
			cout << brackets.top() + 1 << " " << i + 1 << endl;
			brackets.pop();
		}
	}
	return 0;
}
```

## 8.2 キュー
### A52 - Queue

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_az

```cpp
int Q;
int Query[100009];
string x[100009];
queue<string> S;

int main()
{
	cin >> Q;
	for(int i = 1; i <= Q; i++)
	{
		cin >> Query[i];
		if (Query[i] == 1) cin >> x[i];
	}
	// クエリ処理
	for(int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1) S.push(x[i]);
		if (Query[i] == 2) cout << S.front() << endl;
		if (Query[i] == 3) S.pop();
	}
	return 0;
}
```

### B52 - Ball Simulation

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_dy


```cpp
int N , X;
string A;
int ball_color[100009];
queue<int> ball;

int main()
{
	// 入力
	cin >> N >> X >> A;
	for (int i = 1; i <= N; i++)
	{
		if (A[i - 1] == '#')
			ball_color[i] = 1;
		if (A[i - 1] == '.')
			ball_color[i] = 2;
	}
	// 初期値の設定
	ball_color[X] = 3;
	ball.push(X);
	// クエリ処理
	while(!ball.empty())
	{
		int pos = ball.front();
		ball.pop();
		if (ball_color[pos - 1] == 2 && pos - 1 > 0)
		{
			ball_color[pos - 1] = 3;
			ball.push(pos - 1);
		}
		if (ball_color[pos + 1] == 2 && pos + 1 <= N)
		{
			ball_color[pos + 1] = 3;
			ball.push(pos + 1);
		}
	}
	// 出力
	for (int i = 1; i <= N; i++)
	{
		if (ball_color[i] == 1) cout << "#";
		if (ball_color[i] == 2) cout << ".";
		if (ball_color[i] == 3) cout << "@";
	}
	cout << endl;
	return 0;
}
```

## 8.3 優先度つきキュー
要素を昇順や降順で保存しておくことができるキュー。

デフォルトでは降順になっているので、小さいものから取り出したい時には昇順へ変更する必要がある。

具体的には、`priority_queue<int>`と記述すると、`priority_queue<int, vector<int>, less<int>>`
になっているので、`priority_queue<int, vector<int>, greater<int>>`へ変更してあげる必要がある。

### A53 - Priority Queue

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ba

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

int Q;
int Query[100009];
int x[100009];
priority_queue<int, vector<int>, greater<int>> S;

int main()
{
	cin >> Q;
	for(int i = 1; i <= Q; i++)
	{
		cin >> Query[i];
		if (Query[i] == 1) cin >> x[i];
	}
	// クエリ処理
	for(int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1) S.push(x[i]);
		if (Query[i] == 2) cout << S.top() << endl;
		if (Query[i] == 3) S.pop();
	}
	return 0;
}
```

## 8.4 連想配列
連想配列はインデックスを自由に指定できる配列、という表現が一番しっくりきた。

### A54 - Map

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bb


```cpp
int Q;
int query[100009];
int score[100009];
string name[100009];
map<string, int> student;

int main()
{
	// 入力
	cin >> Q;
	for (int i = 0; i < Q; i++)
	{
		cin >> query[i];
		if (query[i] == 1)
			cin >> name[i] >> score[i];
		else
			cin >> name[i];
	}
	// mapに代入
	for (int i = 0; i < Q; i++)
	{
		if (query[i] == 1)
			student[name[i]] = score[i];
		else
			cout << student[name[i]] << endl;
	}
	return 0;
}
```

### B54 - Counting Same Values

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ea

```cpp
long long N;
long long A[100009];
map<long long, long long> count_num;

long long	ft_count(long long n)
{
	if (n < 2)
		return 0;
	return (n * (n - 1)) / 2;
}

int main()
{
	// 入力
	cin >> N;
	for (int i = 0; i < N; i++) cin >> A[i];

	// カウント
	for (int i = 0; i < N; i++) count_num[A[i]]++;

	// 合計
	long long ans = 0;
	for (auto& pair : count_num) {
		ans += ft_count(pair.second);
	}

	// 出力
	cout << ans << endl;
	return 0;
}
```

## 8.5 集合の管理
### A55 - Set

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bc

```cpp
int Q;
int Query[100009];
int Card[100009];
set<int> S;

int main()
{
	// 入力
	cin >> Q;
	for (int i = 1; i <= Q; i++)
		cin >> Query[i] >> Card[i];
	for (int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1)
			S.insert(Card[i]);
		if (Query[i] == 2)
			S.erase(Card[i]);
		if (Query[i] == 3)
		{
			auto itr = S.lower_bound(Card[i]);
			if (itr == S.end())
				cout << "-1" << endl;
			else
				cout << (*itr) << endl;
		}
	}
	return 0;
}
```

### B55 - Difference

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_eb


```cpp
int Q;
int Query[100009];
int Card[100009];
set<int> ondesk_card;

int main()
{
	// 入力
	cin >> Q;
	for (int i = 1; i <= Q; i++)
	{
		cin >> Query[i] >> Card[i];
	}
	// クエリ処理
	for (int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1)
		{
			ondesk_card.insert(Card[i]);
		}
		if (Query[i] == 2)
		{
			if (ondesk_card.empty())
				cout << "-1" << endl;
			else
			{	
				auto itr = ondesk_card.lower_bound(Card[i]);
                int minDiff = INT_MAX;
				if (ondesk_card.size() == 1)
					minDiff = abs(Card[i] - *ondesk_card.begin());
                if (itr != ondesk_card.end())
                {
                    minDiff = abs(Card[i] - *itr);
                }
                if (itr != ondesk_card.begin())
                {
                    itr--;
                    minDiff = min(minDiff, abs(Card[i] - *itr));
                }
				cout << minDiff << endl;
			}
		}
	}
}
```
## 8.6 文字列のハッシュ

### A56 - String Hash

ハッシュ値を用いて文字列の比較を行う。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bd

```cpp
long long N, Q;
string S;
long long a[200009];
long long b[200009];
long long c[200009];
long long d[200009];

// ハッシュ値を計算するための変数 
long long mod = INT_MAX;	// なるべく大きな素数
long long T[200009];		// 文字列を数に変換するための箱
long long H[200009];
long long Power100[200009];

// S[l,r]のハッシュ値を返す関数
// あまりの計算に注意！
long long ft_hash(int l, int r)
{
	long long val = H[r] -(H[l - 1] * Power100[r - l + 1] % mod);
	if (val < 0) val += mod;
	return val;
}


int main()
{
	// 入力
	cin >> N >> Q >> S;
	for (int i = 1; i <= Q; i++) cin >> a[i] >> b[i] >> c[i] >> d[i];
	
	// 文字を数値に変換
	for (int i = 1; i <= N; i++) T[i] = (S[i - 1] - 'a') + 1;

	// 100のn乗を前計算しておく
	Power100[0] = 1;
	for (int i = 1; i <= N; i++) Power100[i] = 100LL * Power100[i - 1] % mod;

	// H[1],...,H[N]を計算する
	H[0] = 0;
	for (int i = 1; i <= N; i++) H[i] = (100LL * H[i - 1] + T[i]) % mod;

	// クエリに答える
	for (int i = 1; i <= Q; i++)
	{
		long long hash1 = ft_hash(a[i], b[i]);
		long long hash2 = ft_hash(c[i], d[i]);
		if (hash1 == hash2) cout << "Yes" << endl;
		else cout << "No" << endl;
	}
	return 0;
}
```

### B56 - Palindrome Queries

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ec

```cpp
long long N, Q;
string S;
string S_rev;
long long L[200009];
long long R[200009];

// ハッシュ値を計算するための変数 
long long mod = 1000000007;	// なるべく大きな素数
long long T[200009];		// 文字列を数に変換するための箱
long long T_rev[200009];		// 文字列を数に変換するための箱
long long H[200009];
long long H_rev[200009];
long long Power100[200009];

// S[l,r]のハッシュ値を返す関数
// あまりの計算に注意！
long long ft_hash(int l, int r)
{
	long long val = H[r] -(H[l - 1] * Power100[r - l + 1] % mod);
	if (val < 0) val += mod;
	return val;
}

// 逆から読んだ時のハッシュ計算
long long ft_hash_rev(int l, int r)
{
	long long val = H_rev[r] -(H_rev[l - 1] * Power100[r - l + 1] % mod);
	if (val < 0) val += mod;
	return val;
}

int main()
{
	// 入力
	cin >> N >> Q >> S;
	for (int i = 1; i <= Q; i++) cin >> L[i] >> R[i];
	
	S_rev = S;
	reverse(S_rev.begin(), S_rev.end());

	// 文字を数値に変換
	for (int i = 1; i <= N; i++) T[i] = (S[i - 1] - 'a') + 1;
	for (int i = 1; i <= N; i++) T_rev[i] = (S_rev[i - 1] - 'a') + 1;


	// 100のn乗を前計算しておく
	Power100[0] = 1;
	for (int i = 1; i <= N; i++) Power100[i] = 100LL * Power100[i - 1] % mod;

	// H[1],...,H[N]を計算する
	H[0] = 0;
	for (int i = 1; i <= N; i++) H[i] = (100LL * H[i - 1] + T[i]) % mod;
	// 反転版も計算しておく
	H_rev[0] = 0;
	for (int i = 1; i <= N; i++) H_rev[i] = (100LL * H_rev[i - 1] + T_rev[i]) % mod;
	

	// クエリに答える
	for (int i = 1; i <= Q; i++) {
		long long hash1 = ft_hash(L[i], R[i]);
		long long hash2 = ft_hash_rev(N - R[i] + 1, N - L[i] + 1);
		if (hash1 == hash2) cout << "Yes" << endl;
		else cout << "No" << endl;
	}
	return 0;
}
```

## 8.7 ダブリング

### A57 - Doubling

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_be

```cpp
#include <iostream>

using namespace std;

int N, Q;
int A[100009];
int X[100009];
int Y[100009];
int dp[32][100009];		// 制約が10^9なので2^10=1024より、2^30あれば十分

int main()
{
	// 入力
	cin >> N >> Q;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= Q; i++) cin >> X[i] >> Y[i];
	// 前もって計算しておく
	for (int i = 1; i <= N; i++) dp[0][i] = A[i];
	for (int d = 1; d <= 30; d++)
	{
		for (int i = 1; i <= N; i++) dp[d][i] = dp[d - 1][dp[d - 1][i]];
	}
	// クエリの処理
	for (int i = 1; i <= Q; i++)
	{
		int pos = X[i];
		for (int d = 30; d >= 0; d--)
		{
			if ((Y[i] >> d) & 1) pos = dp[d][pos];
		}
		cout << pos << endl;
	}
	return 0;
}
```

## 8.8 セグメント木：RMQ
### A58 - RMQ (Range Maximum Queries)

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bf

```cpp
class SegmentTree{
	public:
	int dat[300000];
	int siz = 1;

	// 要素datの初期化を行う
	void	init(int N)
	{
		siz = 1;
		while (siz < N) siz *= 2;
		for (int i = 1; i < siz * 2; i++) dat[i] = 0;
	}

	// クエリ1に対する処理
	void update(int pos, int x)
	{
		pos = pos + siz - 1;
		dat[pos] = x;
		while (pos >= 2)
		{
			pos /= 2;
			dat[pos] = max(dat[pos * 2], dat[pos * 2 + 1]);
		}
	}

	// クエリ2に対する処理
	int query(int l, int r, int a, int b, int u)
	{
		if (r <= a || b <= l) return -1000000;
		if (l <= a && b <= r) return dat[u];
		int m = (a + b) / 2;
		int AnswerL = query(l, r, a, m, u * 2);
		int AnswerR = query(l, r, m, b, u * 2 + 1);
		return max(AnswerR, AnswerL);
	}
};

int N, Q;
int Query[100009];
int pos[100009];
int x[100009];
int l[100009];
int r[100009];
SegmentTree Z;

int main()
{
	// 入力
	cin >> N >> Q;
	for (int i = 1; i <= Q; i++)
	{
		cin >> Query[i];
		if (Query[i] == 1) cin >> pos[i] >> x[i];
		if (Query[i] == 2) cin >> l[i] >> r[i];
	}
	// クエリ処理
	Z.init(N);
	for (int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1) Z.update(pos[i], x[i]);
		if (Query[i] == 2)
		{
			int ans = Z.query(l[i], r[i], 1, Z.siz + 1, 1);
			cout << ans << endl;
		}
	}
	return 0;
}
```

### B58 - Jumping

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ee

```cpp
class SegmentTree {
private:
    vector<int> tree;
    int n;

public:
    SegmentTree(int size) {
        n = size;
        tree.resize(4 * n, INT_MAX);
    }

    void update(int idx, int value, int node = 1, int start = 0, int end = -1) {
        if (end == -1) end = n - 1;
        if (start == end) {
            tree[node] = value;
        } else {
            int mid = (start + end) / 2;
            if (idx <= mid) {
                update(idx, value, 2 * node, start, mid);
            } else {
                update(idx, value, 2 * node + 1, mid + 1, end);
            }
            tree[node] = min(tree[2 * node], tree[2 * node + 1]);
        }
    }

    int query(int L, int R, int node = 1, int start = 0, int end = -1) {
        if (end == -1) end = n - 1;
        if (R < start || L > end) return INT_MAX;
        if (L <= start && end <= R) return tree[node];
        int mid = (start + end) / 2;
        return min(query(L, R, 2 * node, start, mid), query(L, R, 2 * node + 1, mid + 1, end));
    }
};

int main() {
    int N, L, R;
    cin >> N >> L >> R;

    vector<int> X(N);
    for (int i = 0; i < N; i++) {
        cin >> X[i];
    }

    vector<int> dp(N, INT_MAX);
    dp[0] = 0;

    SegmentTree seg(N);
    seg.update(0, 0);

    for (int i = 1; i < N; i++) {
        int left = lower_bound(X.begin(), X.end(), X[i] - R) - X.begin();
        int right = upper_bound(X.begin(), X.end(), X[i] - L) - X.begin() - 1;
        if (left <= right) {
            int min_jump = seg.query(left, right);
            if (min_jump != INT_MAX) {
                dp[i] = min_jump + 1;
            }
        }
        seg.update(i, dp[i]);
    }

    cout << dp[N - 1] << endl;

    return 0;
}
```

## 8.9 セグメント木：RSQ
### A59 - RSQ (Range Sum Queries)

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bg

```cpp
class SegmentTree {
private:
    vector<int> tree;
    int n;

public:
    SegmentTree(int size) {
        n = size;
        tree.resize(4 * n, INT_MAX);
    }

    void update(int idx, int value, int node = 1, int start = 0, int end = -1) {
        if (end == -1) end = n - 1;
        if (start == end) {
			// 基底ケース：葉ノードの更新
            tree[node] = value;
        } else {
            int mid = (start + end) / 2;
            if (idx <= mid) {
                update(idx, value, 2 * node, start, mid);		// 左側の子ノードの更新
            } else {
                update(idx, value, 2 * node + 1, mid + 1, end);	// 右側の子ノードの更新
            }
			// 現在のノードの値を更新
            tree[node] = min(tree[2 * node], tree[2 * node + 1]);
        }
    }

    int query(int L, int R, int node = 1, int start = 0, int end = -1) {
        if (end == -1) end = n - 1;
        if (R < start || L > end) return INT_MAX;		// 範囲外へはみだす場合
        if (L <= start && end <= R) return tree[node];	// 完全に範囲外の場合
        int mid = (start + end) / 2;
		// 左右の子ノードに再起的にクエリを行い、その結果の最小値を返す
        return min(query(L, R, 2 * node, start, mid), query(L, R, 2 * node + 1, mid + 1, end));
    }
};


int main() {
	// 入力
	int N, L, R;
    cin >> N >> L >> R;
    vector<int> X(N);
    for (int i = 0; i < N; i++) {
        cin >> X[i];
    }

	// 配列の初期化
    vector<int> dp(N, INT_MAX);
    dp[0] = 0;

    SegmentTree seg(N);
    seg.update(0, 0);

	// セグメント木を用いた動的計画法
    for (int i = 1; i < N; i++) {
        int left = lower_bound(X.begin(), X.end(), X[i] - R) - X.begin();
        int right = upper_bound(X.begin(), X.end(), X[i] - L) - X.begin() - 1;
        if (left <= right) {
			// 範囲内の最小ジャンプ数を取得
            int min_jump = seg.query(left, right);
            if (min_jump != INT_MAX) {
                dp[i] = min_jump + 1;
            }
        }
        seg.update(i, dp[i]);
    }

	// 最終地点までの最小ステップ数を出力
    cout << dp[N - 1] << endl;

    return 0;
}

```

### B59 - Number of Inversions

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ef

```cpp

```

## 8.10 チャレンジ問題
### A60 - Stock Price

情報の取捨選択が大切な問題。もう絶対に起算日にならない！という日をstackから消去していく。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bh

```cpp
int N, A[200009];
int Answer[200009];
stack<pair<int, int>> Level2;

int main() {
	// 入力
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];

	// スタックの変化の再現
	for (int i = 1; i <= N; i++) {
		if (i >= 2) {
			Level2.push(make_pair(i - 1, A[i - 1]));
			while (!Level2.empty()) {
				int kabuka = Level2.top().second;
				if (kabuka <= A[i]) Level2.pop();
				else break;
			}
		}

		// 起算日の特定
		if (!Level2.empty()) Answer[i] = Level2.top().first;
		else Answer[i] = -1;
	}

	// 出力
	for (int i = 1; i <= N; i++) {
		if (i >= 2) cout << " ";
		cout << Answer[i];
	}
	cout << endl;
	return 0;
}
```

# 9章 グラフアルゴリズム 
## 9.0 グラフとは
## コラム5 グラフに関する用語
## 9.1 グラフの実装方法

### A61 - Adjacent Vertices

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bi

```cpp
int N, M;
int A[100009], B[100009];
vector<int> Graph[100009];

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i];
		Graph[A[i]].push_back(B[i]);
		Graph[B[i]].push_back(A[i]);
	}
	for (int i = 1; i <= N; i++)
	{
		cout << i << ": {";
		for (int j = 0; j < Graph[i].size(); j++)
		{
			if (j > 0) cout << ", ";
			cout << Graph[i][j];
		}
		cout << "}" << endl;
	}
	return 0;
}
```

### B61 - Influencer

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_eh

```cpp
int N, M;
int A[100009], B[100009];
vector<int> Friends[100009];

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i];
		Friends[A[i]].push_back(B[i]);
		Friends[B[i]].push_back(A[i]);
	}
	int max_friends = 0;
	int max_friends_student = 0;
	for (int i = 1; i <= N; i++)
	{
		int num_friends = Friends[i].size();
		if (max_friends < num_friends)
		{
			max_friends = num_friends;
			max_friends_student = i;
		}
	}
	cout << max_friends_student << endl;
	return 0;
}
```

## 9.2 深さ優先探索
### A62 - Depth First Search

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_am


```cpp
int N, M;
int A[100009], B[100009];
vector<int> Graph[100009];
bool visited[100009];

void	ft_dfs(int place)
{
	visited[place] = true;
	for (int i = 0; i < Graph[place].size(); i++)
	{
		int new_place = Graph[place][i];
		if (visited[new_place] == false) ft_dfs(new_place);
	}
	return ;
}

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i];
		Graph[A[i]].push_back(B[i]);
		Graph[B[i]].push_back(A[i]);
	}
	// 全ての頂点を未踏に初期化
	for (int i = 1; i <= N; i++) visited[i] = false;
	// 1つ目の頂点から再帰関数による深さ優先探索
	ft_dfs(1);
	// 出力
	for (int i = 1; i <= N; i++)
	{
		if (visited[i] == false)
		{
			cout << "The graph is not connected." << endl;
			return 0;
		}
	}
	cout << "The graph is connected." << endl;
	return 0;
}
```

### B62 - Print a Path

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ei

```cpp
int N, M;
int A[100009], B[100009];
vector<int> Graph[100009];
bool visited[100009];
vector<int> path;

void	ft_dfs(int place)
{
	visited[place] = true;
	path.push_back(place);
	if (place == N) return;
	for (int i = 0; i < Graph[place].size(); i++)
	{
		int new_place = Graph[place][i];
		if (visited[new_place] == false) ft_dfs(new_place);
		if (path.back() == N) return;
	}
	visited[place] = false;
	path.pop_back();
	return ;
}

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i];
		Graph[A[i]].push_back(B[i]);
		Graph[B[i]].push_back(A[i]);
	}
	// 全ての頂点を未踏に初期化
	for (int i = 1; i <= N; i++) visited[i] = false;
	// 1つ目の頂点から再帰関数による深さ優先探索
	ft_dfs(1);
	// 出力
	for (int i = 0; i < path.size(); i++)
	{
		if (i > 0) cout << " ";
		cout << path[i];
	}
	cout << endl;
	return 0;
}
```


https://qiita.com/messhii222/items/4a66e5f0f9ad37b2f18d

## 9.3 幅優先探索
### A63 - Shortest Path 1

https://atcoder.jp/contests/tessoku-book/tasks/math_and_algorithm_an

```cpp
int N, M;
int A[100009], B[100009];
vector<int> Graph[100009];
int dist[100009];
queue<int> Q;

int main()
{
	// 入力
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i];
		Graph[A[i]].push_back(B[i]);
		Graph[B[i]].push_back(A[i]);
	}
	// 初期化
	for (int i = 1; i <= N; i++) dist[i] = -1;
	// 幅優先探索
	Q.push(1);
	dist[1] = 0;
	while (!Q.empty())
	{
		// 先頭の要素を取得して削除
		int pos = Q.front();
		Q.pop();
		// 未確定のノードがあったら現状+1してキューに加える
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next = Graph[pos][i];
			if (dist[next] == -1)
			{
				dist[next] = dist[pos] + 1;
				Q.push(next);
			}
		}
	}
	// 出力
	for (int i = 1; i <= N; i++) cout << dist[i] << endl;
	return 0;
}
```

### B63 - 幅優先探索

https://atcoder.jp/contests/tessoku-book/tasks/abc007_3

```cpp
int R, C, sy, sx, gy, gx;
char board[2509];
int dist[2509];
vector<int> Graph[2509];
queue<int> Q;

int main()
{
	// 入力
	cin >> R >> C >> sy >> sx >> gy >> gx;
	for (int i = 1; i <= C * R; i++) cin >> board[i];
	// 初期化
	for (int i = 1; i <= C * R; i++) dist[i] = -1;
	for (int i = 1; i <= C * R; i++)
	{
		if (board[i] == '.')
		{
			dist[i] = -1;
			if (i % C != 1)
			{
				if (board[i - 1] == '.') Graph[i].push_back(i - 1);
			}
			if (i % C != 0)
			{
				if (board[i + 1] == '.') Graph[i].push_back(i + 1);
			}
			if (i > C)
			{
				if (board[i - C] == '.') Graph[i].push_back(i - C);
			}
			if (i < C * (R - 1))
			{
				if (board[i + C] == '.') Graph[i].push_back(i + C);
			}
		}
	}
	// 幅優先探索
	int start = (sy - 1) * C + sx;
	Q.push(start);
	dist[start] = 0;
	while (!Q.empty())
	{
		// 先頭の要素を取得して削除
		int pos = Q.front();
		Q.pop();
		// 未確定のノードがあったら現状+1してキューに加える
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next = Graph[pos][i];
			if (dist[next] == -1 || dist[next] > dist[pos] + 1)
			{
				dist[next] = dist[pos] + 1;
				Q.push(next);
			}
		}
	}
	cout << dist[(gy - 1) * C + gx] << endl;
	return 0;
}
```

## 9.4 ダイクストラ法

### A64 - Shortest Path 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bl

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

const int INF = 1e9;

int N, M;
int A[100009];
int B[100009];
int C[100009];
vector<pair<int, int>> graph[100009];
int cur[100009];

int main() {
    cin >> N >> M;
    for (int i = 1; i <= M; i++) {
        cin >> A[i] >> B[i] >> C[i];
        graph[A[i]].push_back(make_pair(B[i], C[i]));
        graph[B[i]].push_back(make_pair(A[i], C[i]));
    }

    // 配列の初期化
    for (int i = 1; i <= N; i++) {
        cur[i] = INF;
    }

    // スタート地点をキューに追加
    cur[1] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;
    Q.push(make_pair(cur[1], 1));

    // ダイクストラ法
    while (!Q.empty()) {
        pair<int, int> top = Q.top();
        int pos = top.second;
        Q.pop();

        // cur[pos]の値が更新されている場合は飛ばす
        if (cur[pos] < top.first) continue;

        for (int i = 0; i < graph[pos].size(); i++) {
            int next_place = graph[pos][i].first;
            int next_time = graph[pos][i].second;
            if (cur[next_place] > cur[pos] + next_time) {
                cur[next_place] = cur[pos] + next_time;
                Q.push(make_pair(cur[next_place], next_place));
            }
        }
    }

    // 出力
    for (int i = 1; i <= N; i++) {
        if (cur[i] == INF)
            cout << "-1" << endl;
        else
            cout << cur[i] << endl;
    }

    return 0;
}
```

### B64 - Shortest Path with Restoration

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ek

```cpp
const int INF = 1e9;

int N, M;
int A[100009], B[100009], C[100009];
int cur[100009];
vector<pair<int, int>> Graph[100009];
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> Q;

int main()
{
	// 入力
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i] >> C[i]; 
		Graph[A[i]].push_back(make_pair(B[i], C[i]));
		Graph[B[i]].push_back(make_pair(A[i], C[i]));
	}
	//　初期値の設定
	for (int i = 1; i <= N; i++) cur[i] = INF;
	// スタート
	cur[1] = 0;
	Q.push(make_pair(cur[1], 1));
	// ダイクストラ法
	while (!Q.empty())
	{
		pair<int, int> top = Q.top();
		Q.pop();
		int pos = top.second;
		if (cur[pos] == INF) continue;
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next_place = Graph[pos][i].first;
			int next_time = Graph[pos][i].second;
			if (cur[next_place] > cur[pos] + next_time)
			{
				cur[next_place] = cur[pos] + next_time;
				Q.push(make_pair(cur[next_place], next_place));
			}
		}
	}
	// 経路を記憶
	stack<int> path;
	int pos = N;
	path.push(pos);
	while (pos != 1)
	{
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next_place = Graph[pos][i].first;
			int next_time = Graph[pos][i].second;
			if (cur[pos] == cur[next_place] + next_time)
			{
				path.push(next_place);
				pos = next_place;
				break;
			}
		}
	}
	// 出力
	cout << path.top();
	path.pop();
	while (!path.empty())
	{
		cout << " " << path.top();
		path.pop();
	}
	cout << endl;
}
```

## 9.5 木に対する動的計画法
### A65 - Road to Promotion

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bm

```cpp
int N;
int A[100009];
int dp[100009];
vector<int> Graph[100009];

int main()
{
	cin >> N;
	for (int i = 2; i <= N; i++)
	{
		cin >> A[i];
		Graph[A[i]].push_back(i);
	}
	for (int i = N; i > 0; i--)
	{
		dp[i] = 0;
		for (int j = 0; j < Graph[i].size(); j++) dp[i] += (dp[Graph[i][j]] + 1);
	}
	for (int i = 1; i <= N; i++)
	{
		if (i > 1) cout << " ";
		cout << dp[i];
	}
	cout << endl;
	return 0;
}
```

### B65 - Road to Promotion Hard

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_el

```cpp
int N, T;
int A[100009];
int B[100009];
int dist[100009];
int dp[100009];
vector<int> Graph[100009];
queue<int> Q;
priority_queue<pair<int, int>> Queue;
bool member[100009];

int main()
{
	// 入力
	cin >> N >> T;
	for (int i = 1; i <= N; i++)
	{
		cin >> A[i] >> B[i];
		Graph[A[i]].push_back(B[i]);
		Graph[B[i]].push_back(A[i]);
		dist[i] = -1;
		dp[i] = -1;
		member[i] = false;
	}
	// 幅優先探索でTとの距離distを求める
	Q.push(T);
	dist[T] = 0;
	Queue.push(make_pair(dist[T], T));
	while(!Q.empty())
	{
		int pos = Q.front();
		Q.pop();
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next = Graph[pos][i];
			if (dist[next] == -1)
			{
				dist[next] = dist[pos] + 1;
				Queue.push(make_pair(dist[next], next));
				Q.push(next);
			}
		}
	}
	// 動的計画法
	while (!Queue.empty())
	{
		int i = Queue.top().second;
		Queue.pop();
		dp[i] = 0;
		member[i] = true;
		for (int j = 0; j < Graph[i].size(); j++)
		{
			dp[i] = max(dp[i], dp[Graph[i][j]] + 1);
		}
	}
	// 出力
	for (int i = 1; i <= N; i++)
	{
		if (i > 1) cout << " ";
		cout << dp[i];
	}
	cout << endl;
	return 0;
}
```

## 9.6 Union-Find木
### A66 - Connect Query

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bn

```cpp
#include <iostream>
#include <vector>

using namespace std;

int N, Q;
int Query[100009], U[100009], V[100009];
int par[100009];
int size_root[100009];

int ft_root(int a)
{
	while(true)
	{
		if (par[a] == -1) break;
		a = par[a];
	}
	return a;
}

void	ft_unite(int u, int v)
{
	int u_root = ft_root(u);
	int v_root = ft_root(v);
	if (u_root == v_root) return;
	if (size_root[u_root] < size_root[v_root])
	{
		par[u_root] = v_root;
		size_root[v_root] += size_root[u_root];
	}
	else
	{
		par[v_root] = u_root;
		size_root[u_root] += size_root[v_root];
	}
}

int main()
{
	// 入力
	cin >> N >> Q;
	for (int i = 1; i <= Q; i++) cin >> Query[i] >> U[i] >> V[i];
	// 初期化
	for (int i = 1; i <= N; i++)
	{
		par[i] = -1;
		size_root[i] = 1;
	}
	// クエリの処理
	for (int i = 1; i <= Q; i++)
	{
		if (Query[i] == 1)
		{
			ft_unite(U[i], V[i]);
		}
		if (Query[i] == 2)
		{
			if (ft_root(U[i]) == ft_root(V[i])) cout << "Yes" << endl;
			else cout << "No" << endl;
		}
	}
	return 0;
}
```

### B66 - Typhoon

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_em

```cpp
int N, M, Q;
int A[100009], B[100009];
int Query[100009], U[100009], V[100009];
int par[100009];
int size_root[100009];
bool del_station[100009];
stack<bool> ans;

int ft_root(int a)
{
	while(true)
	{
		if (par[a] == -1) break;
		a = par[a];
	}
	return a;
}

void	ft_unite(int u, int v)
{
	int u_root = ft_root(u);
	int v_root = ft_root(v);
	if (u_root == v_root) return;
	if (size_root[u_root] < size_root[v_root])
	{
		par[u_root] = v_root;
		size_root[v_root] += size_root[u_root];
	}
	else
	{
		par[v_root] = u_root;
		size_root[u_root] += size_root[v_root];
	}
}

int main()
{
	// 入力
	cin >> N >> M;
	for (int i = 1; i <= M; i++) cin >> A[i] >> B[i];
	for (int i = 1; i <= N; i++) del_station[i] = false;
	cin >> Q;
	for (int i = 1; i <= Q; i++)
	{
		cin >> Query[i] >> U[i];
		if (Query[i] == 2) cin >> V[i];
		if (Query[i] == 1) del_station[U[i]] = true;
	}
	// 初期化
	for (int i = 1; i <= N; i++)
	{
		par[i] = -1;
		size_root[i] = 1;
	}
	// 削除されない路線を追加
	for (int i = 1; i <= M; i++)
	{
		if (del_station[i] ==  false) ft_unite(A[i], B[i]);
	}
	// クエリの処理
	for (int i = Q; i > 0; i--)
	{
		if (Query[i] == 1)
		{
			ft_unite(A[U[i]], B[U[i]]);
		}
		if (Query[i] == 2)
		{
			if (ft_root(U[i]) == ft_root(V[i])) ans.push(true);
			else ans.push(false);
		}
	}
	while (!ans.empty())
	{
		if (ans.top() == true) cout << "Yes" << endl;
		else cout << "No" << endl;
		ans.pop();
	}
	return 0;
}
```
## 9.7 最小全域木問題

### A67 - MST (Minimum Spanning Tree)

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bo

```cpp
int N, M;
int A[100009];
int B[100009];
int C[100009];
int par[100009];
int size_root[100009];
vector<pair<int, int>> sides;

void	ft_init()
{
	for (int i = 1; i <= N; i++) par[i] = -1;
	for (int i = 1; i <= N; i++) size_root[i] = 1;
}

int ft_root(int a)
{
	while(true)
	{
		if (par[a] == -1) break;
		a = par[a];
	}
	return a;
}

void	ft_unite(int u, int v)
{
	int u_root = ft_root(u);
	int v_root = ft_root(v);
	if (u_root == v_root) return;
	if (size_root[u_root] < size_root[v_root])
	{
		par[u_root] = v_root;
		size_root[v_root] += size_root[u_root];
	}
	else
	{
		par[v_root] = u_root;
		size_root[u_root] += size_root[v_root];
	}
}

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i] >> C[i];
		sides.push_back(make_pair(C[i], i));
	}
	// 辺が短い順番にソート
	sort(sides.begin(), sides.end());
	// Union-find
	int total_len = 0;
	ft_init();
	for (int i = 0; i < sides.size(); i++)
	{
		int side = sides[i].second;
		if (ft_root(A[side]) != ft_root(B[side]))
		{
			ft_unite(A[side], B[side]);
			total_len += C[side];
		}
	}
	cout << total_len << endl;
	return 0;
}
```

### B67 - Max MST

A67のソートを昇順から降順に変更するだけ。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_en

```cpp
int N, M;
int A[100009];
int B[100009];
int C[100009];
int par[100009];
int size_root[100009];
vector<pair<int, int>> sides;

void	ft_init()
{
	for (int i = 1; i <= N; i++) par[i] = -1;
	for (int i = 1; i <= N; i++) size_root[i] = 1;
}

int ft_root(int a)
{
	while(true)
	{
		if (par[a] == -1) break;
		a = par[a];
	}
	return a;
}

void	ft_unite(int u, int v)
{
	int u_root = ft_root(u);
	int v_root = ft_root(v);
	if (u_root == v_root) return;
	if (size_root[u_root] < size_root[v_root])
	{
		par[u_root] = v_root;
		size_root[v_root] += size_root[u_root];
	}
	else
	{
		par[v_root] = u_root;
		size_root[u_root] += size_root[v_root];
	}
}

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++)
	{
		cin >> A[i] >> B[i] >> C[i];
		sides.push_back(make_pair(C[i], i));
	}
	// 辺が長い順番にソート
	sort(sides.begin(), sides.end(), greater<pair<int, int>>());
	// Union-find
	int total_len = 0;
	ft_init();
	for (int i = 0; i < sides.size(); i++)
	{
		int side = sides[i].second;
		if (ft_root(A[side]) != ft_root(B[side]))
		{
			ft_unite(A[side], B[side]);
			total_len += C[side];
		}
	}
	cout << total_len << endl;
	return 0;
}
```

## 9.8 最大フロー問題
### A68 - Maximum Flow

Ford-Fulkerson法を用いて最大フローを求める問題。

https://qiita.com/0xkei10/items/10253708affb6baf76de

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bp

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Edge{
	int to;
	int cap;
	int rev;
};

class MaximumFlow{
public:
	int size_ = 0;
	bool used[409];
	vector<Edge> Graph[409];

	// 頂点N個の残余グラフの作成o
	void	ft_init(int N)
	{
		size_ = N;
		for (int i = 0; i <= size_; i++) Graph[i].clear();
	}

	// 頂点aからbに向かう、上限c L/sの辺を追加
	void	ft_add_edge(int a, int b, int c)
	{
		int Current_size_a = Graph[a].size();
		int Current_size_b = Graph[b].size();
		Graph[a].push_back(Edge{b, c, Current_size_b});
		Graph[b].push_back(Edge{a, 0, Current_size_a});
	}

	// 深さ優先探索
	// Fはスタートからposに達するまでの残余グラフの辺の要領の最大値
	// 返り値は流したフローの値（流せない場合は0）
	int dfs(int pos, int goal, int F)
	{
		// ゴールに到達：フローを流せる！
		if (pos == goal) return F;
		used[pos] = true;
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			// 容量0の辺は使えない
			if (Graph[pos][i].cap == 0) continue;
			// すでに訪問した頂点に行っても意味がない
			if (used[Graph[pos][i].to] == true) continue;
			// 目的地までのパスを探す
			int flow = dfs(Graph[pos][i].to, goal, min(F, Graph[pos][i].cap));
			// フローを流せる場合、残余グラフの容量をFlowだけ増減させる
			if (flow >= 1)
			{
				Graph[pos][i].cap -= flow;
				Graph[Graph[pos][i].to][Graph[pos][i].rev].cap += flow;
				return flow;
			}
		}
		// 全ての辺を探索しても見つからなかった場合
		return 0;
	}

	// 頂点sから頂点tまでの最大フローの総流量を流す
	int max_flow(int s, int t)
	{
		int total_flow = 0;
		while (true)
		{
			for (int i = 0; i <= size_; i++) used[i] = false;
			int F = dfs(s, t, 1000000000);

			// フローを流せなくなったら操作終了
			if (F == 0) break;
			total_flow += F;
		}
		return total_flow;
	}
};

int N, M;
int A[409];
int B[409];
int C[409];
MaximumFlow Z;

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= M; i++) cin >> A[i] >> B[i] >> C[i];

	// 辺を追加
	Z.ft_init(N);

	for (int i = 1; i <= M; i++) Z.ft_add_edge(A[i], B[i], C[i]);

	// 出力
	cout << Z.max_flow(1, N) << endl;
	return 0;
}
```

### B68 - ALGO Express

この問題は3時間くらい理解にかかった。
「燃やす埋める問題」という問題と同値らしい。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_eo

https://koyumeishi.hatenablog.com/entry/2021/01/14/052223


このzennの記事は最大フロー・最小カット問題から丁寧に説明されている。

https://zenn.dev/kiwamachan/articles/37a2c646f82c7d

```cpp
struct Edge{
	int to;		// 行き先
	int cap;	// 容量
	int rev;	// 逆辺の位置
};

class MaximumFlow{
public:
	int size_ = 0;
	bool used[409];
	vector<Edge> Graph[409];

	// 頂点N個の残余グラフの作成
	// clear()ですべての要素を削除（初期化）
	void	ft_init(int N)
	{
		size_ = N;
		for (int i = 0; i <= size_; i++) Graph[i].clear();
	}

	// 頂点aからbに向かう、上限c L/sの辺を追加
	void	ft_add_edge(int a, int b, int c)
	{
		int Current_size_a = Graph[a].size();
		int Current_size_b = Graph[b].size();
		Graph[a].push_back(Edge{b, c, Current_size_b});	// 向かいの辺の位置は現状の辺の格納数に依存する
		Graph[b].push_back(Edge{a, 0, Current_size_a});	// 逆方向には流量0で設定
	}

	// 深さ優先探索（deep first search）、再帰関数でもある
	// 返り値は流したフローの値（流せない場合は0）
	int dfs(int pos, int goal, int F)	// Fはスタートからposに達するまでの「残余グラフの辺の容量」の最大値
	{	
		if (pos == goal) return F;	// ゴールに到達：フローを流せる！
		used[pos] = true;
		for (int i = 0; i < Graph[pos].size(); i++)	// 現在位置(pos)から出ている辺の数だけ探索
		{
			if (Graph[pos][i].cap == 0) continue;								// 容量0の辺は使えない
			if (used[Graph[pos][i].to] == true) continue;						// すでに訪問した頂点に行っても意味がない
			int flow = dfs(Graph[pos][i].to, goal, min(F, Graph[pos][i].cap));	// 目的地までのパスを探す（次の行き先、ゴール、最小容量の更新）
			if (flow >= 1)														// フローを流せる場合、残余グラフの容量をFlowだけ増減させる
			{
				Graph[pos][i].cap -= flow;
				Graph[Graph[pos][i].to][Graph[pos][i].rev].cap += flow;
				return flow;
			}
		}
		// 全ての辺を探索しても見つからなかった場合
		return 0;
	}

	// 頂点sから頂点tまでの最大フローの総流量を流す
	int max_flow(int s, int t)
	{
		int total_flow = 0;
		while (true)
		{
			for (int i = 0; i <= size_; i++) used[i] = false;	// すべての場所を未訪問にする
			int F = dfs(s, t, 1000000000);
			// フローを流せなくなったら操作終了
			if (F == 0) break;
			total_flow += F;
		}
		return total_flow;
	}
};

int N, M;
int A[409];
int B[409];
int P[409];
MaximumFlow Z;
int total_profit;

int main()
{
	cin >> N >> M;
	total_profit = 150 * N; // 最大の利益がとれたと仮定
	
	for (int i = 1; i <= N; i++) cin >> P[i];
	for (int i = 1; i <= M; i++) cin >> A[i] >> B[i];

	// N個の頂点を準備（スタートをN+1, ゴールをN+2として頂点を追加）
	Z.ft_init(N + 2);
	// M本の辺を追加
	for (int i = 1; i <= N; i++) Z.ft_add_edge(N + 1, i, 150 - P[i]);	// スタートからは「選ぶ」コストを追加
	for (int i = 1; i <= N; i++) Z.ft_add_edge(i, N + 2, 150);		// ゴールへは「選ばない」コストを追加
	for (int i = 1; i <= M; i++) Z.ft_add_edge(B[i], A[i], 2000000);	// A[i]を選んだのにB[i]を選ばなかったら罰金として200ポインお

	// 出力
	total_profit -= Z.max_flow(N + 1, N + 2);
	if (total_profit < 0) cout << 0 << endl;
	else cout << total_profit << endl;
	return 0;
}
```

## 9.9 二部マッチング問題
### A69 - Bipartite Matching

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bq


```cpp
struct Edge{
	int to;		// 行き先
	int cap;	// 容量
	int rev;	// 逆辺の位置
};

class MaximumFlow{
public:
	int size_ = 0;
	bool used[409];
	vector<Edge> Graph[409];

	// 頂点N個の残余グラフの作成
	// clear()ですべての要素を削除（初期化）
	void	ft_init(int N)
	{
		size_ = N;
		for (int i = 0; i <= size_; i++) Graph[i].clear();
	}

	// 頂点aからbに向かう、上限c L/sの辺を追加
	void	ft_add_edge(int a, int b, int c)
	{
		int Current_size_a = Graph[a].size();
		int Current_size_b = Graph[b].size();
		Graph[a].push_back(Edge{b, c, Current_size_b});	// 向かいの辺の位置は現状の辺の格納数に依存する
		Graph[b].push_back(Edge{a, 0, Current_size_a});	// 逆方向には流量0で設定
	}

	// 深さ優先探索（deep first search）、再帰関数でもある
	// 返り値は流したフローの値（流せない場合は0）
	int dfs(int pos, int goal, int F)	// Fはスタートからposに達するまでの「残余グラフの辺の容量」の最大値
	{	
		if (pos == goal) return F;	// ゴールに到達：フローを流せる！
		used[pos] = true;
		for (int i = 0; i < Graph[pos].size(); i++)	// 現在位置(pos)から出ている辺の数だけ探索
		{
			if (Graph[pos][i].cap == 0) continue;								// 容量0の辺は使えない
			if (used[Graph[pos][i].to] == true) continue;						// すでに訪問した頂点に行っても意味がない
			int flow = dfs(Graph[pos][i].to, goal, min(F, Graph[pos][i].cap));	// 目的地までのパスを探す（次の行き先、ゴール、最小容量の更新）
			if (flow >= 1)														// フローを流せる場合、残余グラフの容量をFlowだけ増減させる
			{
				Graph[pos][i].cap -= flow;
				Graph[Graph[pos][i].to][Graph[pos][i].rev].cap += flow;
				return flow;
			}
		}
		// 全ての辺を探索しても見つからなかった場合
		return 0;
	}

	// 頂点sから頂点tまでの最大フローの総流量を流す
	int max_flow(int s, int t)
	{
		int total_flow = 0;
		while (true)
		{
			for (int i = 0; i <= size_; i++) used[i] = false;	// すべての場所を未訪問にする
			int F = dfs(s, t, 1000000000);
			// フローを流せなくなったら操作終了
			if (F == 0) break;
			total_flow += F;
		}
		return total_flow;
	}
};

int		N;
char	C[159][159];
MaximumFlow Z;

int main()
{
	cin >> N;
	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= N; j++) cin >> C[i][j];
	}
	// 頂点を準備
	Z.ft_init(N * 2 + 2);
	for (int i = 1; i <= N; i++)
	{
		for (int j = 1; j <= N; j++)
		{
			if (C[i][j] == '#') Z.ft_add_edge(i, N + j, 1);
		}
	}
	for (int i = 1; i <= N; i++)
	{
		Z.ft_add_edge(N * 2 + 1, i, 1);
		Z.ft_add_edge(N + i, N * 2 + 2, 1);
	}
	// 出力
	cout << Z.max_flow(N * 2 + 1, N * 2 + 2) << endl;
	return 0;
}
```

### B69 - Black Company 2

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_ep

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Edge{
	int to;		// 行き先
	int cap;	// 容量
	int rev;	// 逆辺の位置
};

class MaximumFlow{
public:
	int size_ = 0;
	bool used[409];
	vector<Edge> Graph[409];

	// 頂点N個の残余グラフの作成
	// clear()ですべての要素を削除（初期化）
	void	ft_init(int N)
	{
		size_ = N;
		for (int i = 0; i <= size_; i++) Graph[i].clear();
	}

	// 頂点aからbに向かう、上限c L/sの辺を追加
	void	ft_add_edge(int a, int b, int c)
	{
		int Current_size_a = Graph[a].size();
		int Current_size_b = Graph[b].size();
		Graph[a].push_back(Edge{b, c, Current_size_b});	// 向かいの辺の位置は現状の辺の格納数に依存する
		Graph[b].push_back(Edge{a, 0, Current_size_a});	// 逆方向には流量0で設定
	}

	// 深さ優先探索（deep first search）、再帰関数でもある
	// 返り値は流したフローの値（流せない場合は0）
	int dfs(int pos, int goal, int F)	// Fはスタートからposに達するまでの「残余グラフの辺の容量」の最大値
	{	
		if (pos == goal) return F;	// ゴールに到達：フローを流せる！
		used[pos] = true;
		for (int i = 0; i < Graph[pos].size(); i++)	// 現在位置(pos)から出ている辺の数だけ探索
		{
			if (Graph[pos][i].cap == 0) continue;								// 容量0の辺は使えない
			if (used[Graph[pos][i].to] == true) continue;						// すでに訪問した頂点に行っても意味がない
			int flow = dfs(Graph[pos][i].to, goal, min(F, Graph[pos][i].cap));	// 目的地までのパスを探す（次の行き先、ゴール、最小容量の更新）
			if (flow >= 1)														// フローを流せる場合、残余グラフの容量をFlowだけ増減させる
			{
				Graph[pos][i].cap -= flow;
				Graph[Graph[pos][i].to][Graph[pos][i].rev].cap += flow;
				return flow;
			}
		}
		// 全ての辺を探索しても見つからなかった場合
		return 0;
	}

	// 頂点sから頂点tまでの最大フローの総流量を流す
	int max_flow(int s, int t)
	{
		int total_flow = 0;
		while (true)
		{
			for (int i = 0; i <= size_; i++) used[i] = false;	// すべての場所を未訪問にする
			int F = dfs(s, t, 1000000000);
			// フローを流せなくなったら操作終了
			if (F == 0) break;
			total_flow += F;
		}
		return total_flow;
	}
};

int		N, M;
char	C[159][25];
MaximumFlow Z;

int main()
{
	cin >> N >> M;
	for (int i = 1; i <= N; i++)
	{
		for (int j = 0; j <= 23; j++) cin >> C[i][j];
	}
	// 頂点を準備
	Z.ft_init(N + 26);
	for (int i = 1; i <= N; i++)
	{
		for (int j = 0; j <= 23; j++)
		{
			if (C[i][j] == '1') Z.ft_add_edge(i, N + j + 1, 1);
		}
	}
	for (int i = 1; i <= N; i++) Z.ft_add_edge(N + 25, i, 10);
	for (int i = 1; i <= 24; i++) Z.ft_add_edge(N + i, N + 26, M);
	// 出力
	if (Z.max_flow(N + 25, N +26) == M * 24) cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```

## 9.10 チャレンジ問題

### A70 - Lanterns

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_br

```cpp
int N, M;
int A[19];
int X[109];
int Y[109];
int Z[109];
int dist[1029];
vector<int> Graph[1029];

int	ft_get_next(int pos, int idx)
{
	int lanterns[19];
	// 10進数からランタンの情報を取得
	for (int i = 1; i <= N; i++) lanterns[i] = (pos >> (i - 1)) & 1;
	// ３つのランタンの状態を反転
	lanterns[X[idx]] = 1 - lanterns[X[idx]];
	lanterns[Y[idx]] = 1 - lanterns[Y[idx]];
	lanterns[Z[idx]] = 1 - lanterns[Z[idx]];
	// ランタンの情報を10進数に戻す
	int ret = 0;
	for (int i = 1; i <= N; i++)
	{
		if (lanterns[i] == 1) ret += (1 << (i - 1));
	}
	return ret;
}

int main()
{
	// 入力
	cin >> N >> M;
	for (int i = 1; i <= N; i++) cin >> A[i];
	for (int i = 1; i <= M; i++) cin >> X[i] >> Y[i] >> Z[i];
	
	// グラフに辺を追加
	// max(i << N)個の状態、M個の状態変更の選択肢
	for (int i = 0; i < (1 << N); i++)
	{
		for (int j = 1; j <= M; j++) Graph[i].push_back(ft_get_next(i ,j));
	}

	// スタート地点とゴール地点を決める
	int start = 0;
	// 初期状態のセット
	for (int i = 1; i <= N; i++)
	{
		if (A[i] == 1) start += (1 << (i - 1));
	}
	int goal = (1 << N) - 1;

	// 配列の初期化・スタート地点をキューに入れる
	queue<int> Q;
	for (int i = 0; i < (1 << N); i++) dist[i] = -1;
	dist[start] = 0;
	Q.push(start);

	// 幅優先探索
	while (!Q.empty())
	{
		int pos = Q.front();
		Q.pop();
		for (int i = 0; i < Graph[pos].size(); i++)
		{
			int next_place = Graph[pos][i];
			if (dist[next_place] == -1)
			{
				dist[next_place] = dist[pos] + 1;
				Q.push(next_place);
			}
		}
	}

	//出力
	cout << dist[goal] << endl;
	return 0;
}
```
## コラム6 Bellman-Ford法
## コラム7 Warshall-Floyd法

# 10章 総合問題
## 10.1 総合問題(1)
### A71 - Homework

ついに総合問題突入。
最初の問題は、夏休みN日の間に1日1つ課題をこなしていくが、宿題の難易度と気温の積だけ労力がかかる。必要最小限の労力はいくつか、というもの。

よくよく考えれば、高い難易度の宿題ほど気温が低い時にやった方がいい。
ということで宿題は昇順、気温は降順でソートしてからそれぞれの要素同士の積の和を取ったものが最小の労力になる。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bs

```cpp
int N;

int main()
{
	cin >> N;
	vector<int> A(N);
	vector<int> B(N);
	for (int i = 0; i < N; i++) cin >> A[i];
	for (int i = 0; i < N; i++) cin >> B[i];
	sort(A.begin(), A.end());
	sort(B.begin(), B.end());
	reverse(B.begin(), B.end());
	int total = 0;
	for (int i = 0; i < N; i++) total += A[i] * B[i];
	cout << total << endl;
	return 0;
}
```

## 10.2 総合問題(2)
### A72 - Tile Painting

H*Wのタイルのマスが黒と白で塗り分けられている。

特定の列か行を選び、全て黒で塗る操作をK回行える時、最大でいくつのマスを黒くすることができるか。

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bt

```cpp
int H, W, K;
char tile[19][109];
char tile_test[19][109];

int	ft_score()
{
	int score = 0;
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++)
		{
			if (tile_test[i][j] == '#') score++;
		}
	}
	return score;
}

void ft_paint_column(int remainig_steps)
{
	vector<pair<int, int>> columns;
	for (int j = 1; j <= W; j++)
	{
		int count = 0;
		for (int i = 1; i <= H; i++)
		{
			if (tile_test[i][j] == '.') count++;
		}
		columns.push_back(make_pair(count, j));
	}
	sort(columns.begin(), columns.end());
	reverse(columns.begin(), columns.end());
	// 余っている回数分、白が多い列から塗っていく
	for (int k = 0; k < remainig_steps && k < W; k++)
	{
		for (int i = 1; i <= H; i++) tile_test[i][columns[k].second] = '#';
	}
}

int main()
{
	// 入力
	cin >> H >> W >> K;
	for (int i = 1; i <= H; i++)
	{
		for (int j = 1; j <= W; j++) cin >> tile[i][j];
	}
	// 列の全探索
	int ans = 0;
	for (int i = 0; i < (1 << H); i++)
	{
		int remainig_steps = K;
		for (int i = 1; i <= H; i++)
		{
			for (int j = 1; j <= W; j++) tile_test[i][j] = tile[i][j];
		}
		for (int k = 0; k < H; k++)
		{
			if (remainig_steps < 1) break;
			if ((i >> k) & 1)
			{
				remainig_steps--;
				for (int j = 1; j <= W; j++) tile_test[k + 1][j] = '#';
			}
		}
		if(remainig_steps > 0) ft_paint_column(remainig_steps);
		ans = max(ans, ft_score());
	}
	cout << ans << endl;
	return 0;
}
```

## 10.3 総合問題(3)
### A73 - Marathon Route

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bu

```cpp
```


## 10.4 総合問題(4)
### A74 - Board Game

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bv

```cpp
```

## 10.5 総合問題(5)
### A75 - Examination

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bw

```cpp
```

## 10.6 総合問題(6)
### A76 - River Crossing 

https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_bx

```cpp
```


## 10.7 総合問題(7)
### A77 - Yokan Party（★4）

https://atcoder.jp/contests/tessoku-book/tasks/typical90_a

```cpp
```



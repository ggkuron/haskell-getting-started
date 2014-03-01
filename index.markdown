# Haskell 入門 vol.1 alpha

* author: gg
  + Twitter: [@ggkuron](https://twitter.com/ggkuron)
  + GitHub: [ggkuron](https://github.com/ggkuron)
  + blog: [ggmemo](http://ggkuron.hatenablog.com/)


## Haskell とは

好きなのどうぞ

* C++ より速くて、Perl より簡潔で、Python よりきちんとしていて、Ruby より柔軟で、C# より型が充実していて、Java より頑強で、PHP とは何の共通点もないものって？ に対する解答

* 宇宙語

* 非正格な評価を特徴とする純粋関数型プログラミング言語であり、高階関数や静的多相型付け、定義可能な演算子、例外処理といった多くの言語で採用されている現代的な機能に加え、パターンマッチングやカ

* 楽しい

* 型が全て


## お断り

こんなん、一回でやり切れるはずなかったんや。。
主にスライドががが。。
ゆっくりしていきましょう。

![](http://blog-imgs-56.fc2.com/k/o/d/kodoku21/1390991174277.jpg)

[いざとなればこれ使わせてもらいます](http://d.hatena.ne.jp/ruicc/20100131/1264905896)


## Haskell Platform

Haskellの全部入り開発環境パッケージ

とりあえず、これ入れよう。

* プログラムを書いて実行



## How to install haskell-platform

Ubuntu

```sh
sudo aptitude install haskell-platform
```

Fedora

```sh
sudo yum install haskell-platform
```

他のディストリビューションでも  


## How to install haskell-platform

Windows or Mac

<http://hackage.haskell.org/platform/>

OSXの場合、brewやportのhaskell-platformは古い可能性があるので上記推奨


### エディタ問題

* windowsでのEditor選択
    + <http://www.haskell.org/haskellwiki/Windows#Editors>
* Mac でのEditor選択
    + <http://www.haskell.org/haskellwiki/Mac_OS_X#Editors_with_Haskell_support>  

* Eclipseにもpluginあるみたい

* ブラウザ上から利用できるIDEとかもあります。[FPComplete](https://www.fpcomplete.com/)


## Haskell とは

遅延評価を特徴とする純粋関数型言語です。

ともかく使ってみよう


## ghc

Glasgow Haskell Compiler

使い方としては3通り

* ghc
    + ネイティブコードを生成するコンパイラ
* ghci
    + 対話型インタプリタ
* runghc
    + Haskellプログラムをスクリプトとして走らせるプログラム
  


## 対話インタプリタで遊ぶ

#### ghci を使う
シェルやコマンドプロンプトやPowerShellで

```sh
ghci
```

で起動する~はず~
ghciが起動すると最初にPrelude.hsという標準ライブラリが読み込まれる。


## 実際に書く

ghci で

```haskell
> 1 + 2 * 3
```

と打ってみよう

```sh
7
```

が返ってくる
結合順も

```haskell
1 + (2 * 3)
```

になってますね？


色々、試してみましょう

```haskell
> True && True
True
> False || True
True
> not False
True
> 5 == 5
True
> 5 /= 5
False
> "hello" == "hello"
True
```

他の言語を触ったことのある人には違和感のない結果だと思います。  
≠が`/=`が変わってるくらいかな。


## ghciでの変数定義

```haskell
> let x = 10
```

これで `x` という名前に `10` が束縛された

```haskell
> x -- Enter
10
```

これで `x` 自体が評価される

Haskellでの変数は一回定義した後は不変。



だけど、ghciでもう一度定義し直すと

```haskell
> let x = 20
x -- Enter
20
```

上書きされている?  
  
これは、前に定義されたxが隠されて新しいxが見えている。  


この状態で、

```haskell
 > x + 20 
 40
```

これは `20 + 20` と同等である



## ファイルに記述して実行

`hello.hs` を用意して

```haskell
double x = x + x
```

とでも書いておく

数値xを引数に取って、`x+x`を結果を返すdoubleという関数です。


### ghci からファイルを読み込み

```ghci
:l hello.hs
```

とすれば OK

```haskell
> double 3 -- Enter
6
```

になったはず


## コメント

`--` から行末まではコメントです  
プログラムの実行には影響を与えません。


## Haskellの特徴である純粋性について

### 純粋関数とは

副作用を持たない関数のこと。
ある関数を同じ引数で呼び出したら、同じ値が返ってくることが保証されるのが純粋関数。


### 副作用とは

関数は引数を適用してなにか返り値を得ることが目的。

* その返り値のことを主作用。
* それ以外の主作用以外の作用によって得られるもののことを副作用という。
  
副作用は複雑さをもたらすので、それを回避することはバグの低減に繋がる。
  


先ほど定義した関数`double`は純粋な関数。  

```haskell
-- 再掲
double x = x + x
```

何回`double 3`しても同じ結果が返ってきますね？



文字列をコンソールに出力する関数`putStrLn`だと

```haskell
putStrLn "文字列"
```

これは副作用が目的の関数。  

コンソールに文字を表示するように命令するけど、  
それが成功するかどうかはHaskellプログラムの関知するところじゃない。
  
Haskellは、この関数について情報を得られないので返り値は意味を持たない。
  


じゃあ、副作用が存在するじゃん。何が純粋関数型だ！という事ですが、  
この副作用を分離することで、純粋関数を有効に用いたプログラムが書ける。  
だから、Haskellは純粋関数型言語。~ということだと思ってる。~

Haskellの仕様には副作用についての記述はなく,それを切り離してるからHaskellは紛れもなく純粋関数型言語だ！！
というご指摘いただきました。ありがとうございました。


### 正格評価と遅延評価

遅延評価を用いる関数型言語の標準となるべくHaskellは作られました。

関数を適用するときに引数をどのように評価するかのことを評価戦略といいます。

それの一種が遅延評価です。


hello.hsに

```haskell
square x = x * x
```

という関数を追加で定義してみる。

ghciで

```ghci
> :r
```

で前回読み込んだファイルを再読み込みできる。
もしくは、

```ghci
> :l hello.hs
```


#### 正格評価

引数を先に評価する戦略。  
多くの言語で使われている。

```haskell
square (3 + 5)
square 8
8 * 8 
64
```

引数に渡された式が先に評価される。
最内簡約とも呼ばれる。



#### 遅延評価

引数の評価をできるだけ後伸ばしにする評価戦略。

```haskell
square (3 + 5)
(3 + 5) * (3 + 5)
8 * 8 
64
```

最外簡約

どちらにしても結果は変わらない。  
関数が停止可能なとき、遅延評価(名前呼び出し）では必ず停止するという性質がある。


## 式と値,そして型

Haskellでは、計算できる要素のことを式(expression)と呼ぶ。
Haskellではもうすべてが式、文(statement)~のようにみえるもの~でさえ式。

```haskell
((1 + 2) * 3) + 4)
```

* `1+2`は式、
* `(1+2)*3`は式、
* そして全体 `((1 + 2) * 3) + 4)`も式

* さらに、`+`も、、式


その式の答え、計算の結果のことを値(value)と呼ぶ。
これ以上計算しようのない式のこと。

* 数字の`42`
* 文字`'a'`は値、

値であるなら式でもある


## 型

すべての式は何らかの型に結び付けられている。
だからもちろん全ての値はそれぞれの型をもつってわけ。


### Haskellでの型の表記

```haskell
42       :: Int
'a'      :: Char
"string" :: String
"a"      :: String
[1,2,3]  :: [Int]
(1,'a')  :: (Int,Char)
```

この表記方法のことを型シグネチャとか型注釈とか言います。

* `::`より左側は値の世界
* `::`より右側は型の世界

全くの別世界。同じ記号とか使ってても別物。


## 型の確認

ghciで`:t`に続けて式を書くと型を教えてくれる。

```haskell
> :t 1 + 2
1+2 :: Num a => a
```

数値型であることが確認できる。  
この`=>`とか`a`はあとで説明します！！  
よろしくお願いします。



### haskellの基本的な型

* Haskellの型名は大文字から始まる。型の世界。
* 変数名や関数名は小文字から始まる。値の世界。
    + 例外あり。


#### 基本型page1
* Int
    + CPUによって32bitとか64bitとか。
* Integer
    + 半端無く大きい整数をつかうときにどぞ。
* Float
    + 単精度不動小数点数
* Double
    + 倍精度浮動小数点数


#### 基本型page2
* Char
    + Unicode文字を表す
    + 'a'
    + 'あ'
* Bool
    + False
    + True のどちらかの値を持つ型
* ()
    + 有効な値がないことを示す型
    + `()`という値だけを持つ型
    + Unitと読む


### Unit

有効な値がないことを示す型。
`()`という値だけを持つ

```haskell
() :: ()
```

値の世界と型の世界は別。
適切に定義された関数では、この型は副作用を持つ関数にしかあらわれない。


### 関数の型

すべての式が型を持つって言ったよね？
じゃあ`+`の型は何なんだろう？

入力として`T1`という型を取って出力として`T2`という型を返す関数の型は、

```haskell
T1 -> T2
```

と表現される。
だからIntを取ってIntを返す関数の型は

```haskell
Int -> Int
```

`+`の型を確認してみてください。

```haskell
> :t (+)
```


## リスト

Haskellのリストは単一の型の要素だけを持てる。

* 文字のリスト
* Intのリスト
* Floatのリスト

など。決して 混ぜられない。

```haskell
[1, 2, 3, 4 ]  -- :: Num t => [t]
['a','b','c']  -- :: [Char]
[]             -- :: [a] 
```

```haskell
[1..4]
```

でも1から4までのリストを表現できる。

```haskell
['a'..'z']
```



## タプル

違う型でも一緒に括れるデータ構造、それがタプル。
括る数、括る型でそれぞれの型も異なったものとなる。

```haskell
(1, "abc")  -- :: Num t => (t, [Char])
("abc", 1)  -- :: Num t => ([Cahr], t)
("abc",'a') -- :: ([Char], Char)
(1,2,3,4,5) -- :: (Num t, Num t1, Num t2, ..) => (t,t1,t2,t3,t4)
[1,2,3,4,5] -- :: Num t => [t]
```

これが全部、違う型



```haskell
(1, "abc")  -- :: (数,文字列)
("abc", 1)  -- :: (文字列,数）
("abc",'a') -- :: (文字列,文字)
(1,2,3)     -- :: (数,数,数)
(1,2,3,4,5) -- :: (数,数,数,数,数)
[1,2,3,4,5] -- :: [数]
```

全部、違う型



## リスト 関数

```haskell
head [1,2,3,4] --  1
head "abc"     --  'a'
```

これでリストの先頭の `1` が取れる

```haskell
tail [1,2,3,4] --  [2,3,4]
tail "abc"     --  "bc"
```
これでリストの先頭以外の残り `[2,3,4]` が取れる
尻尾というか本体みたいな感じある

```haskell
last [1..4] --  4
init [1..4] --  [1,2,3]
```


## (:) cons

```haskell
1 : [2 3 4]   --  [1,2,3,4]
(:) 1 [2 3 4] --  [1,2,3,4]
```

1 つの要素とリストを連結して新しいリストを作る
Haskellのcons


これは中置演算子として定義されている。
左辺が第1引数、右辺が第2引数となる2引数を取る関数。

`+`とか`*`とかと同じですね。


### 横道

Haskellでは、
中置演算子として定義されている関数は、括弧`()`で囲むことで通常の関数として使える。

```haskell
1 + 2 -- 3
(+) 1 2 -- 3
((+) 1 2) -- 3
((+) ((*) 3 5) 2) -- 17
```

逆に、2引数を取る関数があると、それをバッククォートで囲むことで中置演算子として使える。

```haskell
f x y = x + y
f 1 2   -- 3
1 `f` 2 -- 3
```


## リストの正体

```haskell
[1,2,3,4]
```

というリストは、実は

```haskell
1 :(2 :(3 :(4 :[])))
```

という形になっている


## リスト連結

```haskell
[1,2,3] ++ [3,4,5]
[1,2,3,3,4,5]
```

リストの連結は`(++)`で行える。
が、この操作は前方のリストを最初から最後まで走査するコストがかかるので
使えるかぎり`(:)`を使うといい。



## 関数定義

```haskell
f :: Int -> Int -- 型の世界(型注釈)
f x  = x + 1    -- 値の世界
```

これで引数 `x` に `1` を足す関数 `f` が定義された。  
Intを取ってIntを返す関数という型注釈をつけてみた。  

```haskell
f 2 -- > 3
f 2.0 -- > 型エラー
```

これはIntを取ってIntを返す関数として定義されたので、
浮動小数点数は受け付けない。
型で弾く(安心)


型注釈をつけなくてもコンパイラは賢いので推論してくれる。  
だけど、人間はコンパイラほど賢くないのでコンパイラに意図を伝えておくために
極力型注釈は付けるようにすべき。


### 型クラス

```haskell
Num a => a
```

とか

```haskell
Fractional a => a
```

とかの表記についてです。  

型クラスとは、振る舞いを定義するインタフェース。  
ある型クラスのインスタンスであるということは、要求される関数を定義している型であるということ。


### 型変数

ここでの`a`は実際の何らかの型を表す変数、型変数。
多相的に型を扱うために使われる。



リストは単一の型だけを保持できるデータ構造でした。  
リストは保持している型によってそれぞれの型が違う。  

```haskell
[1,2,3,4,5]   :: Num t => [t]
['a','b','c'] :: [Char]
[1.0,2.0,3.0] :: Fractional t => [t]
```

例えば、`head`はリスト先頭の要素を返す関数だったけど、  
関数は式で、式は型に結び付けられている。  
1つの`head`という関数で全てのリストに対する定義するためには型変数がいい働きをしてくれる。



```ghci
:t head
head :: [a] -> a
```

これはどんな型でもいいから`a`という型を包んだリストを取ってその包まれた`a`という型を返す関数

型変数としては`a`とか`b`とか`c`とかがよく使われる。



もうひとつリスト連結関数`(++)`を見ておきましょう。

```haskell
[1..5] ++ ['a','b','c'] -- だめ。
> :t (++)
(++) :: [a] -> [a] -> [a]
```

aは実際の何らかの型を表す。

* 第1引数の`a`は`Num t => t`
* 第2引数の`a`は`Char`

になって、型があわない。



```haskell
Num a => a
```

```haskell
Fractional a => a
```

`=>`の左側を型制約と呼ぶ。

```haskell
:t 30 :: Num a => a
```

型変数`a`は`Num`型クラスのインスタンスである、という制約を加えている。  
インスタンスになるためにはそのクラスが既定している関数を実装する必要がある。  
この制約によって、Num型クラスが定義を要求する関数を`a`が備えた型だということをコンパイラに伝えられる。


### Num 型クラス

`Num`は数の型クラス

これのインスタンスになるためには以下を要求する 

* `(+) :: a -> a -> a` 
* `(*) :: a -> a -> a` 
* `(-) :: a -> a -> a`
* `negate :: a -> a`
* `abs :: a -> a`



* `Int`
* `Integer`
* `Float` 
* `Double`
などがこのインスタンス

今まで見てきたように  
数字リテラル`1`とかは多相定数としてこのNumクラスの任意のインスタンスとして振る舞う

```ghci
:t 20
20 :: Num t => t
```


### Fractional 型クラス

`Fractional`は分数の型クラス

+ `/`
+ `recip`

を要求する
`Float`,`Double`がこのインスタンス



### Eq 型クラス

等値比較のための型クラス
Haskellの基本型は全てこの型クラスのインスタンス。

* `==`
* `/=`


各型クラスの要件はghciで

```ghci
:i Num
```

のようにして知ることができる。



## 無名関数(λ関数)

λ式を使って無名関数を作れる。

```haskell
\x -> x + 1
```

名前の無い関数。  
目を細めれば`\`はλに みえてきた…？


## どうやって呼び出す

もしもその関数に `f` という名前が付いていれば

```haskell
f 1
```

で良い

ならば

```haskell
(\x -> x + 1) 1 -- > 2
```

で良い



## 条件分岐

```haskell
if True then 1 else  2 -- > 1
```

elseは必須

```haskell
if False then 3 else 4 -- > 4
```

```haskell
if 1 == 2 then 10 else 20 -- > 20
```

### 完全な余談

if-then-elseは遅延評価のHaskellでは関数で問題なく定義できる。
だけどHaskellのifはcase式へのdesugarになってる。
理由は


## パターンマッチ

関数の本体を引数の形で場合分けできる。
数値、文字、リスト、タプル、Boolなど多くの型で使える。

```haskell
isThree :: Int -> Bool
isThree 3 = True
isThree _ = False
```

```haskell
factorial :: Int -> Int
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

```haskell
head' x:xs = x
head' []   = []
```

どれにも合わないパターンがあると例外が飛ぶ 。
オプションつければghcが叱ってくれる。



## ガード

引数の形が同一でもさらに条件を絞って場合分けできる。

```haskell
max' :: (Ord a) => a -> a -> a
max' a b  
    | a <= b    = b
    | otherwise = a
```

`=`の左側が`Bool`型の値となる式です。
`otherwise`って`True`です。
上側から順に合致するまで下っていきます。
合致したところのみ関数本体が評価されます。


## 空リストに配列操作関数を使うと

```haskell
head [] -- > Exception! 
tail [] -- > Exception!
init [] -- > Exception!
last [] -- > Exception! 
```

全域関数にはなっていない。
では、これを全域関数で定義し直しましょう。

型のことがわかったので、同一の型となるようにそれぞれ定義してみましょう。

```haskell
tail' x:xs = xs
tail' [] = []
```

これはパターンマッチの練習に。


## リストの直接 n 番目の要素が欲しい

こういう関数が欲しい。ていうかあったけど忘れた。
…というときの探し方。
[hoogle](http://haskell.org/hoogle/)にアクセス。
欲しい関数の型は

```haskell
[a] -> Int -> a
```

に類するような型

まさにこれな`(!!)` という関数がある
これは中置演算子とし定義されているので

```haskell
[10,20,30,40] !! 2 -- > 30
```

これでOK!



## リスト操作の種々の関数


### map

全要素に特定の関数を適用した結果のリストを得る

```haskell
map (\x -> x * 2) [1,2,3] -- > [2,4,6]
```


### foldr

全要素を特定の関数で右から畳み込む

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
```

```haskell
foldr (+) 0 [1,2,3] -- > 6
```

もともと渡したリストは

```haskell
[1,2,3]
(1:(2:(3:([]))))
```

この`:`を引数として渡した2引数を取る関数に  
`[]`を引数として渡した初期値に置き換える

```haskell
(1+(2+(3+(0))))
```

これが`foldr`


### foldl

```haskell
foldl (+) 0 [1,2,3] -- > 6
((((0)+1)+2)+3)
```

これが左畳み込み  
`foldr`を使うべきか`foldl`を使うべきかは処理の効率を考えて

これは、また今度


## foldr

```haskell
foldr (\x y -> x * 10 + y) 0 [1,2,3]
```

これは

```haskell
x # y = x * 10 + y
```

を

```haskell
(1:(2:(3:([])))) 
```

に対して適用させることなので、

```haskell
(1 #(2 #(3 #(0)))) 
```

と等価なので `60` となる


```haskell
foldl (#) 0 [1,2,3]
```

だと

```haskell
(((0 # 1)# 2)# 3)
```

と等価なので `123`になる。



## 無限リスト

遅延評価の仕組みのおかげで無限長のリストを扱える

```haskell
repeat 1 -- > [1,1,1,1,1,1,1 ...)
```

このままでは ghciが永遠出力し続ける (Ctrl + C で中断可能)


### repeat

与えられた引数を無限に要素に持つリストを返す

```haskell
repeat 10 -- > [10,10,10, ...]
```


### cycle

与えられた引数のリストを繰り返す無限長リストを返す

```haskell
cycle [1,2,3] -- > [1,2,3,1,2,3,1,2,3 ...]
```


### iterate

与えられた関数を繰り返し適用して  
得られる要素が並ぶリストを返す

```haskell
iterate (/x -> x * 10) 1 -- > [1,10,100,1000,10000 ...]

iterate (/x -> + x 1) 0 -- > [0,1,2,3,4 ...]
```


## 無限リストから有限の要素を取り出す

### take

```haskell
take 5 $ repeat 1 -- > [1,1,1,1,1]

take 10 $ cycle [3 2 1] -- > [3,2,1,3,2,1,3,2,1,3]
```

もちろん `take` は有限長のリストにも使える
`$`は開き括弧と無限遠方にある閉じ括弧のペアだと思うといい。

どのように定義されているかを見たければhoogleへ！

```haskell
take 5 $ [1,2,3,4,5,6,7] -- > [1,2,3,4,5]
```



### 無限リストを畳み込む

```haskell
and' :: [Bool] -> Bool
and' xs = foldr (&&) True xs
```

例えば

```haskell
[True,True,False,True]
```

を与えてみると,このリストは表記を変えると

```haskell
True :(True:(False:(True:([]))))
```
なので定石通り,foldrを適用して

```haskell
True (&&)(True (&&)(False(&&)(True(&&)(True))))
True (&&)(True (&&)(False(&&)(True && True)))
True (&&)(True (&&)(False && True))
True (&&)(True && False)
True && False
False
```

となる

これを無限リストに適応すると

```haskell
and' $ repeat False
-- and' $ [False, False, False ...]
-- False (&&)(False (&&)(False ...
False
```

`&&`は引数1つ目がFalseなら後ろを評価せず停止するように定義されているんです。

```haskell
(&&) :: Bool -> Bool -> Bool
True && True = True
_    && _    = False
```

遅延評価だからそこで停止するんです。
正格評価だと引数の評価をすべて先にしにいくので止まりません。



## zipWith

関数を使って2つのリストを1つにする

```haskell
zipWith :: (a -> b -> c) -> [a] -> [b] -> [c]
```

```haskell
zipWith (+) [1..9] [9,8..1]
```


## filter

全要素を特定の関数でフィルタリングして  
残ったもののみで構成されたリストを得る

```haskell
:t filter
filter :: (a -> Bool) -> [a] -> [a]
```

```haskell
filter (\x -> x >= 5) [1..9] -- > [5,6,7,8,9]
```


## 文字列

```haskell
"abc" -- :: [Char]
""    -- :: [Char]
[]    -- :: [a]
```

```haskell
> :t "abc" 
"abc" :: [Char]
> :t 'a'
'a' :: Char
```

```haskell
"abc" ++ "def" -- > "abcdef"
```

`Int` から`Char`を作る関数が欲しいなら
その型は`Int -> Char`なので
hoogleで調べる。
`Char -> Int`もついでに。

```haskell
intToDigit 1 -- > '1'
```


## Hello World!

```haskell
putStrLn "Hello World!"
```

`println` は文字列を行として標準出力に表示する関数

```ghci
:t putStrLn
putStrLn :: String -> IO ()
```

### IO ()
Unitの結果をもたらすAction型

## IO 
モナドと呼ばれる型クラスのインスタンス。
モナド。恐ろしい名前

しかし、ただの型クラスだ。



## 連想リスト

Data.Mapに定義されている
一般的には連想リストと呼ばれてるものだけど、Haskellのリストは使われてないので、
Mapと呼ぶこと。
自前でリストで定義するより高速になるように作られている。
PreludeやData.Listと競合する名前が使われているので修飾インポートする

```haskell
import qualified Data.Map as Map
```

```haskell
number = Map.fromList [(1,"one"), (2,"two"), (3, "three")]
Map.lookup number 1 -- "one"
```

上記の例は `1` というキーの値が `"one"`，  
`2` というキーの値が `"two"` のマップである．



## 今回は説明できなかったこと

* Monad
* 


## プロジェクト作成

ディレクトリを作って

```sh
mkdir appdir
```

cabalファイルを作って

```sh
vim app.cabal
```

```cabal
Name:   MyApp
Version: 0.0
Description: 
Build-Type: Simple
Cabal-Version:  >=1.2
Executable MyAppExecutable
  Main-is:    main.hs
  Build-Depends: base >= 2.0
               , free-game >= 1.0
```


## 実行可能ファイル作成

```sh
cabal setup
cabal build
```



`main` という関数が main 関数
(Moduleを記述するとMainというモジュールのmain関数を探しに行く)
ここから起動する


## 引数を受け取ってみる

```haskell
module Main where
import System.Environment (getArgs)

main :: IO ()
main = do
    args <- getArgs
    putStrLn ("Hello, " ++ args !! 0)
```

### 一番上の二行
- Mainという名前のモジュールをこれから書く
- それは、Systemモジュールの一部をimportする。    

Haskellの全てのプログラムは
Mainモジュールのmainアクションから実行開始する。

module名は一文字目大文字
定義名は一文字目小文字

#### IO ()
ユニットの結果をもたらすAction型
## IO 型
モナドと呼ばれる型クラスのインスタンス。
モナド。恐ろしい名前
次のように改造

## 実行してみる

```sh
./MyAppExecutable
```

```txt
Hello, World!
```

ビルド

```sh
cabal build
```

## 引数を与えて実行

```sh
./a.out World!
```

```sh
Hello, World!
```

## cabal 

Common Build And Tool

Haskell用のビルド、パッケージングシステム。
Haskellのpackage manager
gemとかに類するもの
欲しいパッケージがあればこれを使って簡単に導入できる。


## 参考書籍 1

[すごいHaskellたのしく学ぼう
![](http://ssl.ohmsha.co.jp/imgm/978-4-274-06885-0.gif)
](http://www.amazon.co.jp/dp/4274068854)


## 参考書籍 2

[プログラミングHaskell
![](http://ssl.ohmsha.co.jp/imgm/978-4-274-06781-5.gif)
](http://www.amazon.co.jp/dp/4274067815)


## 参考書籍 3
[実践F# 関数型プログラミング入門
![](https://gihyo.jp/assets/images/cover/2011/9784774145167.jpg)
](http://www.amazon.co.jp/dp/4774145165)


## 参考資料
[The Haskell School of Music](http://www.cs.yale.edu/homes/hudak/Papers/HSoM.pdf)


## 勉強に使えるサイト

* [FP Complete](https://www.fpcomplete.com/school)



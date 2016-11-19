# 複数のggplot2要素を関数の戻り値で渡したい

## Q

複数のggplot2の要素を加えたいのですが、以下のように手動でやるとうまくいきます:

```r
require(ggplot2)
```

```
## Loading required package: ggplot2
```

```r
p <- ggplot(aes(x = mpg, y = hp), data = mtcars)
p + geom_vline(xintercept = 20) + geom_point(data = mtcars)
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-1-1.png)<!-- -->

でも、これ関数に持たせようと思って以下のようにしてみるとエラーがでます:

```r
myFunction <- function() {
  return(
    geom_vline(xintercept = 20) + geom_point(data = mtcars)
  )
}
p <- ggplot(aes(x = mpg, y = hp), data = mtcars)
# p + myFunction()
# ここで以下のエラーがでます:
# "エラー: 二項演算子の引数が数値ではありません"
```

ggplot2に関数の戻り値で複数の要素を放り込むにはどうしたらいいでしょうか?

## A

ggplot2は要素の「リスト」をサポートしています:

```r
myFunction <- function()
 list(geom_vline(xintercept = 20),
      geom_point(data = mtcars))

p <- ggplot(aes(x = mpg, y = hp), data = mtcars)
p + myFunction()
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

このように各要素をリストとしてまとめ、"+"で追加するとうまく組み込まれます。このgeom_*以外にも、"+"で束ねることができるものであれば同様にlistとして渡すことができます。

またこれを利用すれば、事前にggplot2の要素をまとめておいてデータセットだけ切り替えるなど、非常に応用がききます。

## 応用

### 要素のリストを使いまわす
「ggplot2は要素のリストをサポートする」ことを応用すれば、以下のようなことができます:


```r
lay = list(stat_summary(fun.y = mean, geom = "line"),
           stat_summary(fun.data = mean_se))

ggplot(mtcars, aes(am, mpg, colour = factor(vs), group=factor(vs))) + lay
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->

```r
ggplot(iris, aes(Species, Sepal.Length, group=1)) + lay
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-4-2.png)<!-- -->

このコードでは、「折れ線グラフでxの各点における標準誤差も描く」というのを繰り返し利用するため、それらを表現するggplot2要素をlayというlistに束ねています。こうすることで、"+ lay"と追加することで簡単に描くことができます。

### 繰り返しを関数にして、lapplyでリストで受け取る
また、繰り返し処理を行いたいときは、lapplyで記述することも可能です:


```r
p <- ggplot(iris, aes(y=Sepal.Width, x=Species)) +
  stat_summary(fun.y=mean, geom = "bar") + 
  ylim(0,6) + 
  lapply(1:3, function(i) geom_segment(x=i-0.4, y=7-i, xend=i+0.4, yend=7-i))
p
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->

繰り返し処理をしたい場合、`for(i in 1:3)`などで実行するとうまくいきません。ですが、繰り返したい処理を関数にしてlapplyで実行するようにすれば、返り値はlistになるのでちゃんと要素の内容が当たるようになります。

### mapplyで複数の引数がある関数を繰り返す
もし複数の引数が必要ならば、mapplyを利用する方法もあります:


```r
p = ggplot(data.frame(x = c(-10, 10)), aes(x)) +
  mapply(function(m, s, co) stat_function(fun = dnorm, args = list(mean = m, sd = s), colour = co), 
         -1:1, c(0.5, 1, 1.5), rainbow(3))
p
```

![](07-ggplot2_element_function_files/figure-epub3/unnamed-chunk-6-1.png)<!-- -->

mapplyは第一引数の関数に対し、それ以降の引数を当てて実行し、その結果をlistで返してきます。なので上の例のように、パラメータを変化させながら重ね書きする時などはきっと重宝するでしょう。

これらはほんの一例ですので、ぜひ色々試してみてください。


## 参考資料

この記事は、以下のstackoverflowの内容を参考に書き起こしました:

- [r - How can I combine multiple ggplot2 elements into the return of a function? - Stack Overflow](http://stackoverflow.com/questions/4835332/how-can-i-combine-multiple-ggplot2-elements-into-the-return-of-a-function)

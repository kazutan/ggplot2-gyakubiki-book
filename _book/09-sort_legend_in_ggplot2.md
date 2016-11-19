# ggplot2で凡例を並び替えたい

## Q1
`ggplot2`を使って積み重ねの縦棒グラフを描くと、このようになります:


```r
require(ggplot2)
```

```
## Loading required package: ggplot2
```

```r
p <- ggplot(diamonds, aes(cut, fill = clarity)) + 
  geom_bar()
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-1-1.png)<!-- -->

しかし、これだと積み重ねの順番と凡例の順番が逆になります。凡例を逆順にしたいのですがどうすればいいでしょうか?

## A1
`guide_legend(reverse = TRUE)`を活用すると逆順になります:

```r
p <- ggplot(diamonds, aes(cut, fill = clarity)) + 
  geom_bar() +
  guides(fill = guide_legend(reverse = TRUE))
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-2-1.png)<!-- -->

この`guides(fill = guide_legend(reverse = TRUE))`によって、`aes(fill = )`で指定したfactorの順番が逆順となります。`aes(colour = )`で指定したfactorについては、以下のようにしてください:

```r
p <- ggplot(diamonds, aes(cut, colour = clarity)) + 
  geom_bar(fill = "white") +
  guides(colour = guide_legend(reverse = TRUE))
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

これらは棒グラフ以外でも有効です。ただし、凡例(legend)内の順序が反転するだけです。

## Q2
`ggplot2`で、「色と形」というように凡例に表示させるものが2種類以上ある場合、その変数の順番は変更できるのでしょうか。例えば以下のような場合です:

```r
p <- ggplot(diamonds, aes(carat, price, colour = clarity, shape = cut)) +
  geom_point() +
  theme(legend.position = "top")
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->

このとき、オプションを指定する順番を入れ替えたり、`shape`と`colour`で指定する変数を入れ替えたりしても、必ずcutが上にくるようになります。これを入れ替える方法はあるのでしょうか?

## A2
version 0.9.2より、`guide_legend(order = )`で順番を指定することができます:

```r
p <- ggplot(diamonds, aes(carat, price, colour = clarity, shape = cut)) +
  geom_point() +
  theme(legend.position = "top") +
  guides(shape = guide_legend(order = 2),
         colour = guide_legend(order = 1))
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->

```r
p + guides(shape = guide_legend(order = 1),
         colour = guide_legend(order = 2))
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-5-2.png)<!-- -->

なお`guide_legend`としているように、設定があたるのは凡例(legend)だけです。

## Q3 (コメント指摘より2015/8/2に追記)
上の内容は離散的な変数の凡例ですが、連続的な変数の凡例(colourbar)ではできますか?

```r
p <- ggplot(iris, aes(x = Species, y = Sepal.Length, colour = Sepal.Width)) + 
  geom_point()
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-6-1.png)<!-- -->

## A3
可能です。Q1の`reverse = TRUE`やQ2の`order = hoge`のオプションは`guide_colourbar`も対応しています。まず`reverse`での例はこちらです:

```r
p <- ggplot(iris, aes(x = Species, y = Sepal.Length, colour = Sepal.Width)) + 
  geom_point() +
  guides(colour = guide_colourbar(reverse = TRUE))
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-7-1.png)<!-- -->

`order`での例はこちらです:

```r
p <- ggplot(diamonds, aes(carat, price, colour = depth, shape = cut)) +
  geom_point() +
  theme(legend.position = "top") +
  guides(shape = guide_legend(order = 1),
         colour = guide_colourbar(order = 2))
p
```

![](09-sort_legend_in_ggplot2_files/figure-epub3/unnamed-chunk-8-1.png)<!-- -->

`guides`をつけていないと、`colourbar`のdepthが優先されて上に来ますが、このコードのように設定すると下に来ます。


## 参考
- [guide_legend. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/guide_legend.html)
- [guide_colourbar. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/guide_colourbar.html) (Q3に対応して追加, 2015/8/2)
- [R: sort legend in ggplot2 - Stack Overflow](http://stackoverflow.com/questions/9577680/r-sort-legend-in-ggplot2)
- [r - Controlling ggplot2 legend display order - Stack Overflow](http://stackoverflow.com/questions/11393123/controlling-ggplot2-legend-display-order)

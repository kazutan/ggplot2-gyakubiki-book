# ggplot2で帯グラフを作る

## Q
`ggplot2`で帯グラフを作成したいのですが、どうやったら描けるでしょうか?

## A
以下の要素を組み合わせると帯グラフが描けます:

1. 形状は棒グラフ(`geom = "bar`)
2. y軸をフルに設定(`position = "full`)
3. y軸をパーセンタイル標記に設定(`scale_y_continuous(labels = percent)`)
4. x軸とy軸を入れ替えて横棒グラフに設定(`coord_flip()`)
5. 項目順の調整 ※必要であれば

以下のコードから要素を追加していきます:

```r
library(ggplot2)
p <- ggplot(mtcars, aes(x = as.factor(gear), fill = as.factor(vs))) +
  geom_bar()
p
```

![](05-bandgraph_ggplot2_files/figure-epub3/unnamed-chunk-1-1.png)<!-- -->

### y軸をフルに設定
y軸を、一端からもう一端へと引き伸ばすには、`position = "fill"を設定します:

```r
p <- ggplot(mtcars, aes(x = as.factor(gear), fill = as.factor(vs))) +
  geom_bar(position = "fill")
p
```

![](05-bandgraph_ggplot2_files/figure-epub3/unnamed-chunk-2-1.png)<!-- -->

このとき、y軸のメモリが0-1.00と比率へ自動的に変化していることに留意してください。

### y軸をパーセンタイル標記に設定
`{scale}`パッケージを読み込んで、`scale_y_continuous(labels=percent)`の設定を追加します:

```r
require(scales)
```

```
## Loading required package: scales
```

```r
p <- ggplot(mtcars, aes(x = as.factor(gear), fill = as.factor(vs))) +
  geom_bar(position = "fill") +
  scale_y_continuous(labels = percent)
p
```

![](05-bandgraph_ggplot2_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

他にも方法はありますがこれが一番スムーズできれいにできます。{ggplot2}パッケージをインストールしているなら、{scales}はおそらくすでにインストールされています。

### x軸とy軸を入れ替えて横棒グラフに設定
この方法については、[ggplot2逆引き - ggplot2で縦軸と横軸をひっくり返したい - Qiita](http://qiita.com/kazutan/items/5c4ee243c48a64b44b2d)を参照してください。

```r
require(scales)
p <- ggplot(mtcars, aes(x = as.factor(gear), fill = as.factor(vs))) +
  geom_bar(position = "fill") +
  scale_y_continuous(labels = percent) +
  coord_flip()
p
```

![](05-bandgraph_ggplot2_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->

### 項目順の調整
この方法については、[ggplot2逆引き - x軸を並べ替えたい - Qiita](http://qiita.com/kazutan/items/7840f743d642122d1219)を参照してください。

```r
require(scales)
mtcars.v2 <- transform(mtcars, gear2 = gear * -1)
p <- ggplot(mtcars.v2, aes(x = reorder(gear, gear2), fill = as.factor(vs))) +
  geom_bar(position = "fill") +
  scale_y_continuous(labels = percent) +
  coord_flip()
p
```

![](05-bandgraph_ggplot2_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->

これで帯グラフの完成です。結構手間がかかります。

## 参考
- [position_fill. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/position_fill.html)
- [scale_x_continuous. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/scale_continuous.html)
- [ggplot2逆引き - ggplot2で縦軸と横軸をひっくり返したい - Qiita](http://qiita.com/kazutan/items/5c4ee243c48a64b44b2d)
- [ggplot2逆引き - x軸を並べ替えたい - Qiita](http://qiita.com/kazutan/items/7840f743d642122d1219)

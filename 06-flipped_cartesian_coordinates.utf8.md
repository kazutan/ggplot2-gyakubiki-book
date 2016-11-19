# ggplot2で縦軸と横軸をひっくり返したい


## Q
例えば以下のようなグラフで、x軸とy軸をひっくり返したい(横棒グラフ)にしたいのですが、どうしたらいいのでしょうか?

```r
library(ggplot2)
ggplot(mtcars, aes(x = as.factor(gear))) +
  geom_bar()
```

![](06-flipped_cartesian_coordinates_files/figure-epub3/unnamed-chunk-1-1.png)<!-- -->


## A
`+ coord_flip()`を追加すると、x軸とy軸が入れ替わります:

```r
ggplot(mtcars, aes(x = as.factor(gear))) +
  geom_bar() +
  coord_flip()
```

![](06-flipped_cartesian_coordinates_files/figure-epub3/unnamed-chunk-2-1.png)<!-- -->

この設定は、別に棒グラフだけではなく、全てに適用可能です。

## 応用
この`coord_flip()`を設定した場合、更に軸に設定を加えるときは**元の座標軸**に設定してください:

例) `count`軸の範囲を変更する場合:

```r
ggplot(mtcars, aes(x = as.factor(gear))) +
  geom_bar() +
  coord_flip() +
  ylim(c(0,20))
```

![](06-flipped_cartesian_coordinates_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

また、棒グラフを横にすると、下から順番にならびます。軸の内容の並べ替えについては、以下の記事を参照してください:
- [ggplot2逆引き - x軸を並べ替えたい - Qiita](http://qiita.com/kazutan/items/7840f743d642122d1219)

今回x軸はfactor型なので、順番を入れ替えるには以下のように`reorder()`を当てて変更しておく必要があります:

例) `as.factor(gear)`軸の順序を反転する場合:

```r
ggplot(mtcars, aes(x = reorder(as.factor(gear), gear * -1))) +
  geom_bar() +
  coord_flip()
```

![](06-flipped_cartesian_coordinates_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->

なお、x軸が連続変量であったならば、単に`+ scale_x_reverse()`を追加すればOKです。

例) `gear`軸の順序を反転する場合:

```r
ggplot(mtcars, aes(x = gear)) +
  geom_bar() +
  coord_flip() +
  scale_x_reverse()
```

![](06-flipped_cartesian_coordinates_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->

## 参考
- [coord_flip. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/coord_flip.html)
- [ggplot2逆引き - x軸を並べ替えたい - Qiita](http://qiita.com/kazutan/items/7840f743d642122d1219)

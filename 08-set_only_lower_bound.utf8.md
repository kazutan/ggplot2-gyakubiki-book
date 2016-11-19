# ggplotで数値軸の下限値(or上限値)だけを指定したい

## Q
数値軸の下限を0にして、上限は設定しないというのはできないのでしょうか。例えば以下のようなコードのイメジです:


```r
+ scale_y_continuous(minlim=0)
```

## A
1.0.0以降のバージョンであれば、下限値・上限値を設定するところに`NA`が使えます。以下をサンプルに設定します:


```r
library(ggplot2)
ggplot(mtcars, aes(wt, mpg)) +
  geom_point()
```

![](08-set_only_lower_bound_files/figure-epub3/unnamed-chunk-2-1.png)<!-- -->

これに、`scale_y_continuous(limits = c(0, NA))`を加えます:

```r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point() + 
  scale_y_continuous(limits = c(0, NA))
```

![](08-set_only_lower_bound_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

また、`ylim(c(0, NA))`でも指定可能ですし、x軸についても同様です:


```r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point() + 
  ylim(c(0, NA)) +
  xlim(c(-2, NA))
```

![](08-set_only_lower_bound_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->

0.9.xのバージョンを使っている場合は、`expand_limits(y=0)`が使用可能です:

```r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point() + 
  expand_limits(y=0)
```

![](08-set_only_lower_bound_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->

もし1.0.0以降を使用しているなら、わかりやすさも含めて前者の方法を用いるといいでしょう。

## 参考
この記事は、StackOverflowに投稿された以下の記事を参考に翻訳し、一部内容を変更して作成しました:

- [r - set only lower bound of a limit for ggplot - Stack Overflow](http://stackoverflow.com/questions/11214012/set-only-lower-bound-of-a-limit-for-ggplot)

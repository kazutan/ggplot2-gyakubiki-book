# ggplot2で同一グラフに2変数の折れ線グラフを描きたい

## Q
このようなデータが手元にあります:


```r
test_data <- data.frame(
var0 = 100 + c(0, cumsum(runif(49, -20, 20))),
var1 = 150 + c(0, cumsum(runif(49, -10, 10))),
date = seq.Date(as.Date("2002-01-01"), by="1 month", length.out=100))
```

この時系列変数`var0`と`var1`の両方共を、`date`をx軸にして`ggplot2`でどうやったら描けますか? できれば`var0`と`var1`の色を変えて、さらに凡例も付けれたら嬉しいです。

## A
もし変数が少ないのであれば、マニュアルで別々に作成ビルドアップできますよ:


```r
library(ggplot2)
ggplot(test_data, aes(date)) +
  geom_line(aes(y = var0, colour = "var0")) +
  geom_line(aes(y = var1, colour = "var1"))
```

![](01-Plotting_two_variables_files/figure-latex/unnamed-chunk-2-1.pdf)<!-- --> 

一般的なアプローチとしては、`tydyr`パッケージを利用してデータを縦型(long format)に変換していく方法があります:


```r
library(tidyr)
library(ggplot2)

test_data_long <- tidyr::gather(test_data, key="variable", value = value, -date) # 縦型に変換

knitr::kable(head(test_data_long, 6))
```


\begin{tabular}{l|l|r}
\hline
date & variable & value\\
\hline
2002-01-01 & var0 & 100.00000\\
\hline
2002-02-01 & var0 & 96.63169\\
\hline
2002-03-01 & var0 & 78.96030\\
\hline
2002-04-01 & var0 & 67.91438\\
\hline
2002-05-01 & var0 & 66.66130\\
\hline
2002-06-01 & var0 & 70.01509\\
\hline
\end{tabular}

```r
ggplot(data=test_data_long, aes(x=date, y=value, colour=variable)) +
  geom_line()
```

![](01-Plotting_two_variables_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 

データを縦型のデータに変換し、`var0`と`var1`を分けるための変数で色分けを指定すればこのようになります。

## 参考
この記事は、StackOverflowに投稿された以下の記事をベースに、コードを一部改変して翻訳して作成しました:
- [r - Plotting two variables as lines using ggplot2 on the same graph - Stack Overflow](http://stackoverflow.com/questions/3777174/plotting-two-variables-as-lines-using-ggplot2-on-the-same-graph)

関連ドキュメント:
- [geom_line. ggplot2 0.9.3.1](http://docs.ggplot2.org/current/geom_line.html)

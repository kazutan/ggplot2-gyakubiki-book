# ggplot2逆引き集

これはQiitaで行われている[**ggplot2逆引き**の記事](http://qiita.com/tags/ggplot2%E9%80%86%E5%BC%95%E3%81%8D)を収集し,書籍の形式にするためのリポジトリです。

## 書籍ファイルの作成方法

### 必要なパッケージ,環境など
Knitr, rmarkdown, bookdownのパッケージがデータのレンダリングに必要です。またpandocの新しいのが必要で,面倒でしたらRStudioの最新版をインストールしてください(内包してます)。
ggplot2逆引き記事内にて使用するパッケージも必要となります。おそらくggplot2パッケージぐらいで大丈夫だと思いますが,面倒でしたらtidyverseパッケージを導入してください。これをインストールするとHadleyverseなパッケージ群が自動的にインストールされます。
もしpdf bookを作りたいのであれば,マシンにtex環境が必要です。日本語のフォントにIPAフォントを指定していますので,以下からダウンロードしてください。

http://ipafont.ipa.go.jp/

また,bookdownはutf-8しか受け付けません。そのためwindowsではうまく動かないかもしれません(未検証)。もし何かありましたらissueなり@kazutan までご連絡ください。

私の作業環境(動作確認環境)は,最後にまとめて表示しています。

### Download

git cloneして持ってくるか,右側のDownload Zipで持ってきてください:

```
$ git clone git@github.com:kazutan/ggplot2-gyakubiki-book.git
```

### レンダリング(本のファイル作成)

#### 種類

- gitbook形式: 以下のコードを実行
```
bookdown::render_book("index.Rmd", output_format = "bookdown::gitbook")
```
- epub形式: 以下のコードを実行
```
bookdown::render_book("index.Rmd", output_format = "bookdown::epub_book")
```
- pdf形式: 以下のコードを実行
```
bookdown::render_book("index.Rmd", output_format = "bookdown::pdf_book")
```

RStudioを利用しているなら,BuildパネルでBuilde Bookから選択してください。

### 生成物の場所

生成物は,`_book`ディレクトリに置かれるように設定してます。`.epub`と`.pdf`は単独ファイルで,それ以外はgitbook形式のファイルとなります。

## session info

```
Session info -----------------------------------------------------------------------------------
 setting  value                       
 version  R version 3.3.2 (2016-10-31)
 system   x86_64, linux-gnu           
 ui       RStudio (1.0.44)            
 language (EN)                        
 collate  en_US.UTF-8                 
 tz       <NA>                        
 date     2016-11-12                  

Packages ---------------------------------------------------------------------------------------
 package    * version  date       source                            
 backports    1.0.4    2016-10-24 cran (@1.0.4)                     
 bookdown     0.1.18   2016-11-08 Github (rstudio/bookdown@601437d) 
 devtools     1.12.0   2016-06-24 CRAN (R 3.3.1)                    
 digest       0.6.10   2016-08-02 cran (@0.6.10)                    
 evaluate     0.10     2016-10-11 CRAN (R 3.3.1)                    
 htmltools    0.3.5    2016-03-21 CRAN (R 3.3.1)                    
 httpuv       1.3.3    2015-08-04 CRAN (R 3.2.3)                    
 knitr        1.15     2016-11-09 CRAN (R 3.3.2)                    
 magrittr     1.5      2014-11-22 CRAN (R 3.2.3)                    
 memoise      1.0.0    2016-01-29 CRAN (R 3.2.3)                    
 mime         0.5      2016-07-07 CRAN (R 3.3.1)                    
 miniUI       0.1.1    2016-01-15 cran (@0.1.1)                     
 R6           2.2.0    2016-10-05 CRAN (R 3.3.1)                    
 Rcpp         0.12.7   2016-09-05 CRAN (R 3.3.1)                    
 rmarkdown    1.1.9014 2016-11-08 Github (rstudio/rmarkdown@91c7de2)
 rprojroot    1.1      2016-10-29 cran (@1.1)                       
 rstudioapi   0.6      2016-06-27 CRAN (R 3.3.1)                    
 shiny        0.14.2   2016-11-01 cran (@0.14.2)                    
 stringi      1.1.2    2016-10-01 CRAN (R 3.3.1)                    
 stringr      1.1.0    2016-08-19 CRAN (R 3.3.2)                    
 withr        1.0.2    2016-06-20 CRAN (R 3.3.1)                    
 xtable       1.8-2    2016-02-05 CRAN (R 3.2.3)                    
 yaml         2.1.13   2014-06-12 CRAN (R 3.2.3)     
```

## Lisence
書籍作成に関する内容については[CC0](https://creativecommons.org/publicdomain/zero/1.0/)とします。
各記事に関するライセンスは[Qiitaの利用規約](http://qiita.com/terms)および各記事の執筆者の指定に準じるものとします。
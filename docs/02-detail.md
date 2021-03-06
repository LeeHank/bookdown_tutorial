# Detail

## 調整地圖

-   這邊先列出所以可以調整的東西，要去哪些file中做設定，等下才會一一細講：

| \_output.yml               | \_bookdown.yml         | index.Rmd                  | 各個 .Rmd                 |
|----------------------------|------------------------|----------------------------|---------------------------|
| 設定TOC只顯示第一/二層標題 | 設定`new_session: yes` | 增加favicon                | Group chapters into parts |
| 設定TOC的header和footer    | 設定`output_dir: docs` | Book cover and description | appendix頁                |
| 設定下載按鈕               | 設定章節順序           | 增加github連結             | Section (un)numbering     |
| 增加edit link              | 設定章節前綴           |                            |                           |
| 增加github連結             |                        |                            |                           |
|                            |                        |                            |                           |

## 與章節組織有關的調整    

### 設定章節順序

-   打開`_bookdown.yml`，加上`rmd_file:`的指令，範例如下：

<!-- -->

    rmd_files:
    - index.Rmd
    - 01-intro.Rmd
    - 04-application.Rmd
    - 02-literature.Rmd
    - 03-method.Rmd
    - 05-summary.Rmd
    - 06-references.Rmd

-   這樣就可以決定章節順序了 

### 有些章節我想隱藏起來

-   目前publish電子書的共識是：不要寫好完整一本書才上傳，而是一直讓你的書保持著publish的狀態，也讓讀者參與這個寫作的過程(因為他也可以即時提醒你哪裡寫錯要改)\

-   所以我們一邊寫，就會一邊push到github上做發布\

-   但有些章節我可能就寫到一半而已，還不想讓別人看，那有兩種做法：
    -   在`bookdown.yml`中，指定章節順序時，把還不想公布的章節拿掉就好(compile時就不會去管那些.Rmd檔)\
    -   如果你連.Rmd檔都不想讓別人看到，那你可以在local的電腦上開個分支，專門去寫那些章節，等到時機成熟才merge回master，並push到github上。  

### 設定章節前綴  

### 章節的(un)numbering  

* 預設的每個章節都會有numbering，如果想移除，就在.Rmd的heading最後，加上`{-}`  
* 舉例來說，我們通常不希望`index.Rmd`裡的標題也被編號，那我們就可以這樣寫:  

```
# My section {-}
```

* 那他就不會被編號，然後後續的章節標號會調整  
* 所以看起來，如果你要所有章節都不要編號，可能要一個一個去加`{-}`，有點累。  


### 設定TOC只顯示第一/二層標題

-   如果你的章節的hierarchies很深，那全部顯示在TOC，會便非常長而難以閱讀。所以這邊就可以顯示，我只要顯示第一層標題就好，或最深只顯示到第二層標題就好

-   打開_output.yml檔，在config下的toc的下面，增加\`collapse: section\`，那就只會顯示第一層標題：

<!-- -->

    bookdown::gitbook:
      config:
        toc:
          collapse: section

-   更詳細的設定，請看[bookdonw的官方文件第3.1節](https://bookdown.org/yihui/bookdown/html.html#gitbook-style)

### 設定TOC的header和footer  

* 其實，就是一開始`_output.yml`預設的東西，我直接貼在下面就好  

```
bookdown::gitbook:
  config:
    toc:
      before: |
        <li><a href="./">A Minimal Book Example</a></li>
      after: |
        <li><a href="https://github.com/rstudio/bookdown" target="blank">Published with bookdown</a></li>
```


### TOC中，把相似的章節組成一個區塊  

* 假設我現在在`_bookdown.yml`中，已經設定好三個章節：  

```
- chapter1.1.Rmd
- chapter1.2.Rmd
- chapter1.3.Rmd
```

* 然後，我想在這三個章節上，加上一個group title叫`chapter1: blabla`，讓讀者在看TOC時，可以清楚知道這三節都屬於chapter1  
* 那做法是：  
  * 先開一個.Rmd檔，命名為chapter1.Rmd，裡面的內容寫`# (PART\*) chapter1 {-}`。這邊的`(PART\*)`，是告訴bookdown，他在TOC中是一個PART，他是不能點的，只是show給你看他的標題。最後的`{-}`，表示這個title不要幫我編號。  
  * 然後，把這個chapter1.Rmd放到`_bookdown.yml`裡面，變成下面這樣，就搞定了：  

```
- chapter1.Rmd
- chapter1.1.Rmd
- chapter1.2.Rmd
- chapter1.3.Rmd
```

### 增加appendix  

* 如果想加入appendix，那我會希望在TOC中，先出現appendix的標題(不能點)，然後是各個appendix的章節。  
* 那就可以這樣做：先開個.Rmd檔，命名為appendix.Rmd，內容如下  

```
# (APPENDIX) Appendix {-} 

# Appendix A
Here is the first appendix chapter.

# Appendix B
Here is the second appendix chapter.
```

* 接著，把這個appendix.Rmd，放到`_bookdown.yml`的目錄下即可

```
- chapter1.Rmd
- chapter1.1.Rmd
- chapter1.2.Rmd
- chapter1.3.Rmd
- appendix.Rmd
```

## 和美感有關的調整：Code highlighting  

* 我們可以設定，code chunk中，highlight的顏色要使用哪種風格  
* 打開`_output.yml`，增加`highlight: `的設定，裡面可選`default`, `tango`, `pygments`, `kate`,...。  

```
bookdown::gitbook:
 highlight: tango
```

## 和sharing有關的調整  

### 讓讀者可以下載source code 或 edit 建議  

* 打開`_output.yml`檔  
* 增加`edit:`, `link:`和`text:`三項，設定如下：  

```
bookdown::gitbook:
  config:
    edit:
      link: https://github.com/username/repository/edit/master/%s
      text: "Suggest an edit"
```

* 把username換成github的帳號，repository換成這個project的repository名稱，就可以了  
* 那讀者在看你的書時，上面的toolbar，就會有個`edit`的按鈕，他滑鼠移過去，就會看到顯示的text(e.g. 這邊的Suggest an edit)。點下去後，就會跳出一個視窗，系統會把你的link中的repository內，讀者正在看的這頁的.Rmd檔丟出來給他看，讓他填修改建議。  


### 讓讀者下載成pdf or rmd file  

* 可以在`_output.yml`中，加入`download:`的設定，讓讀者可以下載看到的東西：  

```
bookdown::gitbook:
  config:
    toc:
      collapse: section
    download: ["pdf","rmd"]
```

* 這邊要注意，你寫pdf的話，原本的output format就也要有pdf，這樣才能讓讀者下載到pdf  
* 你有寫rmd的話，那剛剛上一步的edit也要做設定，系統才知道要去哪裡抓rmd檔給讀者  

### 讓讀者連回你的github  

* 如果你想在網頁上加個按鈕，讓user按下去後，就可以連到你的github，那可以這麼做：  
* 先打開`_output.yml`，加上`sharing:`的設定，如下：  

```
bookdown::gitbook:
  highlight: tango
  config:
    toc:
      collapse: section
    download: ["rmd"]
    sharing:
      github: yes
      facebook: no
      twitter: no
      all: no
```

* 然後打開`index.Rmd`，在最上面的YAML設定檔中，新增`github-repo:`，把你的github資訊設定好：  

```
title: A Minimal Book Example
author: Yihui Xie
date: '2021-12-11'
site: bookdown::bookdown_site
documentclass: book
description: This is a minimal example of using the bookdown package to write a book.
  The output format for this example is bookdown::gitbook.
github-repo: your-github/your-repo
```

* 那只要讀者點了github圖標，就會連到你指定的repository  

## 其他之後有時間再整理的內容  

### index.Rmd裡面還很多不知道在寫啥的  

* 原始的index.Rmd裡面，還有很多設定我都不知道在幹麻:

```
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
```

* 我現在就是先刪光光，反正應該沒差拉  

### .Rmd裡面還可以設定各種可用的細節

#### Internal cross-references

#### Numbering and referencing equations

#### Figures captions

#### Tables

#### Citations

### Footnotes




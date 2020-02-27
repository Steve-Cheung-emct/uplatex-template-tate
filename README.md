# 利用 Jlreq 實現的標點擠壓和標點懸挂方法

`Jlreq.cls` 以豐富的選項著稱。我們可以利用 jlreq 的虛擬字體，進行標點懸挂。

### 參考網頁
[阿部 紀行氏的日本語LaTeX排版組件](https://github.com/abenori/jlreq/blob/master/README-ja.md)．

## Jlreq 使用方法
通常
```latex
\documentclass[ % ドキュメントクラス
	uplatex, % upLaTeXを使う
	tate, % 縦書き
%    twocolumn, % 二段組
	hanging_punctuation, % ぶら下げ組（標點懸挂）
	paper=a4, % 用紙サイズ
	reference_mark=interlinear, %注の合標を行間に
	sidenote_type=symbol, %sidenote的形式
	sidenote_length=0mm, %當使用傍注時必須設置此項目
	book % 書籍のためのスタイル
]{jlreq}
```

その他，`oneside / twoside / onecolumn / twocolumn / titlepage / notitlepage / draft / final / openright / openany / leqno / fleqn`というよくあるオプションを受け付けます．

### 注意
`tate` 模式下，notitlepage 不起作用。


### `\sidenote` 傍注命令

` sidenote_length=0mm, ` 當使用傍注時必須設置此項目。將0mm改爲20mm或 10zw（例）。
用法與`\footnote`類似。

```latex
正文\sidenote{正文所注文字}{注釋内容……}。正文。
```


### `\endnote`
後注的使用。與`\footnote`同樣的格式．可設置`\jlreqsetup{endnote_position=headings}`或 `\jlreqsetup{endnote_position=paragraph}`兩種模式。前者在章標題前刷新。

使用方法：
```latex
正文。正文。正文。

\vspace{\baselineskip}
\theendnotes
\clearpage
\chapter{xxxx}
```

### `\warichu`
輸出割注．自動計算行分割位置．（複数回的編譯十分必要．）`\warichu*`用於手動設置改行位置．格式如下：
```latex
\warichu*{{一行目右側行，每行xx字} & {一行目左側行，每行xx字}\\{}
 {二行目右側行} & {二行目左側行}}……
```

以上。`&`省略的場合表示自動模式。自動模式下的奇數字數改行將發生問題。需依賴手動改行。


### 其他内容

詳細參考[阿部 紀行氏的日本語LaTeX排版組件](https://github.com/abenori/jlreq/blob/master/README-ja.md)．


## JFM
以下のような独自のJFMを使います．パッケージによっては，パッケージ独自のJFMや，また標準のJFMを使うように設定がし直される場合があります．本クラスファイルで使用するJFMを使う場合には，パッケージオプションなどを適切に与えて設定をする必要があります．

### pLaTeX/upLaTeXの場合
JFMの名前は次の通りです．`[]`で囲まれている文字は設定により入ったり入らなかったりします．
```
[u][b][z]jlreq[g][-v]
```
それぞれの文字は以下の場合に入ります．

* `u`: upLaTeX利用時
* `b`: ぶら下げ組み利用時．（クラスオプションに`hanging_punctuation`が指定された時．）
* `z`: 行頭における開き括弧類の前の空きが，段落開始時が全角二分，折り返し時が二分の時．（クラスオプションに`open_bracket_pos=zenkakunibu_nibu`が指定された時．）
* `g`: ゴシック用フォント．
* `-v`: 縦書き用．

例：（在此之前需為系統配置日文字體嵌入方案）

```latex
\documentclass[a4paper]{utbook}
%%%%%%%%%%%%%%% ubzjlreq-h/v  %%%%%%%%%%%%%%%
\DeclareFontFamily{JY2}{ubzjlreq}{}
\DeclareFontFamily{JT2}{ubzjlreq}{}

\DeclareFontShape{JY2}{ubzjlreq}{m}{n}{<->s*[0.924690]ubzjlreq}{}
\DeclareFontShape{JY2}{ubzjlreq}{m}{it}{<->ssub*ubzjlreq/m/n}{}
\DeclareFontShape{JY2}{ubzjlreq}{m}{sl}{<->ssub*ubzjlreq/m/n}{}
\DeclareFontShape{JY2}{ubzjlreq}{m}{sc}{<->ssub*ubzjlreq/m/n}{}

\DeclareFontShape{JT2}{ubzjlreq}{m}{n}{<->s*[0.924690]ubzjlreq-v}{}
\DeclareFontShape{JT2}{ubzjlreq}{m}{it}{<->ssub*ubzjlreq/m/n}{}
\DeclareFontShape{JT2}{ubzjlreq}{m}{sl}{<->ssub*ubzjlreq/m/n}{}
\DeclareFontShape{JT2}{ubzjlreq}{m}{sc}{<->ssub*ubzjlreq/m/n}{}

\DeclareRobustCommand\ubzjlreq{\kanjifamily{ubzjlreq}\selectfont}
%%%%%%%%%%%%%%% ubzjlreq-h/v  %%%%%%%%%%%%%%%

\begin{document}
\ubzjlreq	% 直接在此使用這個命令。
\input{maintext.tex}	% 載入的正文文本。
\end{document}
```
## 注意：如何為系統配置日文字體嵌入方案

應使用下列命令查看本機支持的日文字型。TeX Live 2019 起支持 了一鍵配置`noto`、`noto-otc`、`sourcehan`、`sourcehan-otc` 四種思源字體。（`*nix/ OS X sudo `必要；Windows 管理員權限打開命令提示符。）
```
kanji-config-updmap-sys  status
```

使用下列命令為系統配置日文明體/黑體嵌入字型。需要安裝思源字體。這里推薦使用
NOTO或者思源的OTF 超級版。見[思源黑體的日語超級版](https://github.com/adobe-fonts/source-han-sans/tree/release/OTF/Japanese)
以及[思源明體（宋体）日語超級版](https://github.com/adobe-fonts/source-han-serif/tree/release/OTF/Japanese)
或者點擊下載[思源NOTO宋体日語超級版OTF](https://noto-website-2.storage.googleapis.com/pkgs/NotoSerifJP.zip）

只需安裝L(ight) / R(egular) / M(edium) / B(old) 四個字重即可。
然後命令行執行一條下列相關的語句。

（如安裝的小塚明朝，則執行第一條。如安裝的noto otf 版，則執行第二條。不可多次執行。）
```
# 小塚フォント(Pr6N版)を使う; 2004JIS字形指定（小塚明朝Pr6N約有15000個繁體漢字）
$ kanji-config-updmap-sys --jis2004 kozuka-pr6n

# 思源字體 otf 版（ JP 超級版或 JP 地區版，其中JP 超級版支持更多的 UTF 索引漢字。）
$ kanji-config-updmap-sys --jis2004 noto

# 思源字體 ttc N合一版。
$ kanji-config-updmap-sys --jis2004 noto-otc

# 思源字體 otf 版（ JP 超級版或 JP 地區版，其中JP 超級版支持更多的 UTF 索引漢字。）
$ kanji-config-updmap-sys --jis2004 sourcehan

# 思源字體 ttc N合一版。
$ kanji-config-updmap-sys --jis2004 sourcehan-otc

# 獨立行政法人情報処理推進機構（IPA）發行的免費商用漢字字型。大約9000多個繁體漢字。
$ kanji-config-updmap-sys --jis2004 ipaex
```

這之後，為系統配置思源明体漢字的韓版字體。因思源日版部分新字形（簡體字）寫法和大陸相異。所以使用思源韓版字形作爲簡體備用字庫。（或者使用華康宋體/楷體/黑體，漢儀的小隸書，方正的仿宋/書宋/隸變舊字形/楷體超大字符集等。）將在下面説明。

### 配置的後備字形（思源韓版）
思源韓版的簡體字部分來自大陸華文版，繁體部分則大多采用了康熙字形。
下載地址見：[思源明體韓版超級OTF](https://github.com/adobe-fonts/source-han-serif/tree/release/OTF/Korean)

```latex
\documentclass[a4paper]{utbook}
% 思源韓版的NFSS 2 配置
\DeclareFontFamily{JY2}{upstsl}{}
\DeclareFontFamily{JT2}{upstsl}{}

\DeclareFontShape{JY2}{upstsl}{m}{n}{<->s*[0.924690]upstsl-h}{}
\DeclareFontShape{JY2}{upstsl}{m}{it}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JY2}{upstsl}{m}{sl}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JY2}{upstsl}{m}{sc}{<->ssub*upstsl/m/n}{}

\DeclareFontShape{JT2}{upstsl}{m}{n}{<->s*[0.924690]upstsl-v}{}
\DeclareFontShape{JT2}{upstsl}{m}{it}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sl}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sc}{<->ssub*upstsl/m/n}{}

\DeclareRobustCommand\upstsl{\kanjifamily{upstsl}\selectfont}

\AtBeginDvi{%
  \special{pdf:mapline upstsl-h	unicode		SourceHanSerifK-Regular.otf}
  \special{pdf:mapline upstsl-v	unicode		SourceHanSerifK-Regular.otf	-w	1}
}

\begin{document}
\upstsl	% 直接在此使用這個命令。表示思源韓版作爲正文字体使用。
\input{maintext.tex}	% 載入的正文文本。
\end{document}

```
那麽，合起來怎麽用呢？

## 使用 EmEditor 進行批量的脚本替換

### 思路：

* 先使用 BabelMap 軟件提取字體的Cmap。
刪除cmap 前面一大段描述性文字，只留下 `0x01`開始的長列表。 利用 EmEditor 批量替換功能，將 `0x` 替換成 `\UI{`；將 `\t` 替換成 `}\t` ；將換行符 `\r\n` 替換成 `\r\n\r\n`（表示兩個換行，即分段落。） 
* 使用 upLaTeX 的 otf 宏包，配合 NFSS 字體設置，將漢字列表輸出。NFSS 配置見下：
```latex
\documentclass[a4paper,9pt]{ujarticle}
\usepackage[uplatex,deluxe]{otf}
\setlength{\parindent}{0mm}

\DeclareFontFamily{JY2}{upstsl}{}
\DeclareFontFamily{JT2}{upstsl}{}

\DeclareFontShape{JY2}{upstsl}{m}{n}{<->s*[0.924690]upstsl-h}{}
\DeclareFontShape{JT2}{upstsl}{m}{it}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sl}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sc}{<->ssub*upstsl/m/n}{}

\DeclareFontShape{JT2}{upstsl}{m}{n}{<->s*[0.924690]upstsl-v}{}
\DeclareFontShape{JT2}{upstsl}{m}{it}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sl}{<->ssub*upstsl/m/n}{}
\DeclareFontShape{JT2}{upstsl}{m}{sc}{<->ssub*upstsl/m/n}{}

\DeclareRobustCommand\upstsl{\kanjifamily{upstsl}\selectfont}
\AtBeginDvi{%
  \special{pdf:mapline upstsl-h	unicode		SourceHanSerifK-Regular.otf}
  \special{pdf:mapline upstsl-v	unicode		SourceHanSerifK-Regular.otf	-w	1}
}
\begin{document}
\begin{multicols}{4}
\upstsl	% 表示思源韓版作爲正文字体使用。
\input{1.txt}  % 載入的Cmap 漢字清單。
\end{multicols}
\end{document}
```
* 利用 python 小程序，將漢字統計出來。python小程序的配置如下：（保存為 test.py，再使用CMD調用）

### CMD 脚本
```
chcp 65001
python test.py  > ./col.txt
```

把需要进行字数统计的文件保存为“`sh.tex`”。

另：如何统计不同字符集的割集？ 

例：统计GB2312对AJ1-6的割集。

方法：三次复制AJ1-6的汉字表，再只复制一次GB2312汉字表。筛选之后：

* 权重为4的是两者的交集；

* 权重为3的是AJ1-6对GB2312的割集；

* 权重为1的则是GB2312对AJ1-6的割集。

例：如何筛选GB2312(GB0)、Big5、AJ1-6三者之间的交集和割集？

方法：

* 5次复制GB2312汉字列表、

* 2次复制AJ1-6汉字列表、

* 1次复制Big5汉字列表。

（AJ1-6和Big5都有大约1万3千个不重复汉字）

筛选之后：

* 权重为`8`的表示三者的公共交集；

* 权重为`7`的表示GB0和AJ1-6的小交集；

* 权重为`6`的表示GB0和Big5的小交集；

* 权重为`5`的表示GB2312对AJ1-6、Big5的割集；

* 权重为`3`的表示AJ1-6和Big5的交集；

* 权重为`2`的表示AJ1-6对GB0、Big5的割集；

* 权重为`1`的表示Big5对AJ1-6、GB0的割集。


### python 小程序
```python
#-*- coding:utf-8 -*-
import io
import sys
import urllib.request
#改變標準輸出的默認編碼為utf8
sys.stdout = io.TextIOWrapper(sys.stdout.buffer,encoding='utf8') 
from collections import Counter
# 統計總漢字数，文本均以utf-8格式保存
TotalChar = [x for x in open(r"./sh.tex", "r", encoding="utf-8").read() if  13312<=ord(x)<=19893 or 19968<=ord(x)<=40943 or 131072<=ord(x)<=173782 or 63744<=ord(x)<=64255 ]

# CJK 基本平面 
# 19968<=ord(x)<=40943 
# CJK-A 
# or 13312<=ord(x)<=19893
# CJK-B
# or 131072<=ord(x)<=173782
# CJK-C
# or 173824<=ord(x)<=177972
# CJK-D
# or 177984<=ord(x)<=178205
# CJK 兼容漢字
# or 63744<=ord(x)<=64255
# CJK 兼容漢字擴展
# or 64048<=ord(x)<=64106 or 64112<=ord(x)<=64217
# or 194560<=ord(x)<=195103 or 917760<=ord(x)<=917999 
# 統計不同漢字的重複次数。
CountChar = Counter(TotalChar)
print("總漢字数：", len((TotalChar)))
print("不同漢字数：", len((CountChar)))
print(CountChar)
# 注意python的縮進。

```
* 使用 emeditor 將 結果整理成一列漢字。撷取我们需要的割集或者补集的内容。
批量替換`': 1, '`為`\r\n`，再刪除文件行首行尾的無關内容。
* 使用excel 快速建立批量替換脚本。保存格式爲UTF16-LE有簽名。後綴名 `*.tsv` 格式如下：
```
on	撂	{\\upstsl{撂}}	
on	㑇	{\\upstsl{㑇}}	
on	㑳	{\\upstsl{㑳}}	
on	㑔	{\\upstsl{㑔}}	
...	...	...
```
* 然後打開需要替換的小説文本進行批量替換。

（因爲是批量替換脚本，每次替換都是不可逆的過程，每進行都需要提前備份文件。）

emeditor 其他使用説明 詳細互聯網搜索。


## 總結

### 正確配合 正文`\ubzjlreq` 和 後備字型 `\upstsl`

（正文前面是初始化的字型，為 \ubzjlreq 命令，并且配合者雙欄命令等）

摘取一段小説正文如下：
```
便在此時，忽聽得碑林旁的樹叢之後，傳出一陣鐵鍊{\upstsl{啷}}{\upstsl{噹}}之聲，又聽後一人誦唸佛經道︰

「是時藥叉共王立要，即於無量百千萬億大衆之中，説勝妙伽他日︰

\begin{quotation}
由愛故生憂\hskip8pt由愛故生怖

若離於愛者\hskip8pt無憂亦無怖\dash{}」
\end{quotation}
……
```

### 詳見附錄文件列表

* `pltex.tex` main.tex 主文件
* `settings.sty` 本次實驗的字體配置
* `編譯 main - pltex.bat` 創建pdf的cmd脚本程序
* `sz.cls` 主class 文件，來自 熊本學園大學院 小川 弘和老師的`upLaTeX`改版（原版為`platex`版）。
* `5.tex` 為 小説正文文本。垂直標點樣式。
* `4.tex` 為 小説正文文本。水平標點樣式。
* `warichus.sty` 日常的割注改版，正文字設置為割注字的1.8倍爲宜。如正文設置為14pt，則割注字設置為8~9pt（只要不出錯，割注字越大越好），字號配置在第62行目。來自 [阿部 紀行氏的網頁](http://abenori.blogspot.com/2016/07/warichu-eplatex.html)。
* `Unicode基本平面+CJK-A 對 AJ 1-6 的補集（不完整）.tsv` Unicode 基本平面+CJK-A 對 adobe Japan 1-6 的補集（共11156字，其中AJ1-6共13382字，Unicode基本平面+CJK-A 共約2萬4千多字）。
* `AJ 1-6 兼容互換.tsv` 將常用漢字互換到兼容漢字，以便通過日文字體使用常用漢字的康熙字形。
* `AJ 1-6 逆向兼容互换(保留常用异体汉字).tsv` 前者的逆向互換。但保留少量基本常用漢字。 
* `縦書標點互換 - 橫2縱.tsv` 將橫排標點替換成縦書模式。
* `縦書標點互換 - v 2 h.tsv` 將縦書標點替換成橫排模式。
* `橫書標點替換-半角2全角.tsv` 將橫排半角標點替換為全角模式。
* `TFM>symt-kr*` 付錄 思源明体韓國版tfm（從`upstsl-h/v` 拷貝而來）
* `VF>symt-kr*` 付錄 思源明体韓國版虛擬字體（從`pxcopyfont` 複製出來）

安裝：
* TFM to `C:\texlive\texmf-local\fonts\tfm\sourcehanskr`
* VF to `C:\texlive\texmf-local\fonts\vf\sourcehankr`
* 執行 `mktexlsr`刷新文件樹即可。



## 已知問題

### SZ.CLS 橫排下，頁碼位置異動，不規範。

 解決辦法：使用ujbook.cls

--------------
## 參考網頁
[阿部 紀行氏的日本語LaTeX排版組件](https://github.com/abenori/jlreq/blob/master/README-ja.md)．

[(u)pTeX における和文多書体の実現 〜Sierraの全和文フォント出力を例として〜 - TeX Alchemist Online](https://doratex.hatenablog.jp/entry/20161206/1480950097)

[(u)pLaTeX のデフォルトの日本語フォントを好きなやつに変える方法 - Qiita](https://qiita.com/zr_tex8r/items/15ec2848371ec19d45ed)



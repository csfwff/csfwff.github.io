title: Tesseract-OCR样本训练
date: '2019-08-06 21:01:27'
updated: '2021-01-06 10:49:56'
tags: [Tesseract-OCR]
permalink: /articles/2019/08/06/1565096487696.html
cover: https://tmx.fishpi.cn/img/flower-6925027_1920.jpg
categories: 
- 菜鸡修炼手册
---
![855545aaaf2b5680d832abf7a0a11269.png](https://tmx.fishpi.cn/img/flower-6925027_1920.jpg)

之前闲着写了一个识别小篆的APP，用到了Tesseract-OCR，由于没有小篆的训练数据，只能自己搞，五六千个字吧，一个个调整，当时花了挺多时间的，哈，记录下训练过程

Tesseract-OCR 仓库地址 ：https://github.com/tesseract-ocr/tesseract

### 1.  下载安装Tesseract

[到这里下载](https://github.com/UB-Mannheim/tesseract/wiki)，下载完成自行安装
这时候已经可以使用自带的训练数据进行文本识别了，具体命令为

```
tesseract imagename outputbase [-l lang] [-psm pagesegmode] [configfile...]
```

简单点就` tesseract myscan.png out`就可以了
好了，今天主要讲怎么训练，关于使用可以[查看文档](https://github.com/tesseract-ocr/tesseract/wiki)

### 2. 下载box editor

box editor在训练过程中需要用到，提前下载。[下载页面](https://github.com/tesseract-ocr/tesseract/wiki/AddOns#Tesseract_box_editors_and_traning_tools)，我下载的是java版本，也就是jTessBoxEditor，具体下载地址是[这里](https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/)，使用时需要有java环境，因为我写Android，环境都是配好的，反正配置起来也不难，搜一下就行，略过。下载回来解压，双击jar能正常打开，基本就没问题了
![QQ截图20190807091500.png](https://tmx.fishpi.cn/img/20210104093220536.png)

### 3.  准备训练内容

要识别什么文字，就准备什么文字。这里我们识别小篆，于是我准备了一份含有6598个已经去除重复汉字的文档，通过设置字体将所有文字转换为方正小篆体，随后调整每行文字个数为10个，文字大小为28号，行间距为1.0倍，方便识别。然后通过系统的打印功能，将整个文档打印成tif格式的图片，部分截图


<img src=https://tmx.fishpi.cn/img/lfK_1-41b73e25.png width=300>

全部转换为图片后，按照`chi.xiaozuan.exp0, chi.xiaozuan.exp1, ……`的规律依次命名各个图片。
![QQ截图20190807091735.png](https://tmx.fishpi.cn/img/20210104093320864.png)

### 4.  创建box文件

命名完成后，执行

```
tesseract chi.xiaozuan.exp0 chi.xiaozuan.exp0 -l chi_sim batch.nochop makebox
```

命令生成box文件，并且按照相同的命令将剩余的其他图片也生成box文件，命令执行成功后，文件夹内会出现与图片名字相同的box文件。
如果样本多的话，建议写个批处理循环跑。
![QQ截图2019080709183601c7262f.png](https://tmx.fishpi.cn/img/20210104164141334.png)

### 5.  box校正

生成box文件后，一般情况下生成的box并不是全部正确的，因此需要手工调整，使其正确切分文字，此时打开前面下载的jTessBoxEditor，选Box Editor，打开图片挨个调整，校正每个文字，校正完成后如下图：
![图片22fc8c5d0.png](https://tmx.fishpi.cn/img/20210104093006520.png)

校正需要使每个小篆文字刚好被方框框住，并且在左侧对应的表格内填入正确的简体字。
`merge`，`split`，`insert`，`delete`分别为合并，分割，插入和删除，另外选择`Box View`可以使用一些快捷键，具体看图：
![QQ截图201908070924431cb38d3f.png](https://tmx.fishpi.cn/img/20210104164234756.png)

（大部分时间都花在了这个步骤，基本每个字都要调整，搞吐血了都快）

### 6.  训练

使用修改正确的box文件对Tesseract进行训练，执行以下命令：

```
tesseract.exe chi.xiaozuan.exp0.tif chi.xiaozuan.exp0 nobatch box.train
```

训练完成后，系统将会生成同名的tr文件
同样，如果样本多，建议跑循环
![QQ截图20190807092518.png](https://tmx.fishpi.cn/img/20210104093621474.png)

### 7.  生成Character集合

训练完成后需要生成Character集合，执行以下命令：

```
unicharset_extractor.exe chi.xiaozuan.exp0 chi.xiaozuan.exp1 chi.xiaozuan.exp2……
```

需要将所有图片合并成一个Character集合，生成一个名为`unicharset`的文件

### 8.  创建字体特征文件

在训练目录下创建一个名为font_properties的字体特征文件，且该文件需要不含BOM头，文件内容为xiaozuan 0 0 0 0 0

### 9.  创建剩余所需文件

执行以下两条命令：

```
mftraining.exe -F font_properties -U unicharset -O chi.xiaozuan.exp0.tr chi.xiaozuan.exp1.tr……
```

```
cntraining.exe chi.xiaozuan.exp0.tr chi.xiaozuan.exp1.tr……
```

执行完成后会生成`inttemp`、`pffmtable`、`normproto`、`shapetable`四个文件，以及一个叫`chi.unicharset`的文件，手动修改前四个文件的名称为`chi.nttemp`、`chi.pffmtable`、`chi.normproto`、`chi.shapetable`
![QQ截图20190807094412475df0e9.png](https://tmx.fishpi.cn/img/20210104164322912.png)

### 10. 合并文件

最后合并以上文件生成训练好的可以直接使用的数据文件，执行以下命令：

```
combine_tessdata chi
```

命令执行完成后会生成chi.traineddata，这就是我们最终需要的数据，可以直接丢在Tesseract的数据文件夹中使用。

### 11. 附上我的批处理脚本，路径次数自行修改，也可以使用相对路径

11.1. makebox

```
echo run make box
set time=11
for /l %%i in (0,1,%time%) do (
	tesseract.exe E:\svn\tesseract\testData\chi.xiaozuan.exp%%i.tif E:\svn\tesseract\testData\chi.xiaozuan.exp%%i -l chi_sim batch.nochop makebox 
)
pause
```

11.2. train

```
echo run train
set time=11
for /l %%i in (0,1,%time%) do (
	tesseract.exe E:\svn\tesseract\testData\chi.xiaozuan.exp%%i.tif E:\svn\tesseract\testData\chi.xiaozuan.exp%%i  -l chi_sim nobatch box.train
)
pause
```

11.3. unicharset

```
echo run unicharset_extractor
setlocal EnableDelayedExpansion
set time=11
for /l %%i in (0,1,%time%) do (
	set str=!str!E:\svn\tesseract\testData\chi.xiaozuan.exp%%i.box 
)
echo !str!
unicharset_extractor.exe !str!
```

11.4. mftraining

```
echo run mftraining
setlocal EnableDelayedExpansion
set time=11
for /l %%i in (0,1,%time%) do (
	set str=!str!E:\svn\tesseract\testData\chi.xiaozuan.exp%%i.tr 
)
echo !str!
mftraining.exe -F font_properties -U unicharset -O chi.unicharset !str!
```

11.5. cntraining

```
echo run cntraining
setlocal EnableDelayedExpansion
set time=11
for /l %%i in (0,1,%time%) do (
	set str=!str!E:\svn\tesseract\testData\chi.xiaozuan.exp%%i.tr 
)
echo !str!
cntraining.exe !str!
```


## 配置说明

### 初次安装

1. 下载官方 [Rime 小狼毫版本安装包](https://rime.im/download/)，并安装到目标机器
2. 下载本方案，将所有文件覆盖到 **Rime** 的用户目录下（最好是先删除原有配置，再全部复制进去）
3. 打开 **font** 文件夹，安装其中的 **黑体字根.ttf** 字体
4. 重新部署 **Rime**，就可以打字了（默认是 86 版五笔方案）

### 切换五笔版本

在本方案的根目录，找到 `tables` 文件夹，该文件夹下的子文件夹 `86/98/06` 分别内置了不同版本的码表，以新世纪版本为例，打开 `tables/06` 文件夹后，将里面所有的内容覆盖到 Rime 用户目录下，重新部署，即可切换为新世纪五笔。

## 基本功能

### 快捷键

|快捷键|功能|
| ---- | ---- |
|ctrl + `|菜单项|
|ctrl + .|中英文标点切换|
|ctrl + shift + h|拆分显示与隐藏|
|ctrl + shift + j|编码提示与隐藏|
|ctrl + shift + f|繁简转换|
|ctrl + shift + u|生僻字显示与隐藏|
|ctrl + shift + d|切的为单字流模式|
|shift|中英文切换|
|shift + space|全角半角切换|
|shfit + delete|删除自造词|

### 功能键

|功能键|功能|
| ----- | ---- |
|help|帮助菜单|
|mode|模式切换菜单|
|date|显示当前日期|
|time|显示当前时间|
|week|显示当前星期|
|nl|显示当前农历|
|jq|显示当前二十四节气|
|z|反查，重复上次上屏内容|
|`|自造词|
|~|以形查音|
|=|激活计算器功能和大小写金额转换功能|
|==|激活农历反查功能|

### 系统日期显示

以下功能默认处于开启状态。

|编码|说明|
| ----- | ---- |
|date|输出当天日期|
|time|输出当前时间|
|week|输出当前星期|

除了以上三个编码可以输出系统时间外，另添加以下关键词，用以输出对应的日期与日间。  
这些关键词包括：【今天、昨天、前天、明天、后天、时间、本周、上周、下周、本月、上月和下月】。  
其中【上月】和【下月】输出的结果如果月末天数不同时按最小月末数计算。

## 常用自定义配置

*以下配置说明按 `wubi.shema.yaml` 方案作为参考，其同样适用于 `wubi_dz.shema.yaml` 等其他方案的配置。*  
**以下所有的配置，都推荐在 `方案.custom.yaml` 中以补丁的方式配置（如果没有对应的 `.custom.yaml` 文件，请自行创建），以避免在更新项目的时候导致个人配置信息丢失！**  
*`方案.custom.yaml` 中的补丁写法，请参照 [定制指南（初阶）](https://github.com/rime/home/wiki/CustomizationGuide)。*  


**注意：在 `custom` 文件首行一定要添加一行注释，避免 `Rime` 解析出错。**

### 自定义候选项个数

默认的候选个数是 `5` 个。文件位置 `wubi.custom.yaml`。

```yaml
# wubi shema setting

patch:
  "menu/page_size": 9
```

### 自定义候选序号样式

默认的序号格式是带圆圈的中文数字符号。文件位置 `wubi.custom.yaml`。

```yaml
# wubi shema setting

patch:
  "menu/alternative_select_labels": [ 〡, 〢, 〣, 〤, 〥, 〦, 〧, 〨, 〩, 〸, 〹, 〺 ]
```

### 四码唯一自动上屏

文件位置 `wubi.custom.yaml`

```yaml
# wubi shema setting

patch:
  "speller/auto_select": true
```

### 回车清空编码

默认情况下，回车会将编码上屏。如果需要设置为回车清空编码，可以按下方法进行修改。

文件位置 `wubi.custom.yaml`

```yaml
# wubi shema setting

patch:
  key_binder/bindings/+:
    - {accept: Return, send: Escape, when: composing}
    - {accept: Return, send: Escape, when: has_menu}
```

### 空码时自动清除编码

文件位置 `wubi.custom.yaml`，本例中，节点有引号和无引号的写法都是可以被正确解析的。

```yaml
# wubi shema setting

patch:
  speller/max_code_length: 4   
  "speller/auto_clear": max_length
```

### 开启自动调频

自动调频功能默认是关闭状态，如需要开启自动调频功能，可参考以下方法。

文件位置 `wubi.custom.yaml`

```yaml
# wubi shema setting

patch:
  translator/enable_user_dict: true
```

### 开启自动造词

自动造词需要满足以下条件：
1. 禁用四码唯一自动上屏功能；
2. 句子输入模式开启；
3. 开启用户词典功能；

文件位置 `wubi.custom.yaml`，此时，将同时开启自动调频、自动造词功能。

```yaml
# wubi shema setting

patch:
  speller/max_code_length: 0   # 最长编码长度，0表示不设置长度
  speller/auto_select: false  # 关闭自动上屏
  translator/enable_sentence: true  # 开启句子输入模式（连打模式）
  translator/enable_user_dict: true  # 启用用户词典
  translator/enable_encoder: true  # 启用编码器
```

### 顶字上屏功能

顶字上屏功能简称「顶功」，这种模式需要固定最长编码为 `4`，并且禁用**四码唯一时自动上屏功能**，配置如下。

文件位置 `wubi.custom.yaml`。

```yaml
# wubi shema setting

patch:
  speller/max_code_length: 4   # 最长编码长度，0表示不设置长度
  speller/auto_select: false  # 关闭自动上屏
```

### 外观文字横向排列

以 windows 端为例，默认外观的文字是竖向排列的，如果想将其修改为横向排列，可以 `weasel.custom.yaml` 中进行修改，这个文件一般是在部署时自动生成的，如果没有该文件，请手动创建一个。

```yaml
# weasel.custom setting

patch:
  "style/vertical_text": false  # 是否启用文本纵向显示
```

#### 排序说明

一简：首选项为一简单字，次级选项为键名汉字。  
二简：首选项为二简单字，次级选项为二级次全码单字。  
三简：首选项为三简单字，次级选项为三级次全码单字。候选的单字按常用字字频做了排序，并且，如果三简中的单字在一简二简首选位置出现过，则退让到最后一位。  
四全：四全码的时候会涉及到字与词，以及单字的字位问题，有的人喜爱打词，有的人喜爱打单，有的人两者兼顾。因此四码的时个不采取让位的排序规则，而是按大数据的词频进行排序，将最常用的结果按最大使用概率反馈给用户。  
*生僻字不参与以上排序逻辑，生僻字需要打全编码显示，并且永远排在最后一位。*

#### 重码率的统计

在网上查看了很多关于五笔词库重码率的统计，感觉都很片面。因为一旦涉及到三版词库的比较，必须要控制变量，被比较的结果才是一个相对可信的值。  
空山词库统一了单字及词组的总数，因此可以在字词数量相等的条件下，对三版词库的编码进行可信的统计计算。  
以下是对三版词库单字重码率的统计结果。  
**可能会颠复你的认知❗❗❗**

|版本|单字总数|含生僻字重码个数|去生僻字总数|去生僻字重码个数|全单字重码率|去生僻字重码率|
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|86|27529|9830|6763|512|35.71%|7.57%|
|98|27529|9661|6763|515|35.09%|7.61%|
|新世纪|27529|10060|6763|531|36.54%|7.85%|

### 在五笔中使用辅助码

如果您经常因为无法正确地判断一个字的结构而影响了打字的心流体验，那么使用辅助码替代结构识别码将帮助您在打字事业上更进一层楼。

不足四码的单字最后一码需要提供一个结构码以作识别，而辅助码则是将结构码换成该字读音的首字母，例如，「去、云、支」的全码都是`fcu`，最后一码`u`表示捺区的上下结构编码；而使用辅助码时，「去、云、支」的编码分别是`fcq、fcy、fcz`。如此一来，您将不必再关注单字的字形结构，利用字音即可完成对单字的定位。

当然，这不是一个完美的方案。五笔使用经典的结构码时，可以在不知道一个字的读音下，完成该字的检索与输入，但如果您不能正确地按笔画书写该字，则无法完成正确的输出。而使用辅助码时，则可能因不懂该字的读音而无法正确地输出；诚然，这是一个弊端。那么，使用经典的结构识别码还是使用辅助码，要看您自身的需求——**这很考验您是否真正地掌握了五笔的精髓。**

另外值得一提的是，因为传统五笔每个区位有三个结构码，一共有十五个；而辅助码通过声母与部分元音韵母的结合，一共有二十三个，在同等词库的条件下，重码率可降低百分之一。但辅助码的本意并不是为了降低重码率，而是提供一种区别于结构识别的机制，以供用户选择。

![](images/%E9%9F%B3%E8%BE%85.png)

您可以下载[Rime-五笔-音辅版](https://gitee.com/hi-coder/rime-wubi-yinfu)，然后将下载好的内容替换到本方案的目录下（一定要替换，而是不是覆盖哦），或者，通过**中书君**进行一键配置与切换。

### 使用快符，更流畅地写作

快符通过**分号键**与字母键结合引导符号上屏的一种快捷功能。本方案内置的快符如下图所示：

![](images/%E5%BF%AB%E7%AC%A6.png)

可通过修改`quick_symbols.dict`文件进行自定义快符。 


## 本项目基于 中书君 项目修改

中书君是针对本方案的一个管理器，旨在帮助广大用户简化 Rime 方案的配置难度。（目前中书君项目还处于 alpha 的内研阶段，beta 正式版还未正式上线。）以下是图示：

项目体验地址：
- Gitee 地址：https://gitee.com/hi-coder/WubiMaster
- Github 地址：https://github.com/mrshiqiqi/WubiMaster

## 


## 有问有答

*问：我部署了 `98` 版码表文件，但想用 `86` 的外观主题，请问怎么配置？*

> 答：在 `tables` 目录的子文件下，含有不同版本的码表文件及一个外观主题文件 `weasel.yaml`，可以将 `tables/86/weasel.yaml` 替换到用户目录下，重新部署，即可使用 `诗意之春` 主题。

*问：更换外观主题后重新部署，但主题还是 `86` 方案默认的绿色主题，这个问题怎么解决？*

> 答：本项目默认不提供任何 `.custom.yaml` 配置文件，因此，在部署完成后，用户目录下会生成一个空的 `weasel.custom.yaml` 文件。解决方法有两种，一种是在这个文件首行添加一行注释，即以 `#` 开头的文字描述，另一种是将这个文件删除。之后再重新部署，新更新的主题即可显现。

*问：我喜欢用单字流方式打字，请问本该案支持吗？*

> 答：支持。可以使用快捷键 `ctrl + shift + d` 切换为单字流模式。

*问：已经安装了字根字体，但注释还是乱码，请如如何处理？*

> 答：这个是由于字根字体编码集原因导致的（新的字根字体正在加工中），目前如果遇到这样的情况，需要您将 Windows 的编码由 GBK 模式切换为 Unicode 模式。
> 具体操作：通过 控制面板>>时间和语言>>语言>>管理语言设置>>管理>>更改系统区域设置>>选择 beta 版本前面的对勾>>确认重启电脑。

## 相关链接

### 项目镜像地址
- Gitee 地址：https://gitee.com/hi-coder/rime-wubi
- Github 地址：https://github.com/myshiqiqi/rime-wubi

### Rime 输入法引擎
- Rime 官网：https://rime.im
- Rime 下载地址：https://rime.im/download
- Rime 帮助文档：https://rime.im/docs
- 小狼毫项目地址：https://github.com/rime/weasel
- 鼠须管项目地址: https://github.com/rime/squirrel
- 中州韵项目地址：https://github.com/rime/ibus-rime
- 小狼毫外观配置说明文档：https://github.com/rime/weasel/wiki
- 小狼毫配色工具：https://bennyyip.github.io/Rime-See-Me
- 鼠须管配色工具：https://gjrobert.github.io/Rime-See-Me-squirrel

### 中书君项目
- Gitee 地址：https://gitee.com/hi-coder/WubiMaster
- Github 地址：https://github.com/myshiqiqi/WubiMaster

### Rime-五笔-音辅版 
- Gitee 地址：https://gitee.com/hi-coder/rime-wubi-yinfu
- Github 地址：https://github.com/myshiqiqi/rime-wubi-yinfu
---


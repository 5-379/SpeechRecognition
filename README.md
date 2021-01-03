# SpeechRecognition

语音识别是人工智能领域的一个重要分支，近年来，语音识别在生活中的应用越来越广泛，例如语音助手、语音翻译、歌曲识别等等。

该项目计划实现一个中文语音识别系统，系统接收一段中文语音输入，输出其对应的文本信息。

## 目录

- [对课题的理解](#对课题的理解)
    - [总体思路](#总体思路)
- [项目的系统流程](#项目的系统流程)
- [重点概念/难点](#重点概念/难点)
    - [数据集](#数据集)
    - [声学模型](#声学模型)
    - [CTC](#CTC)
        - [损失函数](#损失函数)
        - [CTC特点](#CTC特点)
    - [语言模型](#语言模型)
        - [隐马尔可夫模型](#隐马尔可夫模型)
        - [维比特算法](#维比特算法)
- [参考文献](#参考文献)


## 对课题的理解

### 总体思路

- 查阅相关资料文档，学习相关知识
- 收集数据集
- 处理输入数据(将输入的语音转为频谱图)
- 构建语音模型(Speech Model)(CNN + LSTM/GRU + CTC)
    - Input： 声学特征MFCC
    - Output： 汉语拼音
- 构建语言模型(Language Model)
    基于概率统计的最大熵隐马尔可夫模型
    - Input： 汉语拼音序列
    - Output： 对应的汉字文本

## 项目的系统流程

1. 特征提取
2. 声学/语音模型
3. CTC 解码
4. 语言模型

## 重点概念/难点

### 数据集

目前互联网上关于中文语言识别的开源数据集十分丰富，有的数据集包括了各种各样的语料库，但是选择的时候必须要注意基于应用场景去选择。例如，若是医疗诊断语音助手往往选择医疗领域的语料库。

具体到该项目，暂时选择普通人平时说话场景下的语料库。

* **清华大学THCHS30中文语音数据集**

  data_thchs30.tgz [OpenSLR国内镜像](<http://cn-mirror.openslr.org/resources/18/data_thchs30.tgz>) [OpenSLR国外镜像](<http://www.openslr.org/resources/18/data_thchs30.tgz>)
* **Free ST Chinese Mandarin Corpus** 

  ST-CMDS-20170001_1-OS.tar.gz [OpenSLR国内镜像](<http://cn-mirror.openslr.org/resources/38/ST-CMDS-20170001_1-OS.tar.gz>) [OpenSLR国外镜像](<http://www.openslr.org/resources/38/ST-CMDS-20170001_1-OS.tar.gz>)

### 声学模型

通过查阅网上资料，目前比较明确的技术方向是 CNN + LSTM/GRU + CTC

声学模型本质上一个神经网络结构，接收的输入为声学特征MFCC，输出为对应的汉语拼音序列

### CTC

CTC 全称Connectionist temporal classification，是一种常用在语音识别、文本识别等领域的算法，用来解决输入和输出序列长度不一、无法对齐的问题。

#### 损失函数

等我看懂了再补上

#### CTC 特点

1. 条件独立：CTC的一个非常不合理的假设是其假设每个时间片都是相互独立的，这是一个非常不好的假设。在OCR或者语音识别中，各个时间片之间是含有一些语义信息的，所以如果能够在CTC中加入语言模型的话效果应该会有提升。
2. 单调对齐：CTC的另外一个约束是输入 ` X ` 与输出 ` Y ` 之间的单调对齐，在OCR和语音识别中，这种约束是成立的。但是在一些场景中例如机器翻译，这个约束便无效了。
3. 多对一映射：CTC的又一个约束是输入序列 ` X ` 的长度大于标签数据 ` Y ` 的长度，但是对于 ` Y ` 的长度大于 ` X ` 的长度的场景，CTC便失效了。

### 语言模型

语言模型接收的输入为汉语拼音，产生的输出为对应的中文汉字。

语言模型的实现使用基于概率统计的最大熵隐马尔可夫模型。

#### 隐马尔可夫模型

将一段汉语拼音转换为对应中文文本，我们可以假设，每一个拼音将产生多个汉字输出，而一个输出的汉字只能对应一个拼音。例如 wo ai zhong guo可以产生：沃埃种郭、我哎中过、我唉钟果、我爱种果、我爱中国等等。。

隐马尔可夫模型就是基于概率统计的方法去选择这些结果中，最有可能是一个句子的句子，即选择最大概率判断是句子的那个输出。例如上面的例子中，我爱中国和我爱种果这两个输出是句子的概率是最大的。


#### 维比特算法

## 参考文献

[1] Connectionist Temporal Classification : Labelling Unsegmented Sequence Data with Recurrent Neural Networks. Graves, A., Fernandez, S., Gomez, F. and Schmidhuber, J., 2006. Proceedings of the 23rd international conference on Machine Learning, pp. 369--376. DOI: 10.1145/1143844.1143891

[2] Sequence Modeling with CTC. Hunnun, Awni, Distill, 2017

# 海事 海商法 中文命名实体识别 数据集
原始数据集来自于 **南京信息工程大学 鲍闯** 硕士论文
```
鲍闯.基于BERT的中文长文本分类算法研究[D].南京信息工程大学,2022.DOI:10.27248/d.cnki.gnjqc.2022.000077.
```

## 数据清洗

1. 在原始数据的基础上，原则上只去除空格、占位符后，将html文本转化为普通文本。

2. 考虑到裁判文书的文本普遍较长，将对文本进行拆分，使用【，。】拆分后，大部分文段的长度小于32，因此去除大于32长度的文本。

有文献提到对法律类文本处理的方式，以后再可考虑进行优化：
> 在处理预训练数据时，基于自建的司法文书结构化引擎标识出了司法文书各类段落（如诉情、辩称、事实认定、审理经过等等），接着针对不同的司法文本内容采取了不同的采样规则。

3. 此时，数据集大约有1600万条[百度云-待上传](https://github.com/StanleySun233)，取处理好的数据集前100,000条作为训练集，后20,000条作为测试集，再后20,000条作为验证集，作为tiny数据集。

4. 使用[RaNER](https://modelscope.cn/models/iic/nlp_raner_named-entity-recognition_chinese-large-generic/summary)@[AdaSeq](https://github.com/modelscope/AdaSeq) 对数据集进行粗标注，格式为：BIO，使用空格分隔。

RaNER的环境配置如下：
|Torch | Python| OS | Cuda |
|-|-|-|-|
|1.12.1|3.8|ubuntu20.04|11.6|

shell命令：
```shell
pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu116
pip install adaseq -i https://pypi.tuna.tsinghua.edu.cn/simple/
```
标注实体包含6类：

| 实体类型   | 英文名 |
|------------|--------|
| 公司名     | CORP   |
| 创作名     | CW     |
| 其他组织名 | GRP    |
| 地名       | LOC    |
| 人名       | PER    |
| 消费品     | PROD   |

参考样式：
```
阳 B-LOC
光 I-LOC
国 I-LOC
际 I-LOC
中 I-LOC
心 I-LOC

法 O
定 O
代 O
表 O
人 O
： O
谭 B-PER
斌 I-PER
```


## Baseline & Benchmark
海事海商法中文命名实体识别数据集将用于毕业论文，待毕设完成后将更新benchmark和baseline。若有余力，更新maritime-law-ner-base和maritime-law-ner-full数据集。

### Benchmark

|数据集|训练集|测试集|验证集|
|-|-|-|-|
|maritime-law-ner-tiny|100k|20k|20k|
|maritime-law-ner-base|TBD|TBD|TBD|
|maritime-law-ner-full|TBD|TBD|TBD|


### Baseline
Acc,Pre,Rec,F1使用百分比评估，Loss函数为CRF的损失函数。

1. maritime-law-ner-tiny

|模型名|Acc|Pre|Rec|F1|Los|
|-|-|-|-|-|-|
|[BiLSTM-CRF](https://github.com/zjy-ucas/ChineseNER)|85.62|67.20|62.76|64.90|0.21|

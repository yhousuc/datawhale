# task04

# 基于深度学习的文本分类

深度学习：提取特征+完成分类

- 学习使用FastText
- 学会使用验证集进行调参 通过sklean进行了相应的实践，方法都或多或少存在一定的问题：
- 转换得到的向量维度很高
- 需要较长的训练实践；
- 没有考虑单词与单词之间的关系，只是进行了统计 与这些表示方法不同，深度学习也可以用于文本表示，还可以将其映射到一个**低维**空间。 其中比较典型的例子有：FastText、Word2Vec和Bert。 在本章我们将介绍FastText，将在后面的内容介绍Word2Vec和Bert

## FastText

FastText是一种典型的深度学习词向量的表示方法，它非常简单通过Embedding层将单词映射到稠密空间，然后将句子中所有的单词在Embedding空间中进行平均，进而完成分类操作。

所以FastText是一个三层的神经网络，输入层、隐含层和输出层。

**FastText在文本分类任务上，是优于TF-IDF的：**

- FastText用单词的Embedding叠加获得的文档向量，将相似的句子分为一类
- FastText学习到的Embedding空间维度比较低，可以快速进行训练

```
import pandas as pd
from sklearn.metrics import f1_score
import fasttext
train = pd.read_csv('./train_set.csv', sep = '\t', nrows=30000)
# 转换为fasttext需要的格式
train['label_ft'] = '_label_' + train['label'].astype(str)
train[['text', 'label_ft']].iloc[:-5000].to_csv('train.csv', index=None, header=None, sep='\t')

model = fasttext.train_supervised('train.csv', lr=1.0, wordNgrams=2, verbose=2, minCount=1, epoch=25, loss="hs")

val_pred = [model.predict(x)[0][0].split('_')[-1] for x in train.iloc[-5000:]['text']]
print(f1_score(train['label'].values[-5000:].astype(str), val_pred, average='macro'))
```
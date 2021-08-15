Summer2021-No.94 在openeuler社区孵化生物信息单细胞领域开源应用软件
================================================



介绍
--------
[https://gitee.com/openeuler-competition/summer-2021/issues/I3J85G](https://gitee.com/openeuler-competition/summer-2021/issues/I3J85G)

软件架构
------------

在这里，我们提出一个集成了三个深度学习算法框架的模型，用于预测TCR_pMHC分子的结合强度。该模型在独立测试数据集上获得了出色的性能，从而使免疫原性T细胞表位的可靠鉴定成为可能。

安装教程
------------

下载相关代码：
  ```sh
  git clone https://gitlab.summer-ospp.ac.cn/summer2021/210010423.git
  ```
这个模型可以通过这种方式安装(请确保你的python版本是3.7):

    # If needed:
    pip install -r requirements.txt
    #or
    conda install --yes --file requirements.txt

注意，代码依赖于'numpy'， 'tensorflow'和其他包。所以请务必安装“requirements.txt”文件中的包及其对应的版本。如果不安装它们，运行很可能会失败。有关更多信息，请参见:

 + [NumPy](http://www.numpy.org/): Library for efficient matrix math in Python
 + [tensorflow](https://tensorflow.google.cn/): An end-to-end open source machine learning platform in Python
 
文件介绍
--------

data 文件夹
--------

我们从VDJdb, IEDB和TetTCR-seq中收集了经过实验验证的抗原肽与tcr相互作用的数据，用于构建高质量的基准数据集。   
根据TCR-CDR3 α链和β链分别划分为训练集、测试集和两个独立测试数据集具体如下:   
1)TCRA_train.csv和TRB_Train.csv是训练集。   
2)TCRA_test.csv和TCRB_test.csv是测试集。   
3)TCRA_COVID-19.csv和TCRB_COVID-19.csv是两个独立测试集。   
4)TRA-VDJdb_TCR cross-reactivity.rar 和 TRB_VDJdb_TCR cross-reactivity.rar 用于评价模型对抗原肽与tcr单链相互作用的预测能力。   
5)TCRAB_IEDB.csv 用于评价预测抗原肽肽- tcr-CDR3αβ相互作用的集成模型。   


pca 文件夹
--------
该文件夹包含了使用PCA编码生成特征数据集的方法。    
我们将每个样本的抗原肽和TCR序列分别填充到最大长度20，并使用PCA编码。并使用主成分分析(PCA)编码。  
对于每个氨基酸，我们选择前20个PCs解释95%以上的总数据变异，并使用8-20个PCs分别生成不同的数值来代表其生化特征。


code 文件夹
--------
包含了特征提取、五折交叉验证、模型构建与训练的源代码。   
1)“fold”文件夹中的代码用于五折交叉验证来选择合适的特征和寻找最优的超参数组合。   
2)“train”文件夹中的源代码用于构造和训练三个深度学习算法预测器。   
3)使用代码(XXX_Feature_Extraction.py)实现特征编码。   

使用说明
----------

#### 在GPU或CPU上运行

软件安装完成后，TensorFlow会随软件一起安装。参考Keras文档配置TensorFlow在GPU/CPU上运行。   
注意，如果你想使用GPU，你还需要安装CUDA和cuDNN，请参阅他们的网站以获得教程。

### 对于想要使用自己的数据进行训练和预测的用户：

#### 自定义训练模型::
用户使用自己数据进行模型训练:
```sh
python Train_Test_Onehot_Chem_Feature_Extraction.py
python Train_Test_PCA_Feature_Extraction.py
```


然后使用文件夹 /code/fold中的代码进行5次交叉验证，以筛选出最好的特征编码方式和超参数寻优。
```sh
#example
python CNN_A_fold_onehot.py
```
最后使用文件夹 /code/train中的代码多模型进行训练

```sh
#example
python CNN_A_ALL_onehot.py
```




### **于雷 2020180010**

# 任务定义

采用 RNN 为小 Baby 起个英文名字吧 神经网络语言模型，即通过神经网络，计算一项自然语言（例如一条句子） 的出现概率，或者根据上文中的词推断句子中某个词的出现概率。例如，下图采 用了一个具有一个输入层、一个隐藏层和一个输出层的 MLP 网络，建模三元文法 模型： 

![image-20201226194126136](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226194126136.png)

本作业提供了 8000 多个英文名字，试训练一个环神经网络语言模型，进而 给定若干个开始字母，由语言模型自动生成后续的字母，直到生成一个名字的结 束符。从模型生成的名字中，挑选你最喜欢的一个，并采用一种可视化技术，绘 制出模型为每个时刻预测的前 5 个最可能的候选字母。 

Tips：事实上，你也可以给定结尾的若干个字母，或者随意给出中间的若干 个字母，让 RNN 补全其它字母，从而得到一个完整的名字。因此，你也可以尝试 设计并实现一个这样的 RNN 模型，从模型生成的名字中，挑选你最喜欢的一个， 并采用可视化技术，绘制出模型为每个时刻预测的前 5 个最可能的候选字母。

# 数据

## 输入

两个训练数据，包含7954条数据

- Data\male.txt
- Data\female.txt

超参数总迭代次数，打印精度（每隔多少次迭代打印一次损失日志），绘图精度（每隔多少次迭代求和取平均作为一个数据点）。

性别，姓名首字母

## 输出

- 损失数据作图。

100000迭代次数

![image-20201226201051954](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226201051954.png)

1000迭代次数

![image-20201226195919590](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226195919590.png)

100迭代次数

![image-20201226211955656](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226211955656.png)

- 根据这个神经网络模型和输入的（类别，姓名的起始字母）对生成的姓名

  输入首字母O

  - 100000次迭代

    ![image-20201226213036530](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226213036530.png)

  - 10000次迭代

    ![image-20201226213256624](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226213256624.png)

  - 1000次迭代

    ![image-20201226213406473](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226213406473.png)

  - 100次迭代

    ![image-20201226213452074](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226213452074.png)

# 实现方法

类别可以像字母一样组成one-hot向量构成张量输入。

我们将输出作为下一个字母是什么的可能性。采样过程中，当前输出可能性最高的字母作为下一时刻输入字母。

在组合隐藏状态和输出之后我增加了第二个linear层o2o，使模型的性能更好。当然还有一个dropout层，参考这篇论文[随机将输入部分替换为0](https://arxiv.org/abs/1207.0580)给出的参数(dropout=0.1）来模糊处理输入防止过拟合。

我们将它添加到网络的末端，故意添加一些混乱使采样特征增加。

![image-20201226210338364](C:\Users\Allen\AppData\Roaming\Typora\typora-user-images\image-20201226210338364.png)

## 评估

查看并对比不同迭代次数的损失日志输出，很明显能发现最大的区别就是训练时间。可以看出迭代次数越多花费时间越多，所需的电脑性能显然也需要更高，100000次迭代的所需时间是10min左右，而10000次迭代所需的时间为1min左右，相差10倍，如果迭代次数更多，可能相差更多，但是结果表明，并非迭代次数越高结果越好，当达到10000迭代时输出结果已经符合要求。因此选择合适的迭代次数很重要，避免不必要的性能时间浪费。


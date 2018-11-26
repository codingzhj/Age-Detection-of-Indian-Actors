# Age-Detection-of-Indian-Actors
利用CNN将印度演员的图片分类成Young,Middle,Old三个类别
### 2018.11.8
* 将所有训练数据和测试据都reshape成50*50pix的灰度图像，利用mini_batch策略将训练数据输入到CNN中进行训练
* 总结以上用到的pillow方法：
<pre><code>from PIL import pillow
im = Image.open(infile).convert('L')
out = im.resize((50,50),Image.ANTIALIAS)</code></pre>
* 该方法只对训练数据进行灰度处理，训练64k个mini_batch，作为本次探索的baseline，线上F1值为0.7606992164
### 2018.11.13
* 利用isin()方法统计了young，middle，old三类演员在训练集中的占比：
  * 其中young有6706个，middle有10804个，old有2396个
* 不将图像进行灰度处理，只进行统一的resize处理——(50,50,3)，mini-batch传入CNN中进行分类，线上F1值为0.7903857746
### 2018.11.15
* 尝试使用Adam算法替代GD算法，训练速度得到了提升而且loss收敛速度得到了提升，因此只训练了10k个batch
* 线上F1值为0.8126883665得到了微小提升，说明之前的模型欠拟合
### 2018.11.26
* 尝试用两个3 * 3卷积核的卷积层代替一个5 * 5卷积核的卷积层，线上F1有不到0.01的提高
* 尝试增加两个卷积层进行训练，起初设置了与之前相同的batch_size，导致网络训练停滞，难以收敛，之后加大了batch_size，网络很快收敛，并且线上F1有了不到0.01的提升
* 最终提交使用的网络结构是6层卷积，2层全连接，Adam算法，learning_rate设置为1e-4，0.2的随即采样率(mini-batch)，线上F1达到0.8196202532
* 得到的结论包括：
  * 在加深网络的同时要加大batch_size进行训练，因为参数规模变大了，如果batch太小的话难以有效更新参数，导致学习效果反而更差
  * 相同网络架构下，盲目的增加batch_size也是不可取的，也会导致训练周期增加，准确率下降

# Age-Detection-of-Indian-Actors
利用CNN将印度演员的图片分类成Young,Middle,Old三个类别
### 2018.11.8
* 将所有训练数据和测试据都reshape成50*50pix的灰度图像，利用mini_batch策略将训练数据输入到CNN中进行训练
* 总结以上用到的pillow方法：
<pre><code>from PIL import pillow
im = Image.open(infile).convert('L')
out = im.resize((50,50),Image.ANTIALIAS)</code></pre>
* 该方法只对训练数据进行灰度处理，训练64k个mini_batch，作为本次探索的baseline，线上F1值为0.7606992164

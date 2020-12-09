# CRNN-learning-notes
I'll write down some tips and questions here.
CRNN踩坑文档
首先我们学习了crnn的基本原理，此处略过不写。采用了一个github上的代码，进行理解和使用。
	文中给出了一个demo，使用预训练模型进行识别测试。以下是项目的基本文件路径：
环境：
使用tf2.3   pyynml库  




运行demo:
输入文档给出的代码：python crnn/demo.py -i example/images/ -t exmaple/table.txt -m PATH/TO/MODEL
就会发生：
 
需要把images后面的/去掉。
正确代码是：
python crnn/demo.py -i example/images -t example/table.txt -m model/exported_model.h5

必须使用terminal运行！！！
使用pycharm自带运行无法成功
运行成功如图所示：
 


对demo的修改：
首先：我想取消tensorflow的通知信息，很烦人。
Import os和os那句话用于调整输出等级。
 
但是这样没用！不知道原因，尚待分析。
我还想把argparse去掉。因为这玩意不像我所能写出来的。经过print，发现其变量就是一个相对路径。直接将所有args的变量全部替换成相对路径。
成功！



分析decode过程：

 
从上图和图片长度看出，inputs.shape[1]与图片的长度成正比，代表竖向切片的数量。其作为logit_lenth进入sequence_lenth，使用greedy算法进行解码。










解码后，使用sparse_to_dense函数进行转化。
这函数输出的值就是0-40的一个数，每个数都是对应字母或者数字的标签。例如A就对应10. 

 
转化后，使用map2string转化为输出的字符。
 

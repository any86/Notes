初学py, 记录下他特殊的语法, [内置函数参考](https://www.runoob.com/python3/python3-built-in-functions.html)

## 设置pip源
    单次修改

我们可以直接在 pip 命令中使用 -i 参数来指定安装包的来源，如下命令：
```
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```
-i 参数后面的地址可以替换成其他的源的地址

永久修改默认源

在C:\user目录下有个与你登录名同名的目录，比如你登录电脑用xiaoming这个用户，这个目录的路径就是C:\user\xiaoming

在这个目录下新建一个目录pip，他的路径就是C:\user\xiaoming\pip

然后用浏览器打开这个路径，在空白处点击右键--新建--文本文档，新建一个pip.txt

输入一下内容：

[global]

index-url = https://pypi.tuna.tsinghua.edu.cn/simple

[install]

trusted-host = pypi.tuna.tsinghua.edu.cn

最后修改pip.txt的扩展名为pip.ini

  其他国内镜像源

中国科学技术大学 : https://pypi.mirrors.ustc.edu.cn/simple

豆瓣：http://pypi.douban.com/simple/

阿里云：http://mirrors.aliyun.com/pypi/simple/ 作者：速读时代 https://www.bilibili.com/read/cv21563507/ 出处：bilibili

## 设置minconda源
```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```


## 列表生成式
```python
c = [i*2 for i in [3,21,67]]
print(c)
# [6, 42, 134]
```
"[]"里分2部分,后面的循环决定数组内容的长度, 前面是数组每一位的内容.


## zip
```python
a = [1,2,5]
b = [3,21]
c = zip(a,b);
d = list(c)
print(d)
# [(1, 3), (2, 21)]
```

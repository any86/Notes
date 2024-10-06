初学py, 记录下他特殊的语法, [内置函数参考](https://www.runoob.com/python3/python3-built-in-functions.html)

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

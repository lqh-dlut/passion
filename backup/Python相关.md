### open()函数
`open('path/file.format', encoding='格式[gbk]', '打开模式[r]')`

#### 打开模式
```
# r 只读，默认模式，不存在则抛出错误
# rb 二进制打开只读
# r+ 读写，指针在文件开头
# w 只写，如果存在则覆盖原文件，否则创建新文件
# a 追加，如果存在则在结尾追加内容，否则创建新文件
```

open后要`f.close()`，或 ` with open() as f:`

操作之后read如果没有内容可能是**指针问题** read前把指针放在开始位置 ` f.seek(0) `

### lambda
` fun1 = lambda x,y[参数]: x+y[执行内容]

### copy 与 deepcopy

赋值 是在同一个内存空间
copy（浅拷贝）只复制一层，子集内的是同一个内存空间
deepcopy深拷贝，完全重建一个集合

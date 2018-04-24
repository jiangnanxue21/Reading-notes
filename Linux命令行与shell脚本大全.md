## sed编辑器

1. 在命令行定义编辑器命令

```
# sed将命令应用到STDIN输入流上，新的数据输出到STDOUT
echo "This is a test" | sed 's/test/not test/'
```


2. 在命令行使用多条命令

```
echo "This is a test" | sed -e 's/test/not test/; s/test/not test/'
```

3. 在文件中读取

```
cat test.sed
    s/test/not test/
    s/test/not test
echo "This is a test" | sed -f test.sed
```

4. sed的参数

    **g,p,w**

    以及可以使用如!代替/ s!test!not test!

5. 如果只想将命令作用于特定行或某些行，则必须用行寻址（line addressing）。

- 以数字形式表示行区间

  *[address] command*
```
  sed '2,${         # 第2到最后一行
    s/fox/elephant/
    s/dog/cat/
  }' data1.txt
```

- 用文本模式来过滤行
```
  sed '/Samantha/s/bash/csh/' /etc/passwd
```
- 删除行
```
  sed '1,$' data1.txt  # 第一到最后一行
  sed '/1/,/3/d' data1.txt # 在第一个模式，1处打开删除功能，在3处关闭
```

6. 插入，附加和修改行

格式:
*sed '[address] command new line'*
```
# insert
echo "Test Line 2" | sed 'i\Test Line 1' #将Test Line 1插入到Test Line 2前面，因此输出如下

Test Line 1
Test Line 2

# append
sed '$a\This is a new line.' data1.txt #理解和insert的不同

This is line number 4.
This is a new line of text.
```

## gawk程序

1. 从命令行读取程序脚本

```
gawk '{print "Hello World"}' # 由于gawk命令行假定脚本是单个文本字符串，你还必须将脚本放到单引号中
```

要终止这个gawk程序，则必须表明数据流已经结束。
Ctrl+D组合键会在bash中产生一个EOF字符

2. 在程序中使用多个命令
```
echo "My name is Rich" | gawk '{$4="Christine"; print $0}'
```
3. 综合的小例子
```
$ cat script4.gawk
BEGIN {
    print "The latest list of users and shells"
    print " UserID  \t  Shell"
    print "---------\t-----------"
    FS=":"
}

{
    print $1  "      \t   "   $7
}

END {
    print "This concludes the listing"
}
```

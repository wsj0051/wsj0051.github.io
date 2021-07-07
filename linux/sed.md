# sed命令
sed是一种流编辑器，它是文本处理中非常好的工具，能够完美的配合正则表达式使用，功能不同凡响。
处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处
理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文
件末尾。文件内容并没有改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件，
可以将数据行进行替换、删除、新增、选取等特定工作，简化对文件的反复操作，编写转换程序等。
## 命令格式
sed的命令格式：sed [options] 'command' file(s);  
sed的脚本格式：sed [options] -f scriptfile file(s);
## 选项
 -e ：直接在命令行模式上进行sed动作编辑，此为默认选项;

 -f ：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作;

 -i ：直接修改文件内容;

 -n ：只打印模式匹配的行；

 -r ：支持扩展表达式;

 -h或--help：显示帮助；

 -V或--version：显示版本信息。
## sed常用命令

 a\ 在当前行下面插入文本;

 i\ 在当前行上面插入文本;

 c\ 把选定的行改为新的文本;

 d 删除，删除选择的行;

 D 删除模板块的第一行;

 s 替换指定字符;

 h 拷贝模板块的内容到内存中的缓冲区;

 H 追加模板块的内容到内存中的缓冲区;

 g 获得内存缓冲区的内容，并替代当前模板块中的文本;

 G 获得内存缓冲区的内容，并追加到当前模板块文本的后面;

 l 列表不能打印字符的清单;

 n 读取下一个输入行，用下一个命令处理新的行而不是用第一个命令;

 N 追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码;

 p 打印模板块的行。 P(大写) 打印模板块的第一行;

 q 退出Sed;

 b lable 分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾;

 r file 从file中读行;

 t label if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾;

 T label 错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾;

 w file 写并追加模板块到file末尾;

 W file 写并追加模板块的第一行到file末尾;

 ! 表示后面的命令对所有没有被选定的行发生作用;

 = 打印当前行号;

## sed替换标记

 g 表示行内全面替换;

 p 表示打印行;

 w 表示把行写入一个文件;

 x 表示互换模板块中的文本和缓冲区中的文本;

 y 表示把一个字符翻译为另外的字符（但是不用于正则表达式）;
 
 \1 子串匹配标记;

 & 已匹配字符串标记;
## 实例
替换文本中的字符串：

```
 sed 's/book/books/' file
```
`-n`选项和`p`命令一起使用表示只打印那些发生替换的行：

```
sed -n 's/test/TEST/p' file
```
直接编辑文件选项-i，会匹配file文件中每一行的第一个book替换为books
```
 sed -i 's/book/books/g' file
```
### 全面替换标记g

使用后缀 /g 标记会替换每一行中的所有匹配：
```
 sed 's/book/books/g' file
```
当需要从第N处匹配开始替换时，可以使用 /Ng：
```
 echo sksksksksksk | sed 's/sk/SK/2g' 
 skSKSKSKSKSK
 echo sksksksksksk | sed 's/sk/SK/3g'
 skskSKSKSKSK  
 echo sksksksksksk | sed 's/sk/SK/4g'
 skskskSKSKSK 
```
### 定界符

以上命令中字符 / 在sed中作为定界符使用，也可以使用任意的定界符
```
 sed 's:test:TEXT:g' 
 sed 's|test|TEXT|g' 
```
定界符出现在样式内部时，需要进行转义：
```
 sed 's/\/bin/\/usr\/local\/bin/g'
```
### 删除操作：d命令

删除空白行：
```
sed '/^$/d' file
```
删除文件的第2行：
```
 sed '2d' file
``` 
删除文件的第2行到末尾所有行：
```
 sed '2,$d' file
``` 
删除文件最后一行：
```
 sed '$d' file
``` 
删除文件中所有开头是test的行：
```
 sed '/^test/'d file
``` 
已匹配字符串标记&
正则表达式 \w\+ 匹配每一个单词，使用 [&] 替换它，& 对应于之前所匹配到的单词：
```
echo this is a test line | sed 's/\w\+/[&]/g'
[this] [is] [a] [test] [line]
```
所有以192.168.0.1开头的行都会被替换成它自已加localhost：
```
sed 's/^192.168.0.1/&localhost/' file
192.168.0.1localhost
```
### 子串匹配标记\1
匹配给定样式的其中一部分：
```
echo this is digit 7 in a number | sed 's/digit \([0-9]\)/\1/'
this is 7 in a number
```
命令中 digit 7，被替换成了 7。样式匹配到的子串是 7，\(..\) 用于匹配子串，对于匹配到的第一个子串就标记为 \1，依此类推匹配到的第二个结果就是 \2，例如：
```
echo aaa BBB | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
BBB aaa
```
love被标记为1，所有loveable会被替换成lovers，并打印出来：
```
sed -n 's/\(love\)able/\1rs/p' file
```
### 选定行的范围：,（逗号）
所有在模板test和check所确定的范围内的行都被打印：
```
sed -n '/test/,/check/p' file
```
打印从第5行开始到第一个包含以test开始的行之间的所有行：
```
sed -n '5,/^test/p' file
```
对于模板test和west之间的行，每行的末尾用字符串aaa bbb替换：
```
sed '/test/,/west/s/$/aaa bbb/' file
```
### 多点编辑：e命令
`-e`选项允许在同一行里执行多条命令：
```
sed -e '1,5d' -e 's/test/check/' file
```
上面sed表达式的第一条命令删除1至5行，第二条命令用check替换test。命令的执行顺序对结果有影响。如果两个命令都是替换命令，那么第一个替换命令将影响第二个替换命令的结果。

和 -e 等价的命令是 --expression：
```
sed --expression='s/test/check/' --expression='/love/d' file
```

[更多详情](https://man.linuxde.net/sed)
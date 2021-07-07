# cat命令
## 参数说明
- -n 或 --number：由 1 开始对所有输出的行数编号。
- -b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。
- -s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。
- -v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
- -E 或 --show-ends : 在每行结束处显示 $。
- -T 或 --show-tabs: 将 TAB 字符显示为 ^I。
- -A, --show-all：等价于 -vET。
- -e：等价于"-vE"选项；
- -t：等价于"-vT"选项；

cat命令是linux下的一个文本输出命令，通常是用于观看某个文件的内容的；  
## cat主要有三大功能
1. 一次显示整个文件。
```
$ cat   filename
```
2. 从键盘创建一个文件。只能创建新文件,不能编辑已有文件. 
```
$ cat  >  filename
```
`cat > test.sh << EOF`以EOF作为输入结束，之前存在的内容会被覆盖掉。  
`cat << EOF >> test.sh` 将内容追加到 test.sh 的后面，不会覆盖掉原有的内容  
EOF只是标识，不是固定的
```
cat << HHH > iii.txt
```
输入内容：
```
> sdlkfjksl
> sdkjflk
> asdlfj
> HHH
```
这里的“HHH”就代替了“EOF”的功能。结果是相同的。
引用 `cat iii.txt`可以看到结果：
```
sdlkfjksl  
sdkjflk  
asdlfj  
```

3. 将几个文件合并为一个文件。
将file1 file合并到file
```
$ cat   file1   file2  > file
```
把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：
```
cat -n textfile1 > textfile2
```
把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：
```
cat -b textfile1 textfile2 >> textfile3
```

## 实例

清空 /etc/test.txt 文档内容：
```
cat /dev/null > /etc/test.txt
```
cat 也可以用来制作镜像文件。例如要制作软盘的镜像文件，将软盘放好后输入：
```
cat /dev/fd0 > OUTFILE
```
相反的，如果想把 image file 写到软盘，输入：
```
cat IMG_FILE > /dev/fd0
```
注：
1. OUTFILE 指输出的镜像文件名。
2. IMG_FILE 指镜像文件。
3. 若从镜像文件写回 device 时，device 容量需与相当。
4. 通常用制作开机磁片。

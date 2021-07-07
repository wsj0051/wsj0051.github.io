# README.md 文件 

### 必要的 README.md 文件

`README.md`是 gitbook 最基础的文件之一，它一般用来描述这本书最基本的信息。
它呈现给读者这本书最初的样子，如果内容不够简洁明了，很可能就没有看下去的欲望了。

可以通过 `gitbook init` 自动创建该文件。 



### 使用其他文件替代README.md

一些项目更愿意将 `README.md` 文件作为项目的介绍而不是书的介绍。
大部分代码托管平台将 `README.md` 自动显示到项目首页，如果你不喜欢这样。
从GitBook >2.0.0 起，就可以在 `book.json` 中定义某个文件作为README。

```
{
    "structure" : {
        "readme" : "information.md"
    }
}
```
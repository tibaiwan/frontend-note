<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*
# Markdown 常用语法清单

- [1、标题](#1%E6%A0%87%E9%A2%98)
- [2、段落](#2%E6%AE%B5%E8%90%BD)
- [3、区块引用](#3%E5%8C%BA%E5%9D%97%E5%BC%95%E7%94%A8)
- [4、代码区块](#4%E4%BB%A3%E7%A0%81%E5%8C%BA%E5%9D%97)
- [5、强调](#5%E5%BC%BA%E8%B0%83)
- [6、列表](#6%E5%88%97%E8%A1%A8)
- [7、分割线](#7%E5%88%86%E5%89%B2%E7%BA%BF)
- [8、链接](#8%E9%93%BE%E6%8E%A5)
- [9、图片](#9%E5%9B%BE%E7%89%87)
- [10、反斜杠`\`](#10%E5%8F%8D%E6%96%9C%E6%9D%A0%5C)
- [11、符号'`'](#11%E7%AC%A6%E5%8F%B7)
- [12、表格](#12%E8%A1%A8%E6%A0%BC)
- [13、其它：](#13%E5%85%B6%E5%AE%83)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

Markdown语法主要分为如下几大部分：

> **标题**，**段落**，**区块引用**，**代码区块**，**强调**，**列表**，**分割线**，**链接**，**图片**，**反斜杠 `\`**，**符号'`'**。

## 1、标题
两种形式：  
1）使用`=`和`-`标记一级和二级标题。
> 一级标题   
> `=========`   
> 二级标题    
> `---------`

效果：
> 一级标题   
> =========   
> 二级标题
> ---------  

2）使用`#`，可表示1-6级标题。
> \# 一级标题   
> \## 二级标题   
> \### 三级标题   
> \#### 四级标题   
> \##### 五级标题   
> \###### 六级标题    

效果：
> # 一级标题   
> ## 二级标题   
> ### 三级标题   
> #### 四级标题   
> ##### 五级标题   
> ###### 六级标题

## 2、段落
段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用**两个以上**空格加上回车（引用中换行省略回车）。

## 3、区块引用
在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：
> \> 区块引用  
> \>> 嵌套引用  

效果：
> 区块引用  
>> 嵌套引用

## 4、代码区块
代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。如    
普通段落：

void main()    
{    
    printf("Hello, Markdown.");    
}    

代码区块：

    void main()
    {
        printf("Hello, Markdown.");
    }

**注意**:需要和普通段落之间存在空行。

## 5、强调
在强调内容两侧分别加上`*`或者`_`，如：
> \*斜体\*，\_斜体\_    
> \*\*粗体\*\*，\_\_粗体\_\_

效果：
> *斜体*，_斜体_    
> **粗体**，__粗体__

## 6、列表
使用`·`、`+`、或`-`标记无序列表，如：
> \-（+\*） 第一项
> \-（+\*） 第二项
> \- （+\*）第三项

**注意**：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：
> + 第一项
> + 第二项
> + 第三项

有序列表的标记方式是将上述的符号换成数字,并辅以`.`，如：
> 1 . 第一项   
> 2 . 第二项    
> 3 . 第三项    

效果：
> 1. 第一项
> 2. 第二项
> 3. 第三项

## 7、分割线
分割线最常使用就是三个或以上`*`，还可以使用`-`和`_`。

## 8、链接
链接可以由两种形式生成：**行内式**和**参考式**。    
**行内式**：
> \[我的Markdown库\]\(https:://github.com/Markdown "Markdown"\)。

效果：
> [我的Markdown库](https:://github.com/Markdown "Markdown")。

**参考式**：
> \[我的Markdown库1\]\[1\]    
> \[我的Markdown库2\]\[2\]    
> \[1\]:https:://github.com/Markdown "Markdown"    
> \[2\]:https:://github.com/Markdown "Markdown"    

效果：
> [我的Markdown库1][1]    
> [我的Markdown库2][2]

[1]: https:://github.com/Markdown "Markdown"
[2]: https:://github.com/Markdown "Markdown"

**注意**：上述的`[1]:https:://github.com/Markdown "Markdown"`不出现在区块中。

## 9、图片
添加图片的形式和链接相似，只需在链接的基础上前方加一个`！`。

## 10、反斜杠`\`
相当于**反转义**作用。使符号成为普通符号。

## 11、符号'`'
起到标记作用。如：
>\`ctrl+a\`

效果：
>`ctrl+a`    

## 12、表格
`Name | Academy | score `  
`- | :-: | -: `  
`Harry Potter | Gryffindor| 90 `  
`Hermione Granger | Gryffindor | 100 `  
`Draco Malfoy | Slytherin | 90`  

效果：
Name | Academy | score 
- | :-: | -: 
Harry Potter | Gryffindor| 90 
Hermione Granger | Gryffindor | 100 
Draco Malfoy | Slytherin | 90

## 13、其它：
列表的使用(非traditonal markdown)：

用`|`表示表格纵向边界，表头和表内容用`-`隔开，并可用`:`进行对齐设置，两边都有`:`则表示居中，若不加`:`则默认左对齐。

|代码库                              |链接                                |
|:------------------------------------:|------------------------------------|
|MarkDown                              |[https://github.com/Markdown](https://github.com/Markdown "Markdown")|
|MarkDownCopy                              |[https://github.com/Markdown](https://github.com/Markdown "Markdown")|

TODOLIST 的是否勾选

`- [ ]` 不勾选  
`- [x]` 勾选

- [ ] 不勾选
- [x] 勾选

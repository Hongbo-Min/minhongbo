# Vim

## .命令

.命令可以讓我們重複上次的修改，它是Vim中最爲强大的多面手。

例如我按下dd刪除一行后，按.可以重複dd這個操作

### 以退为进

var foo = "method("+argument1+","+argument2+")";

var foo = "method(" + argument1 + "," + argument2 + ")";

![image-20220714173607458](C:\Users\90617\AppData\Roaming\Typora\typora-user-images\image-20220714173607458.png)

f+是找到光標向後第一個 +号， s把两条命令合并成一条

；命令会重复上次f命令查找的字符

，命令会向反方向f{char}的方向查找

### 查找并手动替换

..We're waiting for content before the site can go live...

...If you are content with this, let's go ahead with it...

...We'll launch as soon as we have the content...

如果我们想把content改为copy 就可以先/content 所有的content会显示高亮 然后运行cw（删除一个单词并进入插入模式）然后输入copy <ESC> 接下来按n会跳到下一个content 自己去判断是否需要变为copy 如果需要按下 . 即可

.范式   一键移动 一键执行

### 构造可重复的修改

当我们的光标在单词的末尾，如果想删除这个单词，我们可以选择将光标移动到这个单词的第一个单词然后dw删除这个单词，

但是这样执行.命令的时候会继续删除单词，而不会从后向前删除

构造可重复的修改意思为如果我们可以直接从最后删除这个单词，那么之后的.操作就可以重复执行上一个操作

### 用次数做简单的算数运算

.blog, .news { background-image: url(/sprite.png); }

.blog { background-position: 0px 0px }

如果我们要将第二行的 .blog改为.news 可以用yyp命令复制一行

cW .news <ESC> 

如果想要把0px改为 -180px 我们可以直接 输入 180<C-x> 代表从光标向后找到第一个数字将它减去180

对应的<C-a>代表加180

**数字的格式**

007的后面是什么？如果对007加1会得到什么结果，可能第一时间想到的是008，但是结果可能会让你诧异。

像某些编程语言中的约定一样，Vim把0开头的数字解释为八进制值，而不是十进制，在八进制体系中 007 + 001 = 010 

如果你经常使用八进制那么，Vim的缺省可能会适合你，但是如果不是那么你可以将这个加入到你的vimrc中

set nrformats = 10？ 这会让Vim把所有数字都当成十进制，不管他们是否以0开头

### 能够重复，就别用次数

Delete more than one word

我们想把这段文字改为 Delete one word ，也就是说，要像这段文字里所讲的那样删除两个单词，有几种方式都可以达到这个目的

一个是 d2w，一个是2dw，d2w理解为调用删除命令然后以2w作为动作命令，可以理解为删除两个单词

而2dw可以理解为做两次删除一个单词的操作，抛开语义不讲，这两种方法都可以得到想要的结果

现在可以考虑第三种方法 dw. 可以理解为删除一个单词然后重复上次的操作



对于次数风格还是重复风格也有同样的争论，怎么做取决于你怎么看保留干净撤销历史记录的价值，以及你是否觉得用次数令人生厌

### 双剑合璧，天下无敌

操作符+动作命令=操作

g~反转大小写

gU转换大写

gu转换小写

假设我们已经知道如何用daw删除一个单词，然后又学到了gU命令（它也是一个操作符），所以我们可以用gUaw把当前单词的转化成为大写字母。

如果我们再扩充一下 例如ap是操作段落，就会发现我们可以进行两个新的操作：用dap删除整个段落，或者用gUap来将整个段落文字转换为大写。

Vim有一个额外规则，即当一个操作符命令被连续调用两次时，它会作用于当前行。所以dd删除当前行，而>>缩进当前行。gU命令是一种特殊情况，既可以用gUgU，也可以用简化的gUU来作用于当前行

**注释**命令以\\{motion}触发，他会切换指定行的注释状态。它是一个操作符命令，因此可以将它与其他的动作命令结合在一起。\\\ap将切换当前段落的注释状态，\\\G会把从当前行到文件末尾间的所有注释切换，而\\\\\则注释当前行

## 插入模式

### 在插入模式中可即时更正错误

按键操作		用途

<C-h>		删除前一个字符（同退格键）

<C-w>		删除前一个单词

<C-u>		删除至行首

这些命令不是插入模式独有的，甚至也不是Vim独有的，在Vim的命令行模式中，以及在bash shell中，也都可以使用它们

### 返回普通模式

<Esc> 切换到普通模式

<C-[>切换到普通模式

<C-o>切换到插入-普通模式

结实插入-普通模式：插入-普通模式是普通模式的一个特例，它能让我们执行完一次普通模式的命令。在此模式中，我们可以执行一个普通模式命令，执行完后，马上又返回到插入模式。

例如：假如我当前行正处于窗口顶部或底部时，有时我会滚动屏幕，以便看到更多的上下文。用zz命令可以重绘屏幕，并把当前行显示在窗口中，这样就能够阅读当前行上下文的半屏内容。可以使用<C-o>zz，在插入模式中出发这条命令。此操作完成后就可以直接到插入模式，因此可以连续不断的输入内容。
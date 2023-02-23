# Word转markdown

## 1.手动修改word（可选）

* 如果标题没有规律需要手动在word里修改

* 一级标题前加一个井号和空格

   ```
   # 
   ```

   * 多个级标题加多个#和一个空格
   * 转换后续用字符串匹配`\#`替换成`#`

* 可去除倾斜（转化后会将倾斜替换成`*斜体*`）

    * 倾斜被错误的使用在代码中调用的常用的方法名和常用的注解参数变量中没有意义
    * 转换后续可用正则匹配`([^#|*|br|\\])\*([^\*|^ ]+)\*`替换成`$1$2`

* 丢失格式

  * 使用在线转换对于文本背景标注的格式会丢失
  * 统一处理成加粗
    * 在word选中文本，样式-选择格式相似的样式-Ctrl+B加粗

## 2.将word转换成md

 * 使用软件转换比如：<https://products.aspose.app/words/zh/conversion/word-to-md>

 * 确保图片和表格能正确转换

## 3.编辑转换后md

* 使用带有正则匹配功能的编辑器打开比如：Nodepad++

* Notepad++换行符匹配问题

  * 使用正则`.\n`可能匹配不到换行符
  * 保存为windows(CR LF)格式的txt时候换行标志为：CR+LF（\r\n）。例如：`hosts`文件
  * 保存为unix(LF)格式的txt时候换行标志为：LF（\n）。例如：`.gitconfig`文件

  ### 3.1处理标题

  * `^(\*\*[0-9] )`替换成`## $1`
  * `^(\*\*[0-9]\.[0-9] )`替换成`### $1`
  * `^(\*\*[0-9]\.[0-9]\.[0-9] )`替换成`#### $1`
  * `^(\*\*[0-9]\.[0-9]\.[0-9]\.[0-9] )`替换成`##### $1`
  * 
  * 注意格式`^(\*\*[0-9]\.[0-9]\.[0-9])`替换成`#### $1`
    * 注意格式`^(\*\*[0-9]\.[0-9]\.[0-9]\.[0-9])`替换成`#### $1`

  * 手动给大标题前加一个井号和一个空格

  ### 3.2处理word的表格显示代码

  * 正则匹配
  * ````
    <br>`([^`]*)`
    ````
  * 替换成`\r$1`
  * 注意正则匹配不要将多个br连在一起匹配
  * 正则匹配`<br>`替换成`\r`
  * 处理`\|Java`和XML、Shell、JSON、#Plain Text
  * 正则匹配`\|(Java|YAML|XML|SQL|JSON|Shell|Bash|Plain Text|HTML)`替换成```$1

  * `\|\r\n\| :- \|`123\r\n```

    * 正则匹配`^\| :- \|$`替换成```
    * 注意不要匹配到表格，示例如下：


  ```java
  |参数|说明|
  | :- | :- |
  |Endpoint|对象存储服务的URL|
  |Access Key|Access key就像用户ID，可以唯一标识你的账户。|
  |Secret Key|Secret key是你账户的密码。|
  
  示例代码如下：
  	
  |Java
  import io.minio.BucketExistsArgs;
  
  public class FileUploader {
    public static void main(String[] args)throws IOException, NoSuchAlgorithmException, InvalidKeyException {
  	
  }|
  | :- |
  ```

    * 最后`\|$`替换成空
  * 慎重替换可先计数后匹配查看再替换
  * `\*\*([a-z|A-Z|-]+)\*\*`替换成`$1`处理代码错误的加粗
  * `\*([a-z|A-Z|-]+)\*`替换成`$1`处理代码错误的斜体
    * 先执行第四步处理特殊符号星号
	* 再扩大范围处理错误的斜体`([^\*])\*([^0-9|\r|\*| ]+)\*([^\*])`替换成`$1$2$3`


## 4.处理特殊符号

* 字符串匹配`\*`替换成`*`

  * 或者`\\\*`替换成`\*`

* 字符串匹配`\_`替换成`_`

  * 或者`\\_`替换成`_`

* 处理错误转换的空格

  * 正则匹配``` `( +)` ```替换成  两个空格（空格太多会被md识别为代码格式），比如：

  ```
  	Timer 的优点在于简单易用，每个
  ```

## 5.使用Typora打开确认

* 使用在线转换，存在特殊符号转换的问题

* 显示\3)，示例：

```
2）master存储了数据文件的元数据，一个文件被分成了若干块存储在多个chunkserver中。

3）用户从master中获取数据元信息，向chunkserver存储数据。

3\) HDFS

HDFS，是Hadoop Distributed File System的简称，是Hadoop抽象文件系统的一种实现。HDFS是一个高度容错性的系统，适合部署在廉价的机器上。
```

* 原：

```
2）master存储了数据文件的元数据，一个文件被分成了若干块存储在多个chunkserver中。
3）用户从master中获取数据元信息，向chunkserver存储数据。

3) HDFS
HDFS，是Hadoop Distributed File System的简称，是Hadoop抽象文件系统的一种实现。
```

* 错误转换效果：阿拉伯数字加英文括号和一个空格会被识别成 多一个反斜杠加数字英文括号
  * （可选操作）个别编号使用数字加英文括号没有意义，可统一换成数字加中文括号把`^\\([0-9])\)`替换成`$1）`
  
* 解决方法正则匹配`^(\\)([0-9])\)`替换成`$2$1\)`
  * 该正则使用要慎重，注意不要匹配到正确的数据
  * 正则把转义给英文括号不然会识别成 后退一步加数字（缺少英文括号）
  
  
* 丢失格式
  * 使用在线转换对于文本背景标注的格式会丢失
  * 后续解决。选中相似样式文本剪切出来，一个一个在md里找到文本进行处理
  * html标签背景色github不支持
  * 
  * 正则匹配`\*\*([^0-9|\r]+)\*\*`找到除了标题的加粗文本
  * `<table><td bgcolor=pink>`找到文本`</td></table>`表格加大内边距块级元素（不支持table里面加粗）
  * `<font style="background: pink">**找到文本**</font>`嵌入文字中行内样式
    * 加粗标记扩大范围处理`([^#][^ ])(\*\*[^\r]+\*\*)`替换成`$1<font style="background: pink">$2</font>`
  * 
  * 比如背景色灰色不能自动转换可用斜体标记`([^\*])\*([^0-9|\r|\*| ]+)\*([^\*])`替换成``$1`$2`$3``
    * 选择相似样式全部复制出来，人工对比起始文本后再替换
    * `^(.*)\*([^\*|\r]+)\*([^\*])`扩大匹配范围替换成`$1$2$3$4`

## 6.最后处理图片

* 在线转换缺点导出图片不是原图

* pandoc缺点不能正确转换表格，表格的代码没有保留空格的格式

* 使用pandoc把图片输出到当前路径的media目录下`--extract-media .`，-i指定word文档，-o输出到md

  ```shell
  pandoc --extract-media . -i 6.docx -o 6.md
  ```

* 

* 正则匹配图片路径不同位数的图片名称

  * `(!\[\]\()Aspose\.Words\.e6f4eae8-6e80-43fc-8de8-694315406177.0([1-9])?0?([0-9]\.png\))`替换成`$1./imgs/chapter6/image$2$3`

  ```shell
  mkdir imgs/chapter6
  mv *.png ./imgs/chapter6
  ```

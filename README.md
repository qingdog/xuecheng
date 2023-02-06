# Word转markdown

## 1.手动修改word

 * 一级标题前加一个井号和空格

   ```
   # 
   ```
  * 多个级标题加多个#和一个空格
  * 全选文字去除加粗（影响md标题识别）、去除倾斜（转化后可能会将倾斜替换成`*斜体*`）

## 2.将word转换成md

 * 使用软件转换比如：<https://products.aspose.app/words/zh/conversion/word-to-md>

 * 确保图片和表格能正确转换

## 3.编辑转换后md

 * 使用带有正则匹配功能的编辑器打开比如：Nodepad++

 ### 	3.1处理标题

  * 字符串匹配`\#`替换成`#`

 ### 	3.2处理word的表格显示代码

* 正则匹配```<br>`([^`]*)` ```替换成`\r$1`
  * 注意正则匹配不要将多个br连在一起匹配

* 正则匹配`<br>`替换成`\r`
* 处理`\|Java`\`替换成`\#XML`、`\#Shell`、`\#JSON`、`\#Plain Text`
    * 正则匹配`\|(Java|YAML|XML|SQL|JSON|Shell|Bash|Plain Text)`替换成```$1
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

## 4.处理特殊符号

* 字符串匹配`\*`替换成`*`

* 字符串匹配`\_`替换成`_`

* 处理错误转换的空格
  * 正则匹配````( +)` ```替换成  两个空格（空格太多会被md识别为代码格式），比如：
  
  ```
  	Timer 的优点在于简单易用，每个
  ```

## 5.使用Typora打开确认

* 未解决的问题

* 显示\3)，示例：

```
2）master存储了数据文件的元数据，一个文件被分成了若干块存储在多个chunkserver中。

3）用户从master中获取数据元信息，向chunkserver存储数据。

\3) HDFS

HDFS，是Hadoop Distributed File System的简称，是Hadoop抽象文件系统的一种实现。HDFS是一个高度容错性的系统，适合部署在廉价的机器上。
```

* 原：

```
2）master存储了数据文件的元数据，一个文件被分成了若干块存储在多个chunkserver中。
3）用户从master中获取数据元信息，向chunkserver存储数据。

3) HDFS
HDFS，是Hadoop Distributed File System的简称，是Hadoop抽象文件系统的一种实现。
```


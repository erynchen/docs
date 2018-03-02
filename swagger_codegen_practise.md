##   SwaggerCodeGen

*一切理论化和概念化的东西网上都能搜到，这里只记录了一个我自己的实践过程*

现在经常用到 yaml, 就势必用到 [swagger 的一套工具](https://swagger.io/docs/swagger-tools/). 
其中 swagger-codegen 可以根据yaml 文件 自动生成代码（可以不用手动写module就值得尝试一下）. 实践如下: 

### 第一部分  基本使用

##### Step1: Java 环境设定

确认 Java 是 1.7 以上版本. 并且在 environment 里有设定: 


![env.png](http://upload-images.jianshu.io/upload_images/210564-acf3222d3acb515c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

system path 里也要设定:

![path.png](http://upload-images.jianshu.io/upload_images/210564-98b44ff3a9d8398b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### Step2: 下载  executable jar 

从 maven直接下载: [swagger-codegen-cli jar](https://oss.sonatype.org/content/repositories/releases/io/swagger/swagger-codegen-cli/2.3.0/)  我下了2.3.0 版.  
如果要自己customize, 可以下载open source源代码, 然后用maven package也很方便. 在第二部分讲.

##### Step3: 把 jar 放在某个目录下

比如: C:\Java\jdk1.8.0_111\lib\swagger.  放在JDK 的lib里 为了集中管理jar, 新建一个swagger 文件夹,因为 接下去 generate 出来的 code都会默认放这个文件夹. 

##### Step4: 运行

CMD 里, 进入之前放jar的文件目录, 运行:
> java -jar swagger-codegen-cli-2.3.0.jar help generate

可以看到有一些help信息显示, 这表示准备工作都正确完成了.

![codegen.png](https://www.amazon.ca/clouddrive/share/isofRepRLHTracPZC6A28v5bx6c1adXFjytKLvY0lVo)



用来运行 codegen的 基本命令格式如下: 

> java -jar swagger-codegen-cli-2.3.0.jar generate -i <path of your Swagger specification> -l <language>

比如你的 yaml 文件在 C:\Dev\git\abc.yaml, 要生成Java 代码, 那么命令行如下: 
>java -jar swagger-codegen-cli-2.3.0.jar generate -i C:\Dev\git\abc.yaml -l java


-----------


###  第二部分  修改源代码

自动生成的代码, 其路径 (或者说package) 默认是这样的:  
`package io.swagger.client.api/module`

我希望尽量减少后期手动修改的内容，把这部分改成我自己需要的路径：
`package com.abc.model;`

这就需要稍微改动一下源代码了，步骤如下： 

##### Step1: 获取open source 源代码

下载[swagger-codegen 源代码](https://github.com/swagger-api/swagger-codegen/tree/v2.3.1), 
解压到某个工作目录下，我放在: C:\...\github\swaggerCodeGen\
然后 import maven project到 eclipse，或者 STS 都可以。

##### Step2: 改代码

修改两个文件，第一个是 主要文件：
>\modules\swagger-codegen\src\main\java\io\swagger\codegen\languages\JavaClientCodegen.java

改 `modelPackage = "io.swagger.client.model";`   成你要的路径。


第二个文件是unit test 文件，和主功能无关，但是不修改的话测试没法过不能package 结束： 
>\modules\swagger-codegen\src\test\java\io\swagger\codegen\DefaultGeneratorTest.java

 把`private static final String MODEL_ORDER_FILE = "/src/main/java/io/swagger/client/model/Order.java";`   改成相应的路径。

##### Step3: package

CMD到源代码目录下: 
> cd C:\...\github\swaggerCodeGen\

> mvn package -X


package success 后，可以在 C:\...\github\swaggerCodeGen\modules\swagger-codegen-cli\target\lib 中找到 ***swagger-codegen-cli.jar***

好了，把这个 jar 再拷贝到你之前的lib目录中（或者任何一个地方，反正不要在源代码里运行吧），按照第一部分的再运行喽，我能得到我想要的 路径 和 文件包啦。

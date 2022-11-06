# Idea使用文档

## 1 idea获取

idea历史版本安装包官网入口：[https://www.jetbrains.com/idea/download/other.html](https://www.jetbrains.com/idea/download/other.html)
目前idea社区版(community)可以免费使用，旗舰版(ultimate)版有一个月试用期。
专业版idea破解可关注微信公众号`星辰之行`，回复关键词`idea`获取
> 仅可破解2021.3.3及以下版本

## 2 idea使用

### 2.1 idea基本设置

#### 2.1.1 idea全局配置

以前不知道idea全局配置时，每次新建maven项目都需要到重新设置maven的目录以及settings文件，浪费了很多的时间。其实idea对maven相关配置是可以进行全局配置的。只需要通过File -> close project关闭所有打开的项目(不是直接右上角关闭)，进入到开始界面，然后Customize -> All setting即可。进入之后和我们普通的setting界面一样操作即可。

![image-20221026224018489](E:\GitBook\Elysia\images\idea\image-20221026224018489.png)   

#### 2.1.2 关闭自动更新

setting->Appearance&Behavior->System Setting->Updates把所有的勾都去掉

![image-20221027233456685](E:\GitBook\Elysia\images\idea\image-20221027233456685.png)  
#### 2.1.3 terminal中使用git bash命令

默认idea的terminal使用的cmd.exe。

![](E:\GitBook\Elysia\images\idea\image-20221027235320480.png)  

我们需要将其修改为本地安装的git的根目录下bin文件夹中的的bash.exe

![](E:\GitBook\Elysia\images\idea\image-20221029180028965.png) 

这样就能在terminal中使用git命令了，并且对于文件的操作可以使用linux命令，如常用的rm命令等。

#### 2.1.4 使用外部程序打开文件

setting->tool->external tool，点击左上角加号，打开如下界面。

![image-20221029180427636](E:\GitBook\Elysia\images\idea\image-20221029180427636.png) 

其中Name和Description自己随便写，注意与要新增的外部程序对应起来就行，Program是外部程序的exe文件；Arguments填写"$FilePath$"即可，其标识外部程序需要打开的文件的路径，working directory会自动填充，其他保持默认设置即可。

使用：选组需要打开的文件，右击在external tools中选择要使用的外部程序即可。

![image-20221029180908990](E:\GitBook\Elysia\images\idea\image-20221029180908990.png) 

#### 2.1.5 设置编码

入口setting->Editor->File encodings，常见设置如下。

> BOM(Byte Order Mark)中文译为"字节顺序标记"。在Windows的一些软件如记事本，在保存UTF-8编码的文件时，会在开头插入三个不可见的字符0xEF 0xBB 0xBF，即BOM。用于让其他编辑器判断该文件是否以UTF-8编码。前面的三个字符其实是**Zero Width No-Break Space**（零宽无间断字符)在UTF-8编码下的表示，在其他编码方式下又有不同，因此编辑器可以根据文件首部的前几个字节确定文件的编码。
>
> 部分语言(如PHP)会把BOM认为是正文的一部分，从而在读取带POM头部的文件会发生异常。

![image-20221029223126475](E:\GitBook\Elysia\images\idea\image-20221029223126475.png) 

#### 2.1.6 热部署

##### 2.1.6.1 springboot热部署

步骤一：setting->Build、Execution、Deployment中勾选Build project automatically

![image-20221029225707878](E:\GitBook\Elysia\images\idea\image-20221029225707878.png)  

步骤二：setting->Advanced Setting勾选以下
![image-20221029225841141](E:\GitBook\Elysia\images\idea\image-20221029225841141.png)  

步骤三：maven中加入以下依赖：

```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
</dependency>
```
这样springboot启动后，idea会自动编译并部署。如果想要手动更新，可以通过Run->Debug action，选择update进行更新(或者ctrl+f10)，如下：

![image-20221029230729991](E:\GitBook\Elysia\images\idea\image-20221029230729991.png) 

##### 2.1.6.2 springMVC本地tomcat启动热部署

步骤一：tomcat选择deployment时选择war exploded形式的artifact。

![image-20221029230909930](E:\GitBook\Elysia\images\idea\image-20221029230909930.png) 

步骤二：设置更新时的动作为update classes and resources。
> 不建议设置下方frame deactivation，因为其会在失去焦点后执行对应的动作。这将在我们最小化、切换窗口等触发对应的操作，而这极大多数情况下是不需要的，浪费计算机资源。

![image-20221029231450258](E:\GitBook\Elysia\images\idea\image-20221029231450258.png) 

步骤三：修改代码后手动更新，更新位置如下：

![image-20221029232553573](E:\GitBook\Elysia\images\idea\image-20221029232553573.png)

#### 2.1.7 将idea的settings文件存放到远程仓库中(本人使用时发现需要梯子)

作为开发人员，无论是工作需要还是换取新工作，经常需要换取新的工作设备，为了更快的设置idea的相关配置(字号，风格等)，可以将idea的settings文件放在远程仓库，步骤如下：
1\.在github上新建一个仓库，用来存放idea的settings文件
2\.idea中file->Manage IDE Setting-> Settings Repository

![image-20221030181051367](E:\GitBook\Elysia\images\idea\image-20221030181051367.png)

3\.填入刚才创建的远程仓库的地址（目前只支持http协议）

![image-20221030181258061](E:\GitBook\Elysia\images\idea\image-20221030181258061.png)

4\.点击Overwrite Remote将settings文件，可以将本地的settings文件覆盖远程仓库的settings文件。idea会弹出如下弹框，我们选择默认选项：在浏览器中登录。

> 当然你也可以选择token的方式，但是这需要在github中新建token，勾选功能时勾选上repo、workflow和write:packages
>
> 注意：本人使用时发现本功能需要使用梯子

![image-20221030182948981](E:\GitBook\Elysia\images\idea\image-20221030182948981.png) 

5\.关闭settings修改后自动同步远程仓库，关闭tools->settings repository的Auto Sync

![image-20221030201814604](E:\GitBook\Elysia\images\idea\image-20221030201814604.png)

### 2.2 idea一些设置的含义

#### 2.2.1 .idea文件夹文件作用

![image-20221029234721573](E:\GitBook\Elysia\images\idea\image-20221029234721573.png)

* compiler.xml文件夹

保存在某些注解作用下通过编译生成的文件、测试文件存放的目录

> 这里要与iml文件区分开，此处存放的文件大多是通过注解新生成的文件，比如queryDSL通过@Entity生成的Q类，MapStruct生成的mapper文件等。

![image-20221030110216144](E:\GitBook\Elysia\images\idea\image-20221030110216144.png)

* dataSource.xml

主要保存idea的database插件配置的数据库连接信息。

![image-20221030111820045](E:\GitBook\Elysia\images\idea\image-20221030111820045.png) 

* encodings.xml

指定某些目录文件的编码方式。

![image-20221030112107779](E:\GitBook\Elysia\images\idea\image-20221030112107779.png) 

> 文件内容与下面setting->editor->file encoding中的红框部分对应。

![image-20221030112244116](E:\GitBook\Elysia\images\idea\image-20221030112244116.png) 

* jarRepositories.xml

该文件会将maven里面定义的jar包数据源都复制过来，它会包含存库的id，名称和url。

![image-20221030114312298](E:\GitBook\Elysia\images\idea\image-20221030114312298.png)

* misc.xml

保存项目各模块版本管理的文件，如模块版本是通过maven管理，则保存pom文件的位置。

![image-20221030115333191](E:\GitBook\Elysia\images\idea\image-20221030115333191.png)

同时该文件还保存版本控制(idea,svn)等的相关设置信息。 

![image-20221030115410041](E:\GitBook\Elysia\images\idea\image-20221030115410041.png) 

* modules.xml

见名知意，该模块保存项目中有哪些模块。可以在file->project structure->module中设置

* vcs.xml

只是标识项目中哪些目录用的什么版本控制(vcs,一般是git或者svn)

![image-20221030173458696](E:\GitBook\Elysia\images\idea\image-20221030173458696.png)

* workspace.xml

保存idea的changelist，maven安装目录、本地仓库、settings文件，artifactfact相关设置，tomcat配置等。此文件过大，就不放截图了。

#### 2.2.2 iml文件作用

一个iml文件只对应一个模块文件，其与project structure中modules对应的模块一一对应，主要包含源文件位置，输出文件target目录存放位置，相关的依赖等，即：

* 标记文件夹类型，如sources, Tests，Resources, TestResources, Excluded等
* 编译文件target的存放位置
* 模块依赖

> 其中比较特殊的一个点是，如果依赖的是一个模块，我们在本地启用tomcat时，务必确认要用的artifact中已经将依赖的模块对应的jar加入进去。

![image-20221030203028154](E:\GitBook\Elysia\images\idea\image-20221030203028154.png)

如果发现没有加入进去，可以在右侧Available Elements中找到需要加入进去的模块，展开并点击其中对应的comile output，选择第一个加入到classes下或者lib下都可以。

> 如果是加入到lib下，jar的命名默认是模块名，并不影响实际使用

![image-20221030203506583](E:\GitBook\Elysia\images\idea\image-20221030203506583.png) 

#### 2.2.3 web项目artifact类型及区别

   * war类型

将工程打成war包，然后上传到服务器发布，默认存放在target目录下

> 存放的目录可通过output directory修改

   * war exploded类型

将web工程以当前文件夹的形式上传到服务器，二者的输出目录结构大致如下：

![image-20221030224758899](E:\GitBook\Elysia\images\idea\image-20221030224758899.png)

> 注意：本地开发时，为了减少tomcat启动时间，很多时候我们需要使用热部署，这种情况下要部署的artifact类型需要为war exploded类型

#### 2.2.4 Idea的build、rebuild模块以及maven的compile的区别

   * build模块

第一次build时会编译当前模块及依赖到的模块，第二次build时只会编译修改的文件(被依赖到的模块修改也会编译)。

> 例如我们删除第一次编译生成的的target中的Node.class文件，第二次编译时将不会成成Node.class，因为Node.java没有修改

   * rebuild模块

每次rebuild模块都会编译当前模块及依赖到的模块。

   * maven的compile

maven的有默认的maven-dependency-plugin插件，执行maven的compile会编译模块文件，同时maven可以执行配置的其他的一些插件，比如如果配置了querydsl的插件，执行maven的compile可以自动生成Q类，这是idea的build和rebuild不能做到的。

> 注意：maven的compile不能编译依赖到的其他模块，需要你手动对其他模块执行maven install或者maven deploy来将依赖到的其他模块安装到本地仓库或者推到远程仓库。

> tips：如果本地启动tomcat，需要在编译时用到maven的插件时，可以在tomcat配置界面，增加before launch，选择Run another Configuration，选择此前创建好的maven configuration(工作目录为当前模块，命令为mvn clean compile -Dmaven.test.skip=true)。

### 2.4 常见问题

#### 2.4.1 spring MVC项目404常见原因分析

1. 先检查需要的资源在artifact中有没有，如果有则直接看第四步。如果没有进入project structure->module 下面的web这个facets，查看web resource directories是否报红，即使不报红也要检查是否对应项目中web文件夹（部分项目是webapp文件夹），尤其<font color='red'>注意是不是下面的WEB-INF目录</font>。
![](E:\GitBook\Elysia\images\idea\image-20221003194121592.png) 

2. 进入project structure->artifacts，点击tomcat发布的artifact，查看output layout中是否有对应模块的web facets resources。如果没有点击output layout下面的+号，选择java ee facets resources
![](E:\GitBook\Elysia\images\idea\image-20221003192634754.png)

3. 检查tomcat要部署的artifacts与刚才设置的是否一样，路径填写是否正确（注意tomcat中application context是否进行过修改）

4. 如果是对静态资源的访问，检查web.xml的url-pattern中是否填写的/或者/\*，如果是则需要配置对静态资源放行，可以用如下方式中的任一种

   * 第一种<mvc:resource location="/XX/" mapping="/XX/\*\*" />
   * 第二种<mvc:default-servlet-handler />

5. 如果是通过requestMapping的方式，检查是否配置了<mvc:annotation-driven /> （默认RequestMappingHandler是没有加入spring容器的)

#### 2.4.2 多模块项目提示没有为某个项目指定jdk

提示内容：java: JDK isn't specified for module 

解决方法1：project structure->moudule->dependencies选择jdk（不要选择projectSDK)

![](E:\GitBook\Elysia\images\idea\image-20221010191800393.png)

解决方法2：重启项目，刷新maven（如果不行，则把.idea文件夹删掉，再重启项目，刷新maven)

#### 2.4.3 启动springboot(本地tomcat)时，提示端口被占用

   1. 输入命令netstat -ano | findstr  port			其中只需要将最后的port修改为端口号
   2. 输入命令taskkill /f /pid pid                             其中最后的pid是通过上面命令查出来的进程号

#### 2.4.4 bootstrap.yml图标文件是红色的

进入modules，选中Spring，然后根据下图操作

![image-20221031112220987](E:\GitBook\Elysia\images\idea\image-20221031112220987.png)

弹出如下弹框，如果没有bootstrap.yml，则加入进去。最后确认bootstrap.yml被勾选

![image-20221031112536548](E:\GitBook\Elysia\images\idea\image-20221031112536548.png)

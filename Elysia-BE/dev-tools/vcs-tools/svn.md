# SVN使用文档

> 写在前面
>
> svn服务端下载地址：[Downloads | VisualSVN](https://www.visualsvn.com/server/download/) 客户端下载地址：[Downloads · TortoiseSVN](https://tortoisesvn.net/downloads.html)

## 1 svn基础

以前使用的git，在新公司使用svn作为版本控制工具，一段时间下来总结就是"svn使用更加简单，同时功能也更加简单"。个人理解svn相当于只有git的工作区和远程仓库，add操作只是标识将哪些文件纳入版本控制，从而决定commit提交哪些文件。因此，在进行add命令之后，如果继续修改，则不能像git checkout fileName那样将工作区文件恢复到上一次add的状态。

> 如果大家对svn命令不够熟悉，操作公司远程仓库又有风险， 推荐自己下载SVN服务端(安装注意事项见第2节)，这样就能为所欲为了。

### 1.1. svn status后字母对应的文件状态

```java
A:add新增								   C:conflict冲突
D:del删除								   M:modify修改
G:modify and merged修改并已经合并			u:update从服务器更新
R:relplace从服务器替换				     I:ignore忽略
?问号代表不在SVN管理范围					 !代表本地代码与服务器不一致
L:已锁定
```

### 1.2 svn常用命令概要
```java
svn checkout svn_url					将远程svn代码拉取到本地，类似git clone
svn update								将远程代码的变动拉取到本地

svn add	filename						将新加入的文件纳入版本控制,修改不用执行
svn commit								将本地更新推送到远程
    
svn ls url								列出指定url下所有分支
svn info								列出分支信息

svn status								列出分支状态 
svn status --no-ignore					不忽略文件，显示所有文件

svn revert -R * 						放弃所有本地修改
svn rm									将已经纳入版本控制的文件删除

svn diff [-r revisionA:revisionB] file	查看差异
```
### 1.3 svn add命令

```java
svn add index.md						将index.md纳入版本控制
svn add pro_dir							将pro_dir文件夹及内部的文件一起纳入版本控制
svn add pro_dir --non-recursive			仅仅将文件夹纳入版本控制
svn add *								一次性增加所有，不包含non-recursive下的文件
svn add * --force						包含non-recursive下的文件
svn add . --no-ignore --force			将忽略掉的文件一同添加进来
```

### 1.4 svn propset命令

1. 添加键值对
```java
svn propset svn:ignore "target *.iml"

```
2. 查看所有的键
```java
svn proplist
```
3. 查看某个键对应的值
```java
svn propget svn:ignore
```
4. 删除某个键
```java
svn propdel svn:ignore .
```
> 上面所有的prop都可以简写为p,例如svn propset可以简写为svn pset
> svn pset -r version -R name "value" . 表示对version版本，递归的设置值

### 1.5 svn log命令

* 查询指定日期之间的变更

```java
svn log -r {2022-11-3}:{2022-11-4} [svn_url]
svn log -r {2022-11-3}:HEAD [suv_url]
```
> 日期格式也可以写为{"2022-11-4 08:00:00"}、{2022-11-4T20:00}、{"2022-11-4 20:00"}、{20221104T2000}

* 查询最近k次的变更

```java
svn log -l 3 [svn_url] 最近三次
```

* 查询指定版本的变更

```java
svn log -r 2000:2010 [svn_url]
```

* 详细、简单log

```
svn log -v [svn_url] 详细log
svn log -q [svn_url] 简略log
```

* log输出到指定文件

```java
svn log [svn_url] > "info.txt"
```
### 1.6 svn resolve命令

```java
svn resolve --accept working a.txt			以a.txt.working作为冲突解决后的版本
svn resolve --accept theirs-full a.txt		以a.txt.RNew作为解决冲突后的版本
svn resolve --accept mine-full a.txt		以a.txt.mine作为解决冲突后的版本
svn reslove --accept mine-conflict a.txt	以a.txt.mine的冲突部分作为解决冲突后的版本
svn resolve --accept theirs-conflict a.txt	以a.txt.RNew的冲突部分作为解决冲突后的版本
svn resolve --accept base a.txt				以未修改前的版本（相当于svn revert a.txt版本)作为解决冲突后的版本
```
> svn resolved a.txt是将a.txt作为解决冲突后的版本

### 1.7 svn merge命令

```java
svn merge -r revision1:revision2 [url] dir	将版本号区间的更新合并到当前工作空间
```
* 如果不指定url则是将当前工作空间对应的远程仓库url，最后一个参数可以是文件夹也可以是文件

### 1.8 svn switch命令
```java
svn switch repo_url							将本地工作区副本切换到指定仓库地址
```
> 跟git checkout branch_name类似，常用在trunk项目与branches项目的切换，使用前可以使用svn info查看本地工作区副本对应哪个远程仓库

### 1.9 文件锁

#### 1.9.1 文件加锁解锁操作

```java
svn lock index.html						对index进行加锁
svn unlock index.html					加锁用户执行该命令进行解锁
```
加锁的用户执行commit后，文件将自动解锁
如果提交时想继续锁定，则执行

```java
svn commit -m "xxx" --no-unlock index.html
```
#### 1.9.2 文件被锁，需要修改怎么办

1. 右击项目->TortoiseSVN->Repo-borwser查看是谁锁定了文件(或者TortoiseSVN->check modification->check respository，或者svn info file_name)

2. 解除锁定方法
	* svn unlock xxx 加锁人可用
	* 右击项目->TortoiseSVN->clean up，勾选break write lock，会解除所有的锁
	* TortoiseSVN -> Repo-borwser，可以对指定的加锁文件进行break lock
	* TortoiseSVN -> check modification -> check repository，可以ctrl+a，批量解锁

## 2 VisualSVN Server使用注意事项

1. 如果要免费使用（community liscense)，安装时不要勾选`Enable search indexing for repositories`，否则你需要购买证书或者只有45天试用期。

> 不同类型license区别：https://www.visualsvn.com/support/topic/00176/

2. 安装时Authentication mode选择Subversion authentication即可

> 两种authentication mode的区别：https://www.visualsvn.com/support/topic/00182/

## 3 基本使用

### 3.1 提交时忽略target文件夹

* 情形一：文件没有纳入版本管理

小乌龟tortoiseSVN->Setting->general->global ignore pattern中填入target(与其他忽略的元素用空格隔开)。

* 情形二：文件已纳入版本管理

方法一：创建一个changelist，建议命名为ignore，将不想要被提交的文件拖动到ignore中(或者右击move to another changelist)，提交时只提交default（这是idea的图形操作，你也可以使用svn changelist命令）。

方法二：删除远程的target文件夹，commit后执行情形一。

方法三：先删除远程的target文件夹，然后(idea中)svn->subversion->edit properties，properties名字:svn ignore，properties值填写target。或者在项目根目录下执行`svn propset svn:ignore target .`。最后commit就可以了。

最后如果target文件夹是深黄色代表忽略成功

### 3.2 idea中merge项目

1. 进入subversion working copies information,点击configure branches

2. 将需要合并进来的远程仓库url加入进来

3. 点击merge from，选择刚刚填写的branch locations对应的项目名

4. 选择quick manual select，即可将指定的commit合并进来

5. 记得勾选左侧复选框，完成点击右下角的merge。后续解决冲突见上一节。

### 3.3 清除账号名、密码缓存

右击项目->TortoiseSVN->Setting->save data，找到authentication data的clear

> clear清除可以选择清除哪些，clear all清除svn所有用户名和密码
> 证书默认存放位置：C:\Users\admin(用户)\AppData\Roaming\Subversion\auth

### 3.4 从主干创建分支

1. 通过TortoiseSVN-branch/tag图形化界面操作
> 该操作是直接操作的远程仓库，因此完成后会在远程仓库创建分支

```java
第一步：右击trunk下的项目project对应的本地文件，选择TortoiseSVN->branch/tag

第二步：在弹出的框中的to path填写branches/project;create copy in the repository选择HEAD revision in the respority.

第三步：确定后，查看远程仓库的branches也已经有project项目
```

2. 通过svn命令操作

```java
svn cp -m "create breanch" https://xxx/trunk/project https://xxx/branches/project
```

## 4 常见问题

### 4.1 svn合并不同仓库的代码错误

错误提示：Server SSL certificate verification failed: certificate issued for a different hostname, issuer is not trusted

解决方案：

1. setting->version control->subversion 开启交互模式，使用默认的配置文件(不勾选自定义配置目录)，展开的ssh中使用默认的password

2. svn(vcs) ->get from version control。查看仓库url是否有乱码

3. svn ls RESPORITORY_URL，输入P接受，然后输入电脑密码、svn的账号密码，接受证书。 

> svn ls RESPORITORY_URL  --username chenguo是以指定svn用户名登录。
> 如果最后还是不行，试着清除auth后，从外边右击项目，用小乌龟update，输入用户名密码

### 4.2 idea一直都在refresh vcs history

1. setting->version control->subversion，执行clear auth cache
2. 执行svn update，输入P(永久保存证书)，然后按照他的提示，依次输入电脑密码、svn用户、svn密码

### 4.3 idea提示do you want to schedulue the following file for deletion from subversion

原因：项目删除的文件已经加入svn，询问是否计划从schedule删除文件。

### 4.4 idea提示authentication required

1. 检查idea中subversion是否设置的是本地安装的svn\bin\svn.exe，（安装的时候需要手动comand line client tools）
2. 检查idea中subversion的配置文件目录是否为默认目录(不勾选即可)
3. idea中对subversion进行clear Auth Cache
4. svn update（或者svn ls url)，按照提示输入电脑密码、svn用户名、密码

> 如果以上三步执行完还是不行，则在subversion的默认配置目录的servers配置文件末尾追加：
> ```java
>    ssl-ignore-host-mismatch = true
>    ssl-ignore-unknown-ca = true
>    ssl-ignore-invalid-date = true 
>```

### 4.5 idea提示Error validating server certificate for ...

1. 先按照【authentication required]的解决办法操作
2. 如果执行完上一步不行，则在subversion的默认配置目录的servers配置文件末尾追加：
```java
    ssl-ignore-host-mismatch = true
    ssl-ignore-unknown-ca = true
    ssl-ignore-invalid-date = true
```

### 4.6 svn提示svn: warning: W150002: 'XXX' is already under version control

svn和git不同，git使用`git add .`或者`git add *`可以将所有未纳入暂存区的文件纳入进来。`svn add .`会认为你将当前目录所有文件都纳入版本库，包括已经在版本库的文件和文件夹，如果你想执行将所有新加入的文件都加入进来，需要执行`svn add . --force`

### 4.7 svn提示'XXX' is scheduled for addition, but is missing

原因：之前用svn add过的文件，已经被svn标记为add状态，等待被加入仓库，此时如果将其删除，svn提交时还是会尝试将其提交到仓库，但是此时其已经miss了

解决办法:
1. svn revert readme.txt 其中readme.txt是被add但是已经被删除的文件
2. idea在local changes对已被删除的文件右击进行revert

### 4.8 svn commit提示svn: E170004: Directory '/' is out of date

原因：别人在你之前修改并提交了你工作区副本所作出的修改

解决办法：
1. svn update拉取最新代码
2. svn status查看冲突原因，可能的情形见【4.9】【4.10】，

### 4.9 svn status提示local file edit, incoming file delete or move upon update

原因：这种情况是别人在你之前删除了文件并提交到远程仓库，因此你工作区所修改的文件其实已经不存在于远程仓库中，当然会有冲突。
解决办法：

1. svn diff conflict_file查看冲突文件的差异
2. 如果你想：

* 以我的修改为准！

```java
		方法一：svn resolved conflict_file
		方法二：svn resolve --accept working conflict_file
```

* 行行行，他们说了算

```java
	方法一：
	svn revert conflict_file		先撤销本地修改
	svn update						更新后本地工作区就没有conflict_file了
	
	方法二：
	svn rm conflict_file --force	如果冲突是目录则rm后要加上-r
	svn commit -m "xxx"			
```

### 4.10 svn status提示local missing or deleted or moved away, incoming dir edit upon merge Summary

原因：这种情况是别人在你之前将某个文件修改并commit到了远程仓库，因此你工作区所删除的文件别人已经抢先修改了(远程有了更新的版本好)，因此发生冲突

如果你想：

* 删掉他！以我为准
```java
svn resolved
```
* 以别人的修改为准
```java
方法一：
svn revert conflict_name			撤销本地删除
svn update							此时工作区文件与远程仓库文件一致
```
### 4.11 idea合并代码时提示There are local changes that will intersect with merge changes.如下

1. shelve local changes，将本地代码改动先搁置。
2. continue merge。再解决冲突

> merge前最好先svn update，让本地工作区干净。
原因：本地代码与合并过来的代码有冲突


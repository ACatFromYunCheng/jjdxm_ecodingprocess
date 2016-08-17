# Androidstudio常用配置 #

### Copyright notice ###

我在网上写的文章、项目都可以转载，但请注明出处，这是我唯一的要求。当然纯我个人原创的成果被转载了，不注明出处也是没有关系的，但是由我转载或者借鉴了别人的成果的请注明他人的出处，算是对前辈们的一种尊重吧！

虽然我支持写禁止转载的作者，这是他们的成果，他们有这个权利，但我不觉得强行扭转用户习惯会有一个很好的结果。纯属个人的观点，没有特别的意思。可能我是一个版权意识很差的人吧，所以以前用了前辈们的文章、项目有很多都没有注明出处，实在是抱歉！有想起或看到的我都会逐一补回去。

从一开始，就没指望从我写的文章、项目上获得什么回报，一方面是为了自己以后能够快速的回忆起曾经做过的事情，避免重复造轮子做无意义的事，另一方面是为了锻炼下写文档、文字组织的能力和经验。如果在方便自己的同时，对你们也有很大帮助，自然是求之不得的事了。要是有人转载或使用了我的东西觉得有帮助想要打赏给我，多少都行哈，心里却很开心，被人认可总归是件令人愉悦的事情。

站在了前辈们的肩膀上，才能走得更远视野更广。前辈们写的文章、项目给我带来了很多知识和帮助，我没有理由不去努力，没有理由不让自己成长的更好。写出好的东西于人于己都是好的，但是由于本人自身视野和能力水平有限，错误或者不好的望多多指点交流。

项目中如有不同程度的参考借鉴前辈们的文章、项目会在下面注明出处的，纯属为了个人以后开发工作或者文档能力的方便。如有侵犯到您的合法权益，对您造成了困惑，请联系协商解决，望多多谅解哈！若您也有共同的兴趣交流技术上的问题加入交流群QQ： 548545202

感谢作者[马伟奇老师][author]，文章出处[Android Studio简单设置][url]


## 开启gradle单独的守护进程 ##
在下面的目录下面创建gradle.properties文件：
/home/<username>/.gradle/ (Linux)
/Users/<username>/.gradle/ (Mac)
C:\Users\<username>\.gradle (Windows)
把下面配置复制gradle.properties文件也可以优化：

	# Project-wide Gradle settings.
	# IDE (e.g. Android Studio) users:
	# Settings specified in this file will override any Gradle settings
	# configured through the IDE.
	# For more details on how to configure your build environment visit
	# http://www.gradle.org/docs/current/userguide/build_environment.html
	# The Gradle daemon aims to improve the startup and execution time of Gradle.
	# When set to true the Gradle daemon is to run the build.
	# TODO: disable daemon on CI, since builds should be clean and reliable on servers
	org.gradle.daemon=true
	# Specifies the JVM arguments used for the daemon process.
	# The setting is particularly useful for tweaking memory settings.
	# Default value: -Xmx10248m -XX:MaxPermSize=256m
	org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
	# When configured, Gradle will run in incubating parallel mode.
	# This option should only be used with decoupled projects. More details, visit
	# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
	org.gradle.parallel=true
	# Enables new incubating mode that makes Gradle selective when configuring projects. 
	# Only relevant projects are configured which results in faster builds for large multi-projects.
	# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand
	org.gradle.configureondemand=true


同时上面的这些参数也可以配置到前面的用户目录下的gradle.properties文件里，那样就不是针对一个项目生效，而是针对所有项目生效。
上面的配置文件主要就是做， 增大gradle运行的java虚拟机的大小，让gradle在编译的时候使用独立进程，让gradle可以平行的运行。
### 1.申请大内存 ###
installation path\studio64.exe.vmoptions or studio.exe.vmoptions
使用文本编辑器打开，找到起始两行，如下
-Xms128m
-Xmx750m

修改最小值和最大值，建议为
-Xms256m
-Xmx2048m

### 2 优化编译 ###
 file->setting->compile
 勾选除第二项之外的其他选项，并在VM options里填入：
 -Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8


默认的 Android Studio 为灰色界面，可以选择使用炫酷的黑色界面。
Settings --> Appearance --> Theme ，选择 Darcula 主题即可
 <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance.png" width="550">
系统字体设置如果你的Android Studio界面中，中文显示有问题，或者选择中文目录显示有问题，或者想修改菜单栏的字体，可以这么设置。
Settings --> Appearance ，勾选 Override default fonts by (not recommended) ，选择一款支持中文的字体即可。我使用的是 微软雅黑 ，效果不错。
 <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance1.png" width="550"> 
编程字体设置此部分会修改编辑器的字体，包含所有的文件显示的字体。
Settings --> Editor --> Colors & Fonts --> Font 。默认系统显示的 Scheme 为 Defualt ，你是不能编辑的，你需要点击右侧的 Save As... ，保存一份自己的设置，并在当中设置。之后，在 Editor Font 中即可设置字体。
Show only monospaced fonts 表示只显示等宽字体，一般来说，编程等宽字体使用较多，且效果较好。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance2.png" width="550">
Settings --> Editor --> Colors & Fonts 中可以还可以设置字体的颜色，你可以根据你要设置的对象进行选择设置，同时你也可以从网络上下载字体颜色设置包导入。
代码格式设置如果你想设置你的代码格式化时显示的样式，你可以这么设置。
Settings --> Code Style 。同样的， Scheme 中默认的配置，你无法修改，你需要创建一份自己的配置。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance3.png" width="550">
默认文件编码无论是你个人开发，还是在项目组中团队开发，都需要统一你的文件编码。出于字符兼容的问题，建议使用 utf-8 。中国的 Windows 电脑，默认的字符编码为 GBK 。
Settings --> File Encodings 。建议将 IDE Encoding 、 Project Encoding 、 Properties Fiels 都设置成统一的编码。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance4.png" width="550">
快捷键Android Studio的快捷键和Eclipse的不相同，但是你可以在Android Studio中使用Eclipse的快捷键。
Settings --> Keymap 。你可以从 Keymaps 中选择对应IDE的快捷键，Android Studio对其他IDE的快捷键支持还是比较多的。建议不使用其他IDE的快捷键，而是使用Android Studio的快捷键。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/keymap.png" width="550">
当你想设置在某一个快捷键配置上进行更改，你需要点击 copy 创建一个自己的快捷键，并在上面进行设置。
Android Studio默认的快捷键中，代码提示为 Ctrl+Space ，会与系统输入法快捷键冲突，需要特殊设置。
Main menu --> Code --> Completion --> Basic ，更改为你想替换的快捷键组合。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/keymap1.png" width="550">
其他设置1Android Studio编辑区域，在中部会有一条竖线。这条线是用以提醒程序员，一行的代码长度最好不要超过这条线。如果你不想显示这条线，可以这么设置。
Settings --> Editor --> Appearance ，取消勾选 Show right margin (configured in Code Style options) 。
    <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance3.png" width="550">
2显示行号
Settings --> Editor --> Appearance ，勾选 Show line numbers 。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/appearance3.png" width="550">
3显示空格。我习惯显示空格，这样就能看出缩进是 tab 缩进还是空格缩进。建议使用空格缩进。
Settings --> Editor --> Appearance ，勾选 Show whitespaces 。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/codestyle.png" width="550">
4去除拼接检查。我个人觉得没用，所以禁用掉。
Settings --> Inspections --> Spelling ，取消勾选。
   <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/inspections.png" width="550">
5如果你使用 Git 进行版本控制，你需要设置 Git 的安装文件目录。
Settings --> Version Control --> Git ，在右侧中选择你的 Git 的安装目录。
6插件。Android Studio和Eclipse一样，都是支持插件的。Android Studio默认自带了一些插件，如果你不使用某些插件，你可以禁用它。
Settings --> Plugins ，右侧会显示出已经安装的插件列表。取消勾选即可禁用插件。 
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/plugins.png" width="550">
我个人禁用了一下插件:
CVS Integration ： CVS 版本控制系统，用不到。
Google Cloud Tools For Android Studio ： Google云 用不到。
Google Login ： Google账号登录，`Google Cloud Tools For Android Studio** 插件需用，用不到。
hg4idea ： Mercurial 版本控制系统，用不到。
这里需要注意的是，如果禁用了2和3选项，将导致不能使用导入官方样例的功能（ import sample ）。
你可以在 Browse repositories 页面中，搜索插件并安装。
我个人额外安装的插件：
.gitignore support ： Git 版本控制系统中 .gitignore 文件管理插件。
7检查更新。Android Studio支持自动检查更新。之前尚未发布正式版时，一周有时会有几次更新。你可以设置检查的类型，用以控制更新类型。
Settings --> Updates 。勾选 Check for updates in channel ，即开通了自动检查更新。你可以禁用自动检查更新。右侧的列表，是更新通道。
Stable Channel ： 正式版本通道，只会获取最新的正式版本。
Beta Channel ： 测试版本通道，只会获取最新的测试版本。
Dev Channel ： 开发发布通道，只会获取最新的开发版本。
Canary Channel ： 预览发布通道，只会获取最新的预览版本。
以上4个通道中， Stable Channel 最稳定，问题相对较少， Canary Channel 能获得最新版本，问题相对较多。
8自动导入。当你从其他地方复制了一段代码到Android Studio中，默认的Android Studio不会自动导入这段代码中使用到的类的引用。你可以这么设置。
Settings --> Editor --> Auto Import ，勾选 Add unambiguous improts on the fly 。
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/autoimport.png" width="550">
9有时很多人运行Android Studio会提醒你 JDK 或者 Android SDK 不存在，你需要重新设置。你需要到全局的Project Structure 页面下进行设置。进入全局的 Project Structure 页面方法如下：
方法1
   <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/autoimport.png" width="550">
选择 Configure --> Project Defaults --> Project Structure
<img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/start.png" width="550">
方法2
   <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/welcome.png" width="550">
选择 File --> Other Settings --> Default Project Structure
  <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/sdklocation.png" width="550"> 
在此页面下设置 JDK 或者 Android SDK 目录即可。
 <img src="https://raw.githubusercontent.com/jjdxmashl/jjdxm_ecodingprocess/master/screenshots/fetching.png" width="550">
10   
这是在检查你的 Android SDK 。有人会在这里卡上很长时间，很大的原因就是：网络连接有问题。可以通过配置hosts 的方式来解决。如果检查需要更新，则需要你进行安装 。
如果想跳过这一步，可以进行如下操作：
在Android Studio安装目录下的 bin 目录下，找到 idea.properties 文件，在文件最后追加disable.android.first.run=true 。


[author]:http://bbs.itheima.com/?4998
[url]:http://bbs.itheima.com/thread-167070-1-1.html
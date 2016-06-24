# 开发流程 #

### Android Studio偏好设置 ###
AndroidStudiosettings.jar文件

为Android Studio的设置导出包


### library 在Android studio 中生成jar包 ###

在library对于目录下的build.gradle中添加以下代码

    //定义一个函数，target是生成jar包的文件名，classDir是class文件所在的文件夹
    def makeJar(String target,String classDir){
	    exec{
		    executable "jar"   //调用jar
		    args "cvf",target
		    args "-C", classDir
		    args "","."
	    }
    }
    
    //新建一个task,名为buildLib,依赖build(build是一个自带的task)
    task buildLib(dependsOn:['build'])<< {
    	makeJar("jjdxmbaseutils.jar","build/intermediates/classes/release")
    }

点击buildLib方法名，右键运行gradle:buildLib方法，等待jar生成，完成后在build.gradle同目录下回找到对应的jar包

### gradle在Android Studio中常用的配置 ###

#### bulidlib.gradle 复制到项目根目录 ####
#### bintray.gradle 复制到项目根目录 ####
#### build模板主程序app.gradle 复制到项目根目录 ####
#### signing.properties 复制到项目根目录 ####
#### debug.keystore 复制到项目根目录 ####
#### README模板.md 复制到项目根目录 ####
#### build模板library.gradle 复制到项目的module类库目录下 ####
#### build模板根项目.gradle 复制到项目的主程序目录下 ####
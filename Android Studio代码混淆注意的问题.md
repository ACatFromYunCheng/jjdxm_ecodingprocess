
文章原地址： [http://blog.csdn.net/jdsjlzx/article/details/51861460][url]

#### Java 是一种跨平台的、解释型语言，Java 源代码编译成中间”字节码”存储于 class 文件中。由于跨平台的需要，Java 字节码中包括了很多源代码信息，如变量名、方法名，并且通过这些名称来访问变量和方法，这些符号带有许多语义信息，很容易被反编译成 Java 源代码。为了防止这种现象，我们可以使用 Java 混淆器对 Java 字节码进行混淆。 ####

	-include {filename}    从给定的文件中读取配置参数 
	-basedirectory {directoryname}    指定基础目录为以后相对的档案名称 
	-injars {class_path}    指定要处理的应用程序jar,war,ear和目录 
	-outjars {class_path}    指定处理完后要输出的jar,war,ear和目录的名称 
	-libraryjars {classpath}    指定要处理的应用程序jar,war,ear和目录所需要的程序库文件 
	-dontskipnonpubliclibraryclasses    指定不去忽略非公共的库类。 
	-dontskipnonpubliclibraryclassmembers    指定不去忽略包可见的库类的成员。
	
	保留选项 
	-keep {Modifier} {class_specification}    保护指定的类文件和类的成员 
	-keepclassmembers {modifier} {class_specification}    保护指定类的成员，如果此类受到保护他们会保护的更好
	-keepclasseswithmembers {class_specification}    保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。 
	-keepnames {class_specification}    保护指定的类和类的成员的名称（如果他们不会压缩步骤中删除） 
	-keepclassmembernames {class_specification}    保护指定的类的成员的名称（如果他们不会压缩步骤中删除） 
	-keepclasseswithmembernames {class_specification}    保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后） 
	-printseeds {filename}    列出类和类的成员-keep选项的清单，标准输出到给定的文件 
	
	压缩 
	-dontshrink    不压缩输入的类文件 
	-printusage {filename} 
	-dontwarn   如果有警告也不终止
	-whyareyoukeeping {class_specification}     
	
	优化 
	-dontoptimize    不优化输入的类文件 
	-assumenosideeffects {class_specification}    优化时假设指定的方法，没有任何副作用 
	-allowaccessmodification    优化时允许访问并修改有修饰符的类和类的成员 
	
	混淆 
	-dontobfuscate    不混淆输入的类文件 
	-printmapping {filename} 
	-applymapping {filename}    重用映射增加混淆 
	-obfuscationdictionary {filename}    使用给定文件中的关键字作为要混淆方法的名称 
	-overloadaggressively    混淆时应用侵入式重载 
	-useuniqueclassmembernames    确定统一的混淆类的成员名称来增加混淆 
	-flattenpackagehierarchy {package_name}    重新包装所有重命名的包并放在给定的单一包中 
	-repackageclass {package_name}    重新包装所有重命名的类文件中放在给定的单一包中 
	-dontusemixedcaseclassnames    混淆时不会产生形形色色的类名 
	-keepattributes {attribute_name,...}    保护给定的可选属性，例如LineNumberTable, LocalVariableTable, SourceFile, Deprecated, Synthetic, Signature, and 
	
	InnerClasses. 
	-renamesourcefileattribute {string}    设置源文件中给定的字符串常量

### 不能混淆的代码 ###

#### 顾名思义，不能混淆代码如果被混淆了，就会出现错误。 ####

1.需要反射的代码
2.系统接口
3.Jni接口
4.需要序列号和反序列化的代码（即实现Serializable接口的JavaBean）
5.与服务端进行元数据交互的JavaBean（JSON、XML中对应的类）

#### 常见错误 ####

1) Proguard returned with error code 1. See console

更新proguard版本 
android-support-v4 不进行混淆 
添加缺少相应的库

2) 使用gson包解析数据时，出现 missing type parameter 异常

在 proguard-project.txt 中添加 

	-dontobfuscate 
	-dontoptimize 

在 proguard-project.txt 中添加 

	# removes such information by default, so configure it to keep all of it. 
	-keepattributes Signature 
	# Gson specific classes 
	-keep class sun.misc.Unsafe { *; } 
	#-keep class com.google.gson.stream.* { ; } 
	# Application classes that will be serialized/deserialized over Gson 
	-keep class com.google.gson.examples.android.model.* { ; }

3) 类型转换错误

在 proguard-project.txt 中添加 

	-keepattributes Signature

4) 空指针异常

混淆过滤掉相关类与方法

5) java.lang.reflect.UndeclaredThrowableException

	-keep interface com.dev.impl.**

6) Error: Unable to access jarfile ..libproguard.jar


#### 路径问题 ####

7) java.lang.NoSuchMethodError

这也是最常见的问题，因为找不到相关方法，方法被混淆了，混淆过滤掉相关方法便可。

### 下面是我实际项目中的混淆配置： ###


	#---------------------------------1.实体类---------------------------------
	-keep class com.package.bean.** { *; }
	
	
	#-------------------------------------------------------------------------
	
	#---------------------------------2.第三方包-------------------------------
	
	#eventBus
	-keepattributes *Annotation*
	-keepclassmembers class ** {
	    @org.greenrobot.eventbus.Subscribe <methods>;
	}
	-keep enum org.greenrobot.eventbus.ThreadMode { *; }
	-keepclassmembers class * extends org.greenrobot.eventbus.util.ThrowableFailureEvent {
	    <init>(java.lang.Throwable);
	}
	
	#glide
	-keep public class * implements com.bumptech.glide.module.GlideModule
	-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
	  **[] $VALUES;
	  public *;
	}
	-keep class com.bumptech.glide.** { *; }
	
	#retrofit2
	-dontwarn retrofit2.**
	-keep class retrofit2.** { *; }
	-keepattributes Signature
	-keepattributes Exceptions
	
	-dontwarn org.robovm.**
	-keep class org.robovm.** { *; }
	
	#okhttp3
	-dontwarn com.squareup.okhttp3.**
	-keep class com.squareup.okhttp3.** { *;}
	-keep class okhttp3.** { *;}
	-keep class okio.** { *;}
	-dontwarn sun.security.**
	-keep class sun.security.** { *;}
	-dontwarn okio.**
	-dontwarn okhttp3.**
	
	#rxjava
	-dontwarn rx.**
	-keep class rx.** { *; }
	
	-dontwarn sun.misc.**
	-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
	 long producerIndex;
	 long consumerIndex;
	}
	-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
	 rx.internal.util.atomic.LinkedQueueNode producerNode;
	}
	-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
	 rx.internal.util.atomic.LinkedQueueNode consumerNode;
	}
	
	#baidu
	-keep class com.baidu.** { *; }
	-dontwarn com.baidu.**
	
	#alipay
	-keep class com.alipay.android.app.IAlixPay{*;}
	-keep class com.alipay.android.app.IAlixPay$Stub{*;}
	-keep class com.alipay.android.app.IRemoteServiceCallback{*;}
	-keep class com.alipay.android.app.IRemoteServiceCallback$Stub{*;}
	-keep class com.alipay.sdk.app.PayTask{ public *;}
	-keep class com.alipay.sdk.app.AuthTask{ public *;}
	-keep class com.alipay.mobilesecuritysdk.*
	-keep class com.ut.*
	
	-dontwarn android.net.**
	-keep class android.net.** { *; }
	
	
	#gson
	-keep class com.google.gson.** {*;}
	#-keep class com.google.**{*;}
	-keep class sun.misc.Unsafe { *; }
	-keep class com.google.gson.stream.** { *; }
	-keep class com.google.gson.examples.android.model.** { *; }
	-keep class com.google.** {
	    <fields>;
	    <methods>;
	}
	-keepclassmembers class * implements java.io.Serializable {
	    static final long serialVersionUID;
	    private static final java.io.ObjectStreamField[] serialPersistentFields;
	    private void writeObject(java.io.ObjectOutputStream);
	    private void readObject(java.io.ObjectInputStream);
	    java.lang.Object writeReplace();
	    java.lang.Object readResolve();
	}
	-dontwarn com.google.gson.**
	
	#umeng
	-dontwarn com.umeng.**
	-keep class com.umeng.**{*;}
	-keep class u.aly.**{*;}
	-keep class com.google.**{*;}
	
	#butterknife
	-keep class butterknife.** { *; }
	-dontwarn butterknife.internal.**
	-keep class **$$ViewBinder { *; }
	
	-keepclasseswithmembernames class * {
	    @butterknife.* <fields>;
	}
	
	-keepclasseswithmembernames class * {
	    @butterknife.* <methods>;
	}
	
	#pinyin4j
	-dontwarn net.soureceforge.pinyin4j.**
	-dontwarn demo.**
	-libraryjars src/libs/pinyin4j-2.5.0.jar
	-keep class net.sourceforge.pinyin4j.** { *;}
	-keep class demo.** { *;}
	-keep class com.hp.** { *;}
	
	#httpclient (org.apache.http.legacy.jar)
	-dontwarn android.net.compatibility.**
	-dontwarn android.net.http.**
	-dontwarn com.android.internal.http.multipart.**
	-dontwarn org.apache.commons.**
	-dontwarn org.apache.http.**
	-dontwarn org.apache.http.protocol.**
	-keep class android.net.compatibility.**{*;}
	#-keep class android.net.http.**{*;}
	-keep class com.android.internal.http.multipart.**{*;}
	-keep class org.apache.commons.**{*;}
	-keep class org.apache.org.**{*;}
	-keep class org.apache.harmony.**{*;}
	
	#lib-wheel
	-dontwarn kankan.wheel.**
	-keep class kankan.wheel.**{*;}
	
	#PhotoPicker
	-dontwarn me.iwf.photopicker.**
	-keep class me.iwf.photopicker.**{*;}
	
	#nineoldandroids
	-dontwarn com.nineoldandroids.*
	-keep class com.nineoldandroids.** { *;}
	
	#weixin
	-dontwarn com.tencent.mm.**
	-keep class com.tencent.mm.**{*;}
	
	#topsnackbar
	-dontwarn com.androidadvance.topsnackbar.**
	-keep class com.androidadvance.topsnackbar.**{*;}
	
	#pull_recyclerview_library
	-dontwarn com.cundong.recyclerview.**
	-keep class com.cundong.recyclerview.**{*;}
	
	#-------------------------------------------------------------------------
	
	#---------------------------------3.与js互相调用的类------------------------
	
	
	
	#-------------------------------------------------------------------------
	
	#---------------------------------4.反射相关的类和方法-----------------------
	
	
	
	#----------------------------------------------------------------------------
	
	#-------------------------------------------基本不用动区域--------------------------------------------
	#---------------------------------基本指令区----------------------------------
	-optimizationpasses 5
	-dontskipnonpubliclibraryclassmembers
	-printmapping proguardMapping.txt
	-optimizations !code/simplification/cast,!field/*,!class/merging/*
	-keepattributes *Annotation*,InnerClasses
	-keepattributes Signature
	-keepattributes SourceFile,LineNumberTable
	#-ignorewarnings
	#----------------------------------------------------------------------------
	
	#---------------------------------默认保留区---------------------------------
	-keep public class * extends android.app.Activity
	-keep public class * extends android.app.Application
	-keep public class * extends android.app.Service
	-keep public class * extends android.content.BroadcastReceiver
	-keep public class * extends android.content.ContentProvider
	-keep public class * extends android.app.backup.BackupAgentHelper
	-keep public class * extends android.preference.Preference
	-keep public class * extends android.view.View
	-keep public class com.android.vending.licensing.ILicensingService
	-keep class android.support.** {*;}
	-keep public class * extends android.os.IInterface
	
	-keep public class * extends android.view.View{
	    *** get*();
	    void set*(***);
	    public <init>(android.content.Context);
	    public <init>(android.content.Context, android.util.AttributeSet);
	    public <init>(android.content.Context, android.util.AttributeSet, int);
	}
	-keepclasseswithmembers class * {
	    public <init>(android.content.Context, android.util.AttributeSet);
	    public <init>(android.content.Context, android.util.AttributeSet, int);
	}
	-keepclassmembers class * implements java.io.Serializable {
	    static final long serialVersionUID;
	    private static final java.io.ObjectStreamField[] serialPersistentFields;
	    private void writeObject(java.io.ObjectOutputStream);
	    private void readObject(java.io.ObjectInputStream);
	    java.lang.Object writeReplace();
	    java.lang.Object readResolve();
	}
	-keep class **.R$* {
	 *;
	}
	-keepclassmembers class * {
	    void *(**On*Event);
	}
	
	-keepclasseswithmembernames class * { # 保持 native 方法不被混淆
	 native <methods>;
	}
	
	-keepclasseswithmembers class * { # 保持自定义控件类不被混淆
	 public <init>(android.content.Context, android.util.AttributeSet);
	}
	
	-keepclasseswithmembers class * {# 保持自定义控件类不被混淆
	 public <init>(android.content.Context, android.util.AttributeSet, int);
	}
	
	-keepclassmembers class * extends android.app.Activity { # 保持自定义控件类不被混淆
	 public void *(android.view.View);
	}
	
	-keepclassmembers enum * { # 保持枚举 enum 类不被混淆
	 public static **[] values();
	 public static ** valueOf(java.lang.String);
	}
	
	-keep class * implements android.os.Parcelable { # 保持 Parcelable 不被混淆
	 public static final android.os.Parcelable$Creator *;
	}
	
	#----------------------------------------------------------------------------
	
	#---------------------------------webview------------------------------------
	-keepclassmembers class fqcn.of.javascript.interface.for.webview {
	   public *;
	}
	-keepclassmembers class * extends android.webkit.webViewClient {
	    public void *(android.webkit.WebView, java.lang.String, android.graphics.Bitmap);
	    public boolean *(android.webkit.WebView, java.lang.String);
	}
	-keepclassmembers class * extends android.webkit.webViewClient {
	    public void *(android.webkit.webView, jav.lang.String);
	}
	#----------------------------------------------------------------------------
	#---------------------------------------------------------------------------------------------------


[url]:http://blog.csdn.net/jdsjlzx/article/details/51861460
原文来自：[Best practices in Android development](https://github.com/futurice/android-best-practices)

通过学习下面的手册可以帮助我们免于再造轮子。如果你对iOS或WP开发感兴趣，可参考[iOS Good Practices](https://github.com/futurice/ios-good-practices)和[Windows App Development Best Practices](https://github.com/futurice/ios-good-practices) 文档。（来源于Futurice网站中Android开发者的经验）
一、几句话
使用Gradle及其推荐的项目框架

把密码等敏感数据放入gradle.properties

不要自己写Http客户端，使用Volley或OkHttp库

使用Jackson库来解析JSON数据

避免[Guava](https://github.com/google/guava)并出于[Dalvik 65K methods limit](https://medium.com/@rotxed/dex-skys-the-limit-no-65k-methods-is-28e6cb40cf71)不要使用过多的库

使用Fragment来绘制UI界面

Activity主要用来管理Fragment

布局文件XML也是代码，好好组织它们

在布局文件里，使用styles以避免重复的属性

使用多个style文件而不是一个巨大的style文件

保持你的 color.xml 短小而DRY，定义色盘

同样保持 dimens.xml DRY，定义通用常量

不要创建一个太深层次的布局

避免WebView的客户端处理，而且要注意内存泄露

使用Robolectric来进行单元测试，Robotium来进行连接（UI）测试

仿真器用Genymotion

一定要用ProGuard 或 DexGuard

二、详细
Android SDK
把你的Android SDK放置在你的主目录里或其他与应用无关的地方。一些IDEs在安装的时候会把SDK关联上，并把SDK放在IDE的同一个目录下。当你需要升级（重装）IDE或者更换IDE时你就会发现糟糕之处啦。另外，如果你的IDE在一个user账户下而不是在root下运行的话，就不要把SDK放在系统级目录下，否则在使用时需要 sudo 权限，
Build System
默认的选择是 [Gradle](http://tools.android.com/tech-docs/new-build-system)。Ant限制比较多而且太大。使用Gradle，你可以很轻易的做到：
-编译不同的flavours 或应用的 variants
-创建简单的 类-脚本 任务

-管理和下载依赖

-自定义keystores

-等等

Android的Gradle插件同样被Google指定为新的标准编译系统，而且Google不断为其升级。

项目结构
有两种流行的选择：旧的Ant & Eclipse ADT项目结构；新的Gradle & Android Studio项目结构。你应该选择后者。如果你的项目使用旧的结构，那么换掉吧。
旧结构

old-structure

├─ assets

├─ libs

├─ res

├─ src

│  └─ com/futurice/project

├─ AndroidManifest.xml

├─ build.gradle

├─ project.properties

└─ proguard-rules.pro
新结构

new-structure

├─ library-foobar

├─ app

│  ├─ libs

│  ├─ src

│  │  ├─ androidTest

│  │  │  └─ java

│  │  │    └─ com/futurice/project

│  │  └─ main

│  │    ├─ java

│  │    │  └─ com/futurice/project

│  │    ├─ res

│  │    └─ AndroidManifest.xml

│  ├─ build.gradle

│  └─ proguard-rules.pro

├─ build.gradle

└─ settings.gradle
新结构主要的不同在于拆分了'源代码集' (main,androidTest)，这是来自Gradle的理念。
使用最高级别"app"有利于将你的app和其他你的应用所引用的库项目（如：library-foobar）做区分。然后settings.gradle保持应用对这些库的索引，而app/build.gradle可以指向这些库。

























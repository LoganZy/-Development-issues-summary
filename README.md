# Development-issues-summary
## 开发中遇到的一些问题和解决方案的合集

1. List item使用material desgin显示时，遇到IllegalArgumentException。  
    ```
     Caused by: java.lang.IllegalArgumentException: 
           AppCompat does not support the current theme features:{
		          windowActionBar: false,
		          windowActionBarOverlay: false,
		          android:windowIsFloating: false, 
		          windowActionModeOverlay: false, 
		          windowNoTitle: false } ;
    ```
   > 解决办法：
   >       （1）在style文件中将  `<item name="android:windowNoTitle">true</item>`  改为`<item name="windowNoTitle">true</item>`  
   >       （2）在values-v21文件夹下面 添加一个新的styles.xml配置文件:`<style name="AppTheme" parent="AppTheme.Base">`,在values文件下的styles文件中多添加一层继承的style标签。  
   >       具体详见  [我的博客](http://blog.csdn.net/woshishui5577/article/details/53285351 "悬停显示")
 
 2. Android stduio 编译项目，Minimum supported Gradle version is 4.1. Current version is 3.3. If using the gradle wrapper,报错信息如下：
    ```
      > Failed to apply plugin [id 'com.android.application']
      > Minimum supported Gradle version is 4.1. Current version is 3.3. If using the gradle wrapper, 
            try editing the distributionUrl in E:\AndroidStudy\gradle\wrapper\gradle-wrapper.properties
            to gradle-4.1-all.zip
    ```
    > 解决方案：
    > 找到 gradle-wrapper.properties文件，修改文件即可。
    >  ``` 
    >  #Tue Jan 30 17:55:46 WIB 2018
    >  distributionBase=GRADLE_USER_HOME
    >  distributionPath=wrapper/dists
    >  zipStoreBase=GRADLE_USER_HOME
    >  zipStorePath=wrapper/dists
    >  distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip

3. 通过Android studio的instant run 安装app。在oppo R15手机上，安装失败，报错信息如下：
	```
	java.lang.RuntimeException: Unable to instantiate activity ComponentInfo{com.wljr.androidstudy/com.
		wljr.androidstudy.MainActivity}: 
		java.lang.ClassNotFoundException: 
			Didn't find class "com.wljr.androidstudy.MainActivity" on path: 
			DexPathList[[zip file "/data/app/com.wljr.androidstudy-8HXxKg1pTTkf1ruzuiEl0w==/base.apk"],
			nativeLibraryDirectories=[/data/app/com.wljr.androidstudy-8HXxKg1pTTkf1ruzuiEl0w=
			=/lib/arm64, /system/lib64, /vendor/lib64]]  
	```
	> 解决方案：
	原因：排查代码，并没有发现问题，支持了分包。最终原因在于instant run安装app时，都是增量化安装，并没有完全的编译。这样在一部分收集上面就会报错。	
	解决：在settting-->Build,Excution,Deploymnent-->Instant Run-->去掉勾选，再次安装app，就可以顺利运行了。详见下图：
	
	![去掉Instant Run的勾选](http://p981u1am0.bkt.clouddn.com/18-6-8/79474348.jpg)
	
		


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
      Failed to apply plugin [id 'com.android.application']
      Minimum supported Gradle version is 4.1. Current version is 3.3. If using the gradle wrapper, 
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
	>       原因：排查代码，并没有发现问题，支持了分包。最终原因在于instant run安装app时，都是增量化安装，并没有完全的编译。这样在一部分收集上面就会报错。	
	>       解决：在settting-->Build,Excution,Deploymnent-->Instant Run-->去掉勾选，再次安装app，就可以顺利运行了。详见下图：
	
	![去掉Instant Run的勾选](http://p981u1am0.bkt.clouddn.com/18-6-8/79474348.jpg)
	
4. 打开新的Android Studio项目，报错 ` Error:Failed to open zip file.
Gradle's dependency cache may be corrupt `；具体信息如下：
	
		Error:Failed to open zip file.
			Gradle's dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)
			Re-download dependencies and sync project (requires network)</a>
			Re-download dependencies and sync project (requires network)</a>
	> 解决方案：
	>       原因：打开新项目的时候，读取项目gradle-->wrapper-->gradle-wrapper.properties文件中distributionUrl配置，在本地并没有响应的缓存。
	>       解决：修改distributionUrl配置为之前可以使用的项目中的配置即可，我的配置如下：
		
		#Thu Jun 14 11:46:52 CST 2018  
		distributionBase=GRADLE_USER_HOME  
		distributionPath=wrapper/dists  
		zipStoreBase=GRADLE_USER_HOME  
		zipStorePath=wrapper/dists  
		distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip
		
	项目根目录下build.gradle 文件中dependencies的gradle版本配置为：
	> `classpath 'com.android.tools.build:gradle:3.0.0'`

5. 加载项目报错，`java.lang.NoSuchMethodError: No static method getFont`，具体信息如下：

		java.lang.NoSuchMethodError:
			 No static method getFont(Landroid/content/Context;ILandroid/util/TypedValue;ILandroid/widget/TextView;)
				 Landroid/graphics/Typeface; in class Landroid/support/v4/content/res/ResourcesCompat; or its super 
				 classes (declaration of 'android.support.v4.content.res.ResourcesCompat' appears in /data/app/
				 com.wljr.androidstudy-2/split_lib_dependencies_apk.apk:classes2.dex)
	            at android.support.v7.widget.TintTypedArray.getFont(TintTypedArray.java:119)
	            at android.support.v7.widget.AppCompatTextHelper.updateTypefaceAndStyle(AppCompatTextHelper.java:208)
	            at android.support.v7.widget.AppCompatTextHelper.loadFromAttributes(AppCompatTextHelper.java:110)
	            at android.support.v7.widget.AppCompatTextHelperV17.loadFromAttributes(AppCompatTextHelperV17.java:38)
	            at android.support.v7.widget.AppCompatTextView.<init>(AppCompatTextView.java:81)
	            at android.support.v7.widget.AppCompatTextView.<init>(AppCompatTextView.java:71)
	            at android.support.v7.widget.AppCompatTextView.<init>(AppCompatTextView.java:67)
	            at android.support.v7.widget.Toolbar.setTitle(Toolbar.java:753)
	            at android.support.v7.widget.ToolbarWidgetWrapper.setTitleInt(ToolbarWidgetWrapper.java）

	> 解决方案：
	
 - 将app的build.gradle中
 
	   compileSdkVersion 26
	   buildToolsVersion '26.0.2'
    修改为
	  
	   compileSdkVersion 27
	   buildToolsVersion '27.0.0'

 -  将项目中的V 7包下的依赖库版本 `support_version = "26.0.2` 修改为`support_version = "27.0.0"`
 
	 **ps：** 更多详情请查看- ->[stackoverflow](https://stackoverflow.com/users/9651968/logan-zy?tab=favorites)
 
 6. 打开Android stduio报错， `Android Studio not starting: Fatal error initializing "com.intellij.util.indexing.FileBasedIndex"`。部分日志信息如下：


		 Internal error. Please report to http://code.google.com/p/android/issues

		java.lang.RuntimeException: com.intellij.ide.plugins.PluginManager$StartupAbortedException: Fatal error initializing 'com.intellij.util.indexing.FileBasedIndex'
		    at com.intellij.idea.IdeaApplication.run(IdeaApplication.java:159)
		    at com.intellij.idea.MainImpl$1$1$1.run(MainImpl.java:46)
		    at java.awt.event.InvocationEvent.dispatch(InvocationEvent.java:311)
		    at java.awt.EventQueue.dispatchEventImpl(EventQueue.java:744)
		    at java.awt.EventQueue.access$400(EventQueue.java:97)
		    at java.awt.EventQueue$3.run(EventQueue.java:697)
		    at java.awt.EventQueue$3.run(EventQueue.java:691)
		    at java.security.AccessController.doPrivileged(Native Method)
		    at java.security.ProtectionDomain$1.doIntersectionPrivilege(ProtectionDomain.java:75)
		    at java.awt.EventQueue.dispatchEvent(EventQueue.java:714)
		    at com.intellij.ide.IdeEventQueue.defaultDispatchEvent(IdeEventQueue.java:697)
		    at com.intellij.ide.IdeEventQueue._dispatchEvent(IdeEventQueue.java:524)
		    at com.intellij.ide.IdeEventQueue.dispatchEvent(IdeEventQueue.java:335)
		    at java.awt.EventDispatchThread.pumpOneEventForFilters(EventDispatchThread.java:201)
		    at java.awt.EventDispatchThread.pumpEventsForFilter(EventDispatchThread.java:116)
		    at java.awt.EventDispatchThread.pumpEventsForHierarchy(EventDispatchThread.java:105)
		    at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:101)
		    at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:93)
		    at java.awt.EventDispatchThread.run(EventDispatchThread.java:82)

	> 解决方案及原因：这个应该是Android studio的一个bug，在Androids open source bug tracker上，有解决办法，如下。
	```
	This Works without the loss any settings or project. It will take you to your previous state at the time of editing an open file.  

	1. go to your home directory. ie /home/XXXXXX/.AndroidStudio.X.X  
	2. rename the .AndroidStudio.X.X to any thing else i.e. back_up  
	3. run your android studio.  
	4. it will prompt you to import current setting or create a new version  
	5. choose the import setting and sellect the back_up directory.  
	6. Bravo You are good to go.
	```
	
	更多解决方案，请参考 [StackOverFlow](https://stackoverflow.com/questions/28003717/android-studio-not-starting-fatal-error-initializing-com-intellij-util-indexin)。
	
 7. 接入Thinker过程，上传patch到bugly平台报错。具体错误信息如下：
	> 上传失败！补丁文件缺失必需字段: Created-Time、Created-By、YaPatchType、VersionName、VersionCode、From、To，请检查补丁文件后重试！

	> 原因和解决方案：
			 在于上传补丁包时选择文件错误导致，应该选择`build-->outputs-->patch-->patch_signed_7zip.apk`这个文件,而不能选择`build-->outputs-->thinkerPatch-->patch_signed_7zip.apk`文件，不然就会报上面的错误。

	可以双击`build-->outputs-->patch-->patch_signed_7zip.apk`文件，在工程右侧视图中可以查看YAPATCH.MF文件中的信息，了解补丁版本是怎么匹配到目标版本的。
	
		Created-Time: 2018-9-17 11:37:15.673 
	    Created-By: YaFix(1.1) 
	    YaPatchType: 2 
	    VersionName: 3.6.3
	    VersionCode: 363
	    From: 1.0.5-base  //基准包的tinkerId
	    To: 1.0.5-patch	//当前补丁包的tinkerId
 8. Glide混淆，使用GlideModule是需要在manifest中指定的，debug版本一直没事，realease版本各种Crash,报错信息:

		 java.lang.IllegalArgumentException: Unable to find GlideModule to find GlideModule implementation...
	
	> 原因和解决方案：
	在没有使用GlideModel之前，已经添加了混淆，在使用了GlideModel之后 还需对实现该接口的类保持不被混淆。完成的混淆代码如下：

		-keep public class * implements com.bumptech.glide.module.GlideModule  
		-keep public class * extends com.bumptech.glide.AppGlideModule  
		-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {  
			  **[] $VALUES;  
			  public *;  
		}
		-keep class com.bumptech.** {
		 *;
		}
 9. 接入热修复thinker,debug模式下运行没有问题，打Release版本apk，就会出现错误，日志信息z摘要如下：	


		com.tencent.tinker.loader.TinkerRuntimeException: Tinker Exception:createDelegate failed
		Caused by: java.lang.VerifyError: com/XXXX/XXXX/application/MyApplicationLike
		
    一般这种情况下，在debug模式运行没有问题，release模式出错，80%都是因为开启混淆设置，但是混淆配置文件，没有配置齐全造成的。
    > 原因和解决方案：
    > 从错误信息中可以，明确定位到问题是出在thinker的applicationLike文件上，这个文件极有可能没有混淆了，我们可以反编译apk文件，看下applicationLike文件来验证。可以看到结果入我们的猜想一样。
    > 定位了问题，直接添加混淆配置，保证这个文件不被混淆即可。
   
	    #tinker and bugly
	   -dontwarn com.tencent.bugly.**
		-keep public class com.tencent.bugly.**{*;}
		-dontwarn com.tencent.tinker.**
		-keep class com.tencent.tinker.** { *; }
		-keep class android.support.**{*;}

		-keep class com.tencent.tinker.loader.** {
			*;
		}
		//将application更换为自己项目中的响应 
		-keep class com.wlibao.application.WlbBaseApplitiaon {
			*;
		}

		-keep public class * implements com.tencent.tinker.loader.app.ApplicationLifeCycle {
			*;
		}

		-keep public class * extends com.tencent.tinker.loader.TinkerLoader {
			*;
		}

		-keep public class * extends com.tencent.tinker.loader.app.TinkerApplication {
			*;
		}
 10. Retrofit2.0，动态拼接配置url，结果拼接的url被转义了。导致接口请求错误。
     > 原因和解决方案：
     在使用@Post、@Path注解搭配使用动态修改url，作为path拼接的url，有多个`/`符号，就会被转义错误。
     
	     @POST("{path}")  
	     Observable<String> httpConnect(@Path("path")  String path, @Body RequestBody requestBody);
       
       >第一种：更换实现形式为 **@Path(value = "path",encoded = true)**，行不通。
       >第二种：使用@Post、@Url注解取代@Path实现；可以解决问题。
       
	     @POST  
		 Observable<JSONObject> httpConnect(@Url String path, @Body RequestBody requestBody);
		
 11. 当使用Gson解析json数据时，如果你的结果类型是一个泛型比如T，此时这个T如果又被其他类包裹那么我们通常的写法是这样。

		 public static <T> ApiResponse<T> fromJson(String json) { 
			 return new Gson().fromJson(json, new TypeToken<ApiResponse<T>>() {}.getType()); 
		 }
	 这样的写法最终是无法获取到结果的，你会得到以下错误
	 
		 Caused by: java.lang.ClassCastException: 
					   com.google.gson.internal.LinkedTreeMap cannot be cast to 
					   com.hengda.platform.easyhttp.example.TestBean

	 >原因和解决方案：
	 这是因为Gson 中的TypeToken 的实现逻辑是,根据TypeToken 的派生类.使用getGenericSuperclass 获取泛型信息的. 而我们这边的泛型并没有办法被正确的传递.，既然没有正确传递，那么只要给它传递正确的类型即可。
	 
	 修改fromJson方法如下：
	 
		 public static <T> ApiResponse<T> fromJson(String json,Class<T> cla) {
		     Type type= $Gson$Types.newParameterizedTypeWithOwner(null, ApiResponse.class, cla); return new Gson().fromJson(json, type); 
	     }
 12. CheckBox指定style的button属性为selector时，点击切换状态时，背景照片被遮盖住部分，展示不全。
     > 原因：在于选中背景和非选中背景照片的宽高属性不一致；而且我们在xml的style属性中layout_width和layout_height属性为wrap_content。切换选中状态时，就会出现宽高属性偏大的图片，不能完全展示。
     
     > 解决方案：
      一、将selector选择器中，选中和未选中的照片宽高属性改为一致。
      二、将checkBox指定style的layout_width和layout_height设置为UI效果图上指定的尺寸值。

 13. 
 
  


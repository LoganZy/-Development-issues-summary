# -Development-issues-summary
开发中遇到的一些问题和解决方案的合集


**1.**使用material desgin显示时，遇到IllegalArgumentException。 <br>
         具体异常如下：Caused by: java.lang.IllegalArgumentException: AppCompat does not support the current theme features:{ windowActionBar: false, windowActionBarOverlay: false, android:windowIsFloating: false, windowActionModeOverlay: false, windowNoTitle: false } ;  <br>
         解决办法：（1）在style文件中将 `<item name="android:windowNoTitle">true</item>` 改为`<item name="windowNoTitle">true</item>` <br>&nbsp&nbsp&nbsp&nbsp（2）在values-v21文件夹下面 添加一个新的styles.xml配置文件:`<style name="AppTheme"   parent="AppTheme.Base">`,在values文件下的styles文件中多添加一层继承的style标签。  <br>
         具体详见 [我的博客](http://blog.csdn.net/woshishui5577/article/details/53285351 "悬停显示")

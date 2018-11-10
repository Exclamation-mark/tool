# Android开发常用控件打包集合
## 1.圆角图片
### 实现方法 ：继承自ImageView。添加边距相应的标签
### 使用方法 ：就像使用ImageView一样去使用它
```
<com.example.mylibrary.View.CircleImageView
        android:layout_width="120dp"
        android:layout_centerInParent="true"
        android:layout_height="120dp"
        android:id="@+id/record"
        app:civ_border_width="3dp"
        app:civ_border_color="#fff"
        android:src="@drawable/btn_voice_3x" />
```
### 自定义标签
标签| 定义 | 单位
----|-----|------
civ_border_width|边界宽度|dimension
civ_border_color|边界颜色|color
civ_border_overlay| 边界是否透明 | boolean
civ_fill_color|圆内填充颜色|color
### 除标签相关的get set方法外，还有重写如下方法用来填充图片：
方法| 参数
--------|-------------
setImageBitmap|Bitmap
setImageDrawable|Drawable
setImageResource| int resId
setImageURI|Uri
### 使用效果:
![效果图](https://camo.githubusercontent.com/e17a2a83e3e205a822d27172cb3736d4f441344d/68747470733a2f2f7261772e6769746875622e636f6d2f68646f64656e686f662f436972636c65496d616765566965772f6d61737465722f73637265656e73686f742e706e67)
### 感谢：这是学习的第一个控件的开发，不是原创，完全来自于：
[点击查看来源](https://github.com/hdodenhof/CircleImageView)
## 2.初步仿QQ录音效果
### 效果图
![qq效果图](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/qqanniu.gif)
![demo](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/demo.gif)
### 特点


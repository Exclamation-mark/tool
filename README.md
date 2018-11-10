# Android开发常用控件打包集合
## 1.聊天界面中的聊天信息子页面（ChatItemView）
### demo效果图

![样品图](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/demo1.gif)
### 特点：
###### 1.要根据信息是发送方还是接收方来判断显示在左边还是右边
###### 2.要根据信息的种类来判断是文字还是图片还是语音。
###### 2.还要保留各种空间的点击事件。
### 缺点：
###### 1能满足要求，但是不够完美，不能满足所有的情况，而且标签不够多不够自由。
###### 2我在这里面封装了一个图片加载器，在引用时可能会与原来的图片加载器产生冲突，最高级的办法是不打包图片加载器，开发者自己传一个图片加载器的对象进去。我曾经见过有开源项目这么做过。

### ChatItemView使用方法：
首先
```
@BindView(R.id.selectPictureView)QPhotoView selectPictureView;
private ChatListAdapter adapter;
private List<ChatDataItemBean> list;
```
声明一些测试数据，先声明一些图片的地址
```
private String[] a = {"http://···/1502950842800.png", "http://···/1503109435882.png", "https://···f55a03b312c8fcc3ce2d55.jpg", "https://···12c8fcc3ce2d55.jpg",
            "https://···2db00baa1cd112af1.jpg", "https://···f3f50e1f81a4c500fa2d5.jpg",
            "https://···76740202&fm=26&gp=0.jpg", "https://···3&fm=200&gp=0.jpg",
            "https://···2F5412701200a0b.jpg", "https://···2F15%2F3K4UE1AH076P.jpg", "https://···_w1228_h687.jpeg",};

```
再声明一些文字
```
private String[] b = {"你好?", "吃了吗", "你能收到吗", "你怎么了", "hello", "放假了", "开学了", "作业做完了吗", "你喜欢我吗", "见过我的小熊吗", "你怎么不俗", "你信宗教吗", "高数过来撒", "德玛西亚", "我还有梦i想", "你过来我打你", "你来我走", "这是第几条", "还好啦", "哈哈哈", "做梦", "且", "还有梦想？", "指鹿为马，还抱着梦想呢", "行i行吧", "我还没吃早饭", "你真的行i型号吧", "怎么活该没", "赞啦", "叫爸爸", "哈佛大学也不去", "真是服了你", "够菜了", "呵呵", "如意十大打撒", "啊实打实大了扩大", "伊斯兰", "基督教", "山东纵波", "淄博", "沂源县", "心动刚", "哇狙击", "中国人"};

```
再声明一些名字
```
private String[] d = {"张飞", "赵云", "李四", "诸葛亮", "刘备", "亚瑟", "蓝令我", "天的撒娇啊是的", "我家兴", "赵自强", "和恶化", "你猜", "没有id", "指尖的温柔", "咖啡猫", "青苔你住"};

```
最后声明一个List，存1-120个数字，120个数字随机取 算作语音时长。
```
    private String[] c= new String[120];
    for (int o = 0; o < 120; o++) {
          c[o] = "" + o;
    }

```
然后，随机生成一个List.
```
        Random r = new Random();
        for (int o = 0; o < 100; o++) {
            list.add(new ChatDataItemBean(e[r.nextInt(e.length)], a[1], a[r.nextInt(a.length)], a[r.nextInt(a.length)], b[r.nextInt(b.length)], c[r.nextInt(c.length)], d[r.nextInt(d.length)]));
        }
```
其中这里边主要调用了ChatDataItemBean的构造方法:
```
    public ChatDataItemBean(ChatType chatType, String myPhoto, String hisPhoto, String picturePath, String text, String durtation,String name) {
            this.hisname = name;
            this.chatType = chatType;
            this.myPhoto = myPhoto;
            this.hisPhoto = hisPhoto;
            PicturePath = picturePath;
            this.text = text;
            this.durtation = durtation;
        }
```
其中ChatType是个枚举类，分别代表六种信息类型，构造如下：
```
public enum ChatType {
    textLeft ,//消息类型为左边文字
    textRight,//消息类型为右边文字
    pictureLeft,//消息类型为左边图片
    pictureRight,//消息类型为右边文字
    voiceLeft,//消息类型为左边语音
    voiceRight;//消息类型为右边语音
}
```
最后用这些：
```
        LinearLayoutManager layoutManager = new LinearLayoutManager(context);
        layoutManager.setOrientation(LinearLayoutManager.VERTICAL);
        recyclerView.setLayoutManager(layoutManager);
        recyclerView.setHasFixedSize(true);
        recyclerView.setLayoutManager(new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, false));
        recyclerView.setAdapter(adapter = new ChatListAdapter(list, context, R.layout.chat_item_layout));
```
下面是适配器的代码：
```
package com.xiaocool.sugarangel.adapter;
import android.content.Context;
import android.content.Intent;
import android.support.v7.widget.RecyclerView;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import com.example.mylibrary.Chat.ChatItemView;
import com.example.mylibrary.Chat.OnChatViewClickListener;
import com.xiaocool.sugarangel.R;
import com.xiaocool.sugarangel.bean.ChatDataItemBean;
import java.util.List;
import com.example.mylibrary.Chat.ChatType.*;
import com.example.mylibrary.Chat.photoType;
/**
 * Created by 赵自强 on 2017/8/25 20:26.
 * 这个类的用处
 */
public class ChatListAdapter extends RecyclerView.Adapter<ChatListAdapter.viewHolder> {
    private List<ChatDataItemBean> list;
    private Context context;
    private int LayoutId;
    public ChatListAdapter(List<ChatDataItemBean> list, Context context, int layoutId) {
        this.list = list;
        this.context = context;
        LayoutId = layoutId;
    }
    public void setList(List<ChatDataItemBean> list) {
        this.list = list;
    }
    @Override
    public viewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        return new viewHolder(LayoutInflater.from(context).inflate(LayoutId, parent, false));
    }
    @Override
    public void onBindViewHolder(viewHolder holder, final int position) {
        ChatDataItemBean chatItemView = list.get(position);
        switch (chatItemView.getChatType()){
            case textLeft:
                holder.chatItemView.setLeftTextType(chatItemView.getHisname(),chatItemView.getText(),chatItemView.getHisPhoto());
                break;
            case textRight:
                holder.chatItemView.setRightTextType("我",chatItemView.getText(),chatItemView.getMyPhoto());
                break;
            case pictureLeft:
                holder.chatItemView.setLeftPictureType(chatItemView.getHisname(),chatItemView.getPicturePath(),chatItemView.getHisPhoto());
                break;
            case pictureRight:
                holder.chatItemView.setRightPictureType("我",chatItemView.getPicturePath(),chatItemView.getMyPhoto());
                break;
            case voiceLeft:
                holder.chatItemView.setLeftVoiceType(chatItemView.getHisname(),chatItemView.getDurtation(),chatItemView.getHisPhoto());
                break;
            case voiceRight:
                holder.chatItemView.setRightVoiceType("我",chatItemView.getDurtation(),chatItemView.getMyPhoto());
                break;
        }
        holder.chatItemView.setOnChatViewClickListener(new OnChatViewClickListener() {
            /*头像的点事件监听*/
            @Override
            public void OnPhotoClicked(View view, photoType photoType) {
                switch ( photoType){
                    case left:
                        Log.i("ChatViewClickListener:", "    "+ position+"条数据：" +"左头像被点击");
                        break;
                    case right:
                        Log.i("ChatViewClickListener:","    "+ position+"条数据：" +"右头像被点击");
                        break;
                }
            }

            @Override
            public void OnTextViewLongClicked(View view) {
                Log.i("ChatViewClickListener", "    "+ position+"条数据：" +"文本被长按");
            }

            @Override
            public void OnPlayButtonClickedListener(View view) {
                Log.i("ChatViewClickListener","    "+ position+"条数据：" +"播放按钮被点击");
            }

            @Override
            public void OnPictureClickedListener(View view) {
                Log.i("ChatViewClickListener", "    "+ position+"条数据：" +"照片被点击");
            }

            @Override
            public void OnPictureLongClickedListener(View view) {
                Log.i("ChatViewClickListener","    "+ position+"条数据：" +"照片被长按");
            }
        });
    }

    @Override
    public int getItemCount() {
        return list.size();
    }

    final class viewHolder extends RecyclerView.ViewHolder {
        private ChatItemView chatItemView;
        public viewHolder(View itemView) {
            super(itemView);
            chatItemView = (ChatItemView) itemView.findViewById(R.id.c);
        }
    }
}

```
### ChatItemView自定义标签：
标签| 定义 | 单位
----|-----|------
show_nickname|是否显示名字|枚举类：0,1,2，3分别代表都不显示名字，只显示左边名字，只显示右边名字，左右两边的名字都显示。
LeftPhotoSize|左边头像的大小|dimension
RightPhotoSize|右边头像的大小|dimension
DefaultLeftChatTextSize|左边聊天文字的大小|dimension
DefaultRightChatTextSize|右边聊天文字的大小|dimension
DefaultNicknameTextSizeLeft|左边昵称文字大小|dimension
DefaultNicknameTextSizeRight|右昵称文字大小|dimension
DefaultNicknameTextColorLeft|左边聊天框内文字颜色|color
DefaultNicknameTextColorRight|右边聊天框内文字颜色|color
DefaultChatTextColorLeft|左边聊天框内文字大小|color
DefaultChatTextColorRight|右边聊天框内文字大小|color
### ChatItemView方法：
方法| 参数| 用途
----|-----|------
setOnChatViewClickListener|OnChatViewClickListener listener|添加监听器
setLeftTextType|String nickname, String text , String path|如果是左边文字的情况，需要设置昵称，聊天的文字以及左边的头像
setRightTextType|String nickname, String text , String path|如果是右边文字的情况，需要设置昵称，聊天的文字以及右的头像
setLeftPictureType|String nickname,String PicturePath,String photoPath|如果是左边图片的情况，需要设置昵称，聊天的图片以及左边的头像
setRightPictureType|String nickname,String PicturePath,String photoPath|如果是右边图片的情况，需要设置昵称，聊天的图片以及右边的头像
setLeftVoiceType|String nickname,String duration,String photoPath|如果是左边图片的情况，需要设置昵称，聊天的图片以及左边的头像
setRightVoiceType|String nickname ,String duration,String photoPath|如果是右边图片的情况，需要设置昵称，聊天的图片以及右边的头像
### 监听事件方法：
方法| 参数| 用途
----|-----|------
OnPhotoClicked|View view,photoType photoType|photoType用来判断是左头像还是右头像
OnTextViewLongClicked|View view|文本长按监听
OnPlayButtonClickedListener|View view|播放按钮点击事件监听
OnPictureClickedListener|View view|图片点击事件监听
OnPictureLongClickedListener|View view|图片长按事件监听

## 2.初步仿QQ录音效果（QRecordAudioView）
### 效果图
qq效果图
![qq效果图](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/qqanniu.gif)
demo效果图
![demo](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/demo.gif)
### 特点：
###### 1.点击中间按钮，按钮的半径会突然变小再变大
###### 2.焦点向左或者向右移动时，左边的按钮的大小就会变化，有最值且能变回原来大小。
###### 3.上方有一个随分贝变化的线。
###### 4.当在左边或右边的圆圈中失去焦点后会有相应的事件响应，当焦点在两边的圈内时，上方的文字会有相应变化。
### demo缺点：
###### 1.还不能随分贝变化
###### 2.留下了在两边按钮松手时的响应事件。
###### 3.还是有点丑。
### QRecordAudioView使用方法
###### 首先定义一个控件：
```
   <com.example.mylibrary.RecordAudioView.QRecordAudioView
        android:visibility="gone"
        android:id="@+id/recordVoiceView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```
所有的动画效果都由控件自己完成，只需要在控制器中设置监听器就ok。
```
recordVoiceView.setRecordInterfaceListener(new RecordInterfaceListener() {
            @Override
            public void OnStartRecord(View ClickView) {
                //当点击录制按钮式应采取的操作
            }

            @Override
            public void OnDeleteRecordSeleted() {
                //当在删除按钮松手时应该采取的操作
            }

            @Override
            public void OnPlayRecordSeleted() {
                //当在播放按钮松手时应该采取的操作
            }

            @Override
            public void OnRecordFinish(int Duration) {
                //当录制声音完成时要采取的操作
            }
        });
```
### QRecordAudioView监听器中需要实现的方法：
方法| 参数
--------|-------------
OnStartRecord|当点击录制按钮式应采取的操作
OnDeleteRecordSeleted|当在删除按钮松手时应该采取的操作
OnPlayRecordSeleted| int 当在播放按钮松手时应该采取的操作
OnRecordFinish|当录制声音完成时要采取的操作
## 3.初步仿相册发送图片时的页面显示（QPhotoView）
### QPhotoView效果图
qq效果图
![qq效果图](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/qq.gif)
demo效果图
![demo](https://raw.githubusercontent.com/Exclamation-mark/tool/master/appearance/demo0.gif)
### 特点：
###### 1.按照时间顺序显示图片，且图片是等高，按比例伸缩图片
###### 2.选中多图时有计数功能，而且选中的图可以自动取消选中，其它图改变次序。
###### 3.可以跳到相册中选择其它图，最多选择20张图
###### 4.只有在有图片选中时，发送按钮才可以点击且发送按钮显示选中的图片的数量。
###### 5.只有在选中图片的数量是一张的时候，才可以选择裁剪功能。
### demo缺点：
###### 1.没有写裁剪和相册以及发送的点击事件，只是留了监听接口
###### 2.QQ的可以摁住图片往上拖直接发送，没有做这个功能。
###### 3.没测过性能怎么样
### QPhotoView使用方法
直接在页面布局中使用下面的代码：
```
    <com.example.mylibrary.photoListView.QPhotoView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:visibility="gone"
        app:maxSeleteCount="20"
        android:id="@+id/selectPictureView"/>
```
然后是添加监听事件响应Click就可以了：
```
selectPictureView.setQPhotoListener(new QPhotoListener() {
            @Override
            public void onAlbumClicked() {
                Log.i("QPhotoListener","点击了去相册按钮");
            }

            @Override
            public void OnSendBtnClicked(int picCount, List<PictureDataBean> selectedPicList) {
                Log.i("QPhotoListener","发送" + picCount +"张图片");
            }

            @Override
            public void OnCropClicked(String picPath) {
                Log.i("QPhotoListener","裁剪图片地址：" +picPath);
            }
        });
```
### QPhotoView自定义标签：
标签| 定义 | 单位
----|-----|------
showCrop|是否启用裁剪功能|boolean
sendBtnBackground|发送按钮背景|reference
albumTextColor|相册文字颜色|color
cropEnabledTextColor|裁剪按钮可点击时的颜色|colorcolor
cropNoUnEnableTextColor|裁剪按钮不可点击时的颜色|colorcolor
cropNoUnEnableTextColor|裁剪按钮不可点击时的颜色|colorcolor
sendEnabledTextColor|发送按钮可点击时的颜色|colorcolor
sendNoUnEnableTextColor|发送按钮不可点击时的颜色|colorcolor
maxSeleteCount|最多可选择的照片数量|integer
### QPhotoView监听器相关方法：
方法名|参数| 用途
----|-----|------
onAlbumClicked|nulll|当点击相册时采取的操作
OnSendBtnClicked|int picCount, List<PictureDataBean> selectedPicList|点击发送时采取的操作，
OnCropClicked|String picPath|点击裁剪时采取的操作

## 4.圆角图片
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
## 总结：
总代码水平，对java的理解和android的理解上还不够，但是看到自己心中想像的东西都被自己一点一点的敲了出来，我还是感到了莫大的满足与开心。而且在这个过程中不但学到了东西而且看到了自己的不足，简直赚翻了。


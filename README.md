
开源地址：[https://github.com/open-android/DragFooterView](https://github.com/open-android/DragFooterView "开源项目地址") 
一个向左拖拽跳转至更多页面的通用控件    


![screenshot](/DragFooterView/screenshot/demo.gif)
 
 
![screenshot](/DragFooterView/screenshot/inspiration.gif)  

## 自定义你自己的Footer效果   
作为一个library，当然不能只支持以上那一种效果啦，所以，这个库的
Footer应该是可定制的，可插拔的。定制Footer只需定义一个继承自
BaseFooterDrawer的类，然后在参数中提供的区域中绘制即可，而其余
的事件分发，拦截都不需要关心。以下是我自己定制的两种Footer效果。     
![screenshot](/DragFooterView/screenshot/custom2.gif)     
![screenshot](/DragFooterView/screenshot/custom1.gif)    

## 使用步骤

### 1. 在project的build.gradle添加如下代码(如下图)

	allprojects {
	    repositories {
	        ...
	        maven { url "https://jitpack.io" }
	    }
	}

![](http://oi5nqn6ce.bkt.clouddn.com/itheima/booster/code/jitpack.png)


### 2. 在Module的build.gradle添加依赖

    compile 'com.github.open-android:DragFooterView:0.1.0'

### 用法
1、在xml中配置如下 **(注意：DragContainer只能有一个子View)**
```xml
    <com.fangxu.library.DragContainer
        android:id="@+id/drag_recycler_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp">

        <android.support.v7.widget.RecyclerView
            android:id="@+id/recycler_view"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@android:color/white" />
    </com.fangxu.library.DragContainer>
```
2、在java类中添加事件监听器DragListener
```java
    DragContainer dragContainer = (DragContainer) findViewById(R.id.drag_recycler_view);
    
    //若需使用自己定制的footer，需要调用DragContainer的setFooterDrawer方法设置定制的footer类，如下
    dragContainer.setFooterDrawer(new ArrowPathFooterDrawer.Builder(this, 0xff444444).setPathColor(0xffffffff).build());
    
    dragContainer.setDragListener(new DragListener() {
        @Override
        public void onDragEvent() {
            //do whatever you want,for example skip to the load more Activity.
            Intent intent = new Intent(HomeActivity.this, ShowMoreActivity.class);
            startActivity(intent);
        }
    });
```

## 属性
|attribute|value type|defalut value| description|
| --- | --- | --- | --- |
|dc_footer_color|color|0xffcdcdcd|footer view的背景颜色|
|dc_reset_animator_duration|integer|700|松开拖拽后复位动画的时长|
|dc_drag_damp|float|0.5f|拖拽阻尼系数，取值在(0,1]之间，取值越小，阻尼越大|




开源地址：[https://github.com/open-android/DragFooterView](https://github.com/open-android/DragFooterView "开源项目地址") 
一个向左拖拽跳转至更多页面的通用控件    


![demo](/DragFooterView/screenshot/demo.gif)
 
 
![inspiration](/DragFooterView/screenshot/inspiration.gif)  

* 详细的使用方法在DEMO里面都演示啦,如果你觉得这个库还不错,请赏我一颗star吧~~~

* 欢迎关注微信公众号、长期为您推荐优秀博文、开源项目、视频

![](http://upload-images.jianshu.io/upload_images/4037105-8f737b5104dd0b5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 自定义你自己的Footer效果   
作为一个library，当然不能只支持以上那一种效果啦，所以，这个库的
Footer应该是可定制的，可插拔的。定制Footer只需定义一个继承自
BaseFooterDrawer的类，然后在参数中提供的区域中绘制即可，而其余
的事件分发，拦截都不需要关心。以下是我自己定制的两种Footer效果。 

![custom2](/DragFooterView/screenshot/custom2.gif)     

![custom1](/DragFooterView/screenshot/custom1.gif)    

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
1、在xml中配置如下 **(注意：DragContainer只能有一个子View)**,RecyclerView向左拖拽
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
    @Override
    public void onDragEvent() {
        Intent intent = new Intent(HomeActivity.this, ShowMoreActivity.class);
        startActivity(intent);
    }
```

## 属性
|attribute|value type|defalut value| description|
| --- | --- | --- | --- |
|dc_footer_color|color|0xffcdcdcd|footer view的背景颜色|
|dc_reset_animator_duration|integer|700|松开拖拽后复位动画的时长|
|dc_drag_damp|float|0.5f|拖拽阻尼系数，取值在(0,1]之间，取值越小，阻尼越大|

* 细节注意：
  //若需使用自己定制的footer，需要调用DragContainer的setFooterDrawer方法设置定制的footer类，如下
    dragContainer.setFooterDrawer(new ArrowPathFooterDrawer.Builder(this, 0xff444444).setPathColor(0xffffffff).build());
    
### 其他控件用法 (HorizontalScrollView用法)
 ```xml
 <com.fangxu.library.DragContainer
                android:id="@+id/drag_scroll_view"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                app:dc_reset_animator_duration="500">

                <HorizontalScrollView
                    android:id="@+id/scroll_view"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:background="@android:color/white"
                    android:scrollbars="none">

                    <LinearLayout
                        android:id="@+id/linear_layout"
                        android:layout_width="match_parent"
                        android:layout_height="170dp"
                        android:orientation="horizontal" />

                </HorizontalScrollView>

            </com.fangxu.library.DragContainer> 
```
```java
 private void setupHorizontalScrollView() {
        LinearLayout linearLayout = (LinearLayout) findViewById(R.id.linear_layout);
        for (int i = 10; i < 20; i++) {
            ImageView imageView = new ImageView(this);
            LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(dp2px(120), ViewGroup.LayoutParams.MATCH_PARENT);
            params.leftMargin = 0;
            params.rightMargin = dp2px(5);
            imageView.setScaleType(ImageView.ScaleType.CENTER_CROP);
            imageView.setLayoutParams(params);
            linearLayout.addView(imageView);
            Glide.with(this).load(Constants.urls[i]).into(imageView);
        }

        DragContainer dragContainer = (DragContainer) findViewById(R.id.drag_scroll_view);
        BaseFooterDrawer drawer = new com.fangxu.dragfooterview.customfooters.ArrowPathFooterDrawer.Builder(this, 0xff444444).setPathColor(0xffffffff).build();
        dragContainer.setFooterDrawer(drawer);
        dragContainer.setDragListener(this);
    }
```	    

### (ImageView用法)
 ```xml
 <com.fangxu.library.DragContainer
                android:id="@+id/drag_image_view"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="10dp"
                app:dc_drag_damp="0.66"
                app:dc_reset_animator_duration="500">

                <ImageView
                    android:id="@+id/image_view"
                    android:layout_width="150dp"
                    android:layout_height="200dp"
                    android:scaleType="centerCrop" />

            </com.fangxu.library.DragContainer>
 ```
 ```java
  private void setupImageView() {
        ImageView imageView = (ImageView) findViewById(R.id.image_view);
        Glide.with(this).load(Constants.urls[0]).into(imageView);

        DragContainer dragContainer = (DragContainer) findViewById(R.id.drag_image_view);
        dragContainer.setFooterDrawer(new BezierFooterDrawer.Builder(this, 0xffffc000).setIconDrawable(getResources().getDrawable(R.drawable.left)).build());
        dragContainer.setDragListener(this);
    }
   ```
   
 ###  (TextView, Button用法)
 
 ```xml
   
    <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="20dp"
                android:orientation="horizontal">

                <com.fangxu.library.DragContainer
                    android:id="@+id/drag_text_view"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_marginLeft="10dp"
                    android:layout_marginRight="10dp"
                    android:layout_weight="1"
                    app:dc_reset_animator_duration="500">

                    <TextView
                        android:id="@+id/text_view"
                        android:layout_width="match_parent"
                        android:layout_height="80dp"
                        android:background="#66ee66"
                        android:gravity="center"
                        android:scaleType="centerCrop"
                        android:text="TextView" />

                </com.fangxu.library.DragContainer>

                <com.fangxu.library.DragContainer
                    android:id="@+id/drag_button"
                    android:layout_width="0dp"
                    android:layout_height="wrap_content"
                    android:layout_marginLeft="10dp"
                    android:layout_weight="1"
                    app:dc_reset_animator_duration="500">

                    <Button
                        android:id="@+id/button"
                        android:layout_width="match_parent"
                        android:layout_height="80dp"
                        android:background="#ff6600"
                        android:scaleType="centerCrop"
                        android:text="Button" />

                </com.fangxu.library.DragContainer>

            </LinearLayout>
 ```
 ```java
 private void setupTextView() {
        DragContainer dragContainer = (DragContainer) findViewById(R.id.drag_text_view);
        dragContainer.setDragListener(this);
    }

    private void setupButton() {
        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(HomeActivity.this, "onClick", Toast.LENGTH_SHORT).show();
            }
        });
        button.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                Toast.makeText(HomeActivity.this, "onLongClick", Toast.LENGTH_SHORT).show();
                return true;
            }
        });

        DragContainer dragContainer = (DragContainer) findViewById(R.id.drag_button);
        dragContainer.setDragListener(this);
    }
	    
 ```    

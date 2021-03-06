+++
title = "Android SurfaceView 截屏方案探索"
date = "2017-09-14"
tags = ["android","第三方"]
categories = ["android"]
+++

### 前言
今年 6月底开始着手公司直播 App 的重写工作，整个 app 是要推倒用 kotlin 重写，重写的同时也增加了旧版所没有的新功能，
其中，直播间的截图分享功能就是其中一个。

### 直奔主题
首先，通过视图基类 View 的 `getDrawingCache()` 方法获取整个直播间截图只是个美好的愿望，调用直播间根布局的 `getDrawingCache()` 方法，
除了 SurfaceView 之外的其他视图，确实能出现在截图中，但理应出现在 SurfaceView 上的画面却是黑的，或者说是空的，无法通过此方法获取画面的数据。

简单来说，SurfaceView 由 Surface 和 View 两部分所组成，而 SurfaceView 的 view 部分只是一个透明的占位符，内容却是渲染在 Surface 上的,
其中底层组件 BufferQueue 提供了 `将可生成图形数据缓冲区的组件（生产者）连到接受数据以便进行显示或进一步处理的组件（消费者）`的接口，生成者如 Canvas，openGL ES ，一旦数据被消费者接受，就无法通过获取这些数据来创建 Bitmap 了，但解决的招数还是有的：

1. 对于使用 Canvas 渲染，渲染两次，其中一次用于创建截图
2. 对于使用 GLES 渲染的，可以使用 `glReadPixels()` 方法创建 Bitmap

在项目我采用的是第二种方法，虽然使用 GLES 时并不非要 GLSurfaceView，但却省事不少，更省事的是我无需去造一个轮子，因为已经有人给出了实现

https://github.com/tarek360/Instacapture  (原repo, 支持Rxjava1)
https://github.com/nyakokishi/Instacapture  (本人 fork 的 repo, 支持Rxjava2)

这个截屏轮子，支持 Android 4.0 以上的 GLSurfaceView 和 TextureView 的截屏，注意并不支持 SurfaceView，使用方法也简单,
当然了，它会遍历当前整个页面的 View 树，完完整整的绘制出一张屏幕截图

### 用法示例：

```
Instacapture.INSTANCE.capture(activity, new SimpleScreenCapturingListener() {
    @Override
    public void onCaptureComplete(Bitmap bitmap) {
       //Your code here..
    }
}, ignoredViews);

// or in Rx way
Instacapture.INSTANCE.captureRx(this, ignoredViews).subscribe(new Action1<Bitmap>() {
    @Override
    public void call(Bitmap bitmap) {
        //Your code here..
     }
});
```

### 参考资料
https://stackoverflow.com/questions/27817577/android-take-screenshot-of-surface-view-shows-black-screen

https://source.android.com/devices/graphics/architecture

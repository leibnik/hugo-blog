title: 使用Bmob后端云遇到的坑
date: 2016-02-27
tags: [android,第三方]
categories: android
layout: post
---
最近尝试使用Bmob提供的后端云，请求响应速度还行，功能上基本能满足开发需求，期间遇到一些坑，查了文档也没见说明，在此做一下记录。

<!-- more -->

比如像微信朋友圈那样发一条带图片的动态。假如是这样的一个类：
```java
public class Daily extends BmobObject{
	
	private static final long serialVersionUID = 1L;

	private String text;

	// 动态与用户是一对一个关系
	private User user;

	private List<String> images;

	// 下面是getter/setter...

}

```

应该考虑到若上传了图片等文件，需要先返回文件的服务器地址，再上传一条记录，确保一条记录完整地添加到数据库表。


当上传一张图片时，可以使用如下代码：
```java
final Daily daily = new Daily();
String picPath = "sdcard/temp.jpg";
BmobFile bmobFile = new BmobFile(new File(picPath));
bmobFile.uploadblock(context, new UploadFileListener() {

    @Override
    public void onSuccess() {
        //bmobFile.getUrl()---返回的上传文件的地址（不带域名）
        //bmobFile.getFileUrl(context)--返回的上传文件的完整地址（带域名）
        User user = BmobUser.getCurrentUser(context,User.class);
        // 设置关联
        daily.setUser(user);
        daily.setImages(new List<String>().add(bmobFile.getFileUrl(context)));
        daily.save(context, new SaveListener() {
            @Override
            public void onSuccess() {
                Log.w("动态发送成功:" + bmobFile.getFileUrl(context));
            }

            @Override
            public void onFailure(int i, String s) {
                
            }
        });

        Log.w("上传文件成功:" + bmobFile.getFileUrl(context));
    }

    @Override
    public void onProgress(Integer value) {
        // TODO Auto-generated method stub
        // 返回的上传进度（百分比）
    }

    @Override
    public void onFailure(int code, String msg) {
        // TODO Auto-generated method stub
        Log.w("上传文件失败：" + msg);
    }
});
```

当发送的图片文件大于一张时，可以使用如下代码：
```java
String filePath_mp3 = "/mnt/sdcard/testbmob/test1.png";
String filePath_lrc = "/mnt/sdcard/testbmob/test2.png";
String[] filePaths = new String[2];
filePaths[0] = filePath_mp3;
filePaths[1] = filePath_lrc;
Bmob.uploadBatch(context, filePaths, new UploadBatchListener() {

    @Override
    public void onSuccess(List<BmobFile> files,List<String> urls) {
        // TODO Auto-generated method stub
        //1、files-上传完成后的BmobFile集合，是为了方便大家对其上传后的数据进行操作，例如你可以将该文件保存到表中
        //2、urls-上传文件的服务器地址
    }

    @Override
    public void onError(int statuscode, String errormsg) {
        // TODO Auto-generated method stub
        ShowToast("错误码"+statuscode +",错误描述："+errormsg);
    }

    @Override
    public void onProgress(int curIndex, int curPercent, int total,int totalPercent) {
        // TODO Auto-generated method stub
        //1、curIndex--表示当前第几个文件正在上传
        //2、curPercent--表示当前上传文件的进度值（百分比）
        //3、total--表示总的上传文件数
        //4、totalPercent--表示总的上传进度（百分比）
    }
});
```

上传多张图片时只需新建一个数组传进`Bmob.uploadBatch()`方法里面就可以了。而我要说的坑就在这里，假如我用下面的代码发送含三张图片的动态。


```java
final Daily entity = new Daily();
Bmob.uploadBatch(context, filesPath, new UploadBatchListener() {
    @Override
    public void onSuccess(List<BmobFile> list, List<String> list1) {

        User user = BmobUser.getCurrentUser(context, User.class);
        entity.setUser(user);
        entity.setImages(list1);
        entity.save(context, new SaveListener() {
            @Override
            public void onSuccess() {
                Log.w("我的动态", "发送成功");
            }

            @Override
            public void onFailure(int i, String s) {
                Log.w("我的动态", "发送失败：" + s);
            }
        });
    }

    @Override
    public void onProgress(int i, int i1, int i2, int i3) {

    }

    @Override
    public void onError(int i, String s) {
        Log.w("上传文件", "发送失败：" + s);
    }
});
```

按官方文档的说法，很简单地就把上面代码中的`list1`理解成所有文件上传成功后返回的三张图片的服务器地址。然而运行程序后，日志显示的却是如下
```java
W/我的动态: 发送成功
W/我的动态: 发送失败：It is a reserved field: createdAt.
W/我的动态: 发送失败：It is a reserved field: objectId.
```

说明第一个的`onSuccess()`方法被调用了三次，也就是说每上传一次文件并且成功上传就调用一次，而不是所有文件都上传成功后才调用。而后面的两个发送失败
是重复使用同一`Daily`对象调用`save()`和`setUser()`方法所导致的。

而解决的方法非常简单，加个判断语句就行了。代码如下：

```java
final Daily entity = new Daily();
Bmob.uploadBatch(context, filesPath, new UploadBatchListener() {

    @Override
    public void onSuccess(List<BmobFile> list, List<String> list1) {

    	// 确保所有图片上传完成后再进行下一步操作
    	if(list.size() == filesPath.length){
    		User user = BmobUser.getCurrentUser(context, User.class);
        	entity.setUser(user);
        	entity.setImages(list1);
        	entity.save(context, new SaveListener() {
	            @Override
	            public void onSuccess() {
	                Log.w("我的动态", "发送成功");
	            }

	            @Override
	            public void onFailure(int i, String s) {
	                Log.w("我的动态", "发送失败：" + s);
	            }
        	});
    	}
        
    }

    @Override
    public void onProgress(int i, int i1, int i2, int i3) {

    }

    @Override
    public void onError(int i, String s) {
        Log.w("上传文件", "发送失败：" + s);
    }
});
```
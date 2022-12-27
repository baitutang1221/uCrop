

### 一款极具观赏性的图片裁剪库


#### 运行效果
![](https://github.com/Yalantis/uCrop/raw/master/preview.gif)




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

    compile 'com.github.open-android:uCrop:v1.0.0'


### 3. 复制如下代码到xml

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">
	
	    <Button
	        android:onClick="start"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:text="开始裁剪图片"/>
	
	    <ImageView
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:id="@+id/iv"/>
	</LinearLayout>

### 4. 复制如下代码到activity中 , 并且在onCreate方法查找ImageView

		private ImageView mIv;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	
	        mIv = (ImageView) findViewById(R.id.iv);
	    }

		public void start(View view){
	        //需要裁剪的图片路径
	        Uri sourceUri = Uri.fromFile(new File(Environment.getExternalStorageDirectory() , "icon_18.jpg"));
	
	        //裁剪完毕的图片存放路径
	        Uri destinationUri = Uri.fromFile(new File(Environment.getExternalStorageDirectory() , "icon_18_2.jpg"));
	
	        UCrop.of(sourceUri, destinationUri) //定义路径
	                .withAspectRatio(4, 3) //定义裁剪比例 4:3 ， 16:9
	                .withMaxResultSize(100, 100) //定义裁剪图片宽高最大值
	                .start(this);
	    }
	
	    @Override
	    public void onActivityResult(int requestCode, int resultCode, Intent data) {
	
	        //裁剪成功后调用
	        if (resultCode == RESULT_OK && requestCode == UCrop.REQUEST_CROP) {
	            final Uri resultUri = UCrop.getOutput(data);
	            //设置裁剪完成后的图片显示
	            mIv.setImageURI(resultUri);
	
	            //出错时进入该分支
	        } else if (resultCode == UCrop.RESULT_ERROR) {
	            final Throwable cropError = UCrop.getError(data);
	        }
	    }

### 5. 在清单文件注册以下activity ，该activity是裁剪界面

		<activity
            android:name="com.yalantis.ucrop.UCropActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

### 6. 并且添加对SD卡的读写权限。

		 <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
	    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"></uses-permission>


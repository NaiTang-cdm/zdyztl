# ImageSelector


Android自定义状态栏

<img src="screenshot/IMG_20230228_154013 (2).jpg" width=280/>

## 依赖
```
buildscript {
    repositories {
        ...
        maven { url "https://jitpack.io" }
    }
    dependencies {
        ...
    }
}
```

```
dependencies {
    implementation 'com.github.smuyyh:ImageSelector:3.0'
}
```

## 版本

**V0.1.1 迁移到jitpack**

**V0.1.0 适配 android 15 分区存储**

## 注意事项

1. 图片加载由调用者自定义一个ImageLoader（详见[使用方式](#使用方式)）, 可通过Glide、Picasso等方式加载
2. 谢谢~~

## 使用

### 初始化
```java
// 自定义图片加载器
ISNav.getInstance().init(new ImageLoader() {
    @Override
    public void displayImage(Context context, String path, ImageView imageView) {
        Glide.with(context).load(path).into(imageView);
    }
});
```

### 直接拍照

```java
ISCameraConfig config = new ISCameraConfig.Builder()
        .needCrop(true) // 裁剪
        .cropSize(1, 1, 200, 200)
        .build();

ISNav.getInstance().toCameraActivity(this, config, REQUEST_CAMERA_CODE);
```

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == REQUEST_CAMERA_CODE && resultCode == RESULT_OK && data != null) {
        String path = data.getStringExtra("result"); // 图片地址
        tvResult.append(path + "\n");
    }
}
```

### 图片选择器

```java

// 自由配置选项
ISListConfig config = new ISListConfig.Builder()
    // 是否多选, 默认true
    .multiSelect(false)
    // 是否记住上次选中记录, 仅当multiSelect为true的时候配置，默认为true
    .rememberSelected(false)
    // “确定”按钮背景色
    .btnBgColor(Color.GRAY)
    // “确定”按钮文字颜色
    .btnTextColor(Color.BLUE)
    // 使用沉浸式状态栏
    .statusBarColor(Color.parseColor("#3F51B5"))
    // 返回图标ResId
    .backResId(android.support.v7.appcompat.R.drawable.abc_ic_ab_back_mtrl_am_alpha)
    // 标题
    .title("图片")
    // 标题文字颜色
    .titleColor(Color.WHITE)
    // TitleBar背景色
    .titleBgColor(Color.parseColor("#3F51B5"))
    // 裁剪大小。needCrop为true的时候配置
    .cropSize(1, 1, 200, 200)
    .needCrop(true)
    // 第一个是否显示相机，默认true
    .needCamera(false)
    // 最大选择图片数量，默认9
    .maxNum(9)
    .build();
        
// 跳转到图片选择器
ISNav.getInstance().toListActivity(this, config, REQUEST_LIST_CODE);
```

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    // 图片选择结果回调
    if (requestCode == REQUEST_CODE && resultCode == RESULT_OK && data != null) {
        List<String> pathList = data.getStringArrayListExtra("result");
        for (String path : pathList) {
            tvResult.append(path + "\n");
        }
    }
}
```

## License

```
   Copyright (c) 2016 smuyyh. All right reserved.

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```

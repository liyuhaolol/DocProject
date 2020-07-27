# 一些Moudle的使用方法

- 要先把对应的.config文件夹放置项目根目录

- 在/build.gradle中添加代码导入对应的文件

```gradle
apply from: ".config/config.gradle"
apply from: ".config/plugin.gradle"
```

## moudle-ft_audio

- 初始化方法，推荐在Application.onCreate中调用

```java
//音频SDK初始化
AudioHelper.init(this);
```

- 调用方法

```java
//直接转跳到电台列表页即可->RadioActivity
intent = new Intent(MainActivity.this, RadioActivity.class);
startActivity(intent);
```

- 需要注意的资源文件

- 前言：经过我自己的测试，理论上app结构下的重名文件会直接覆盖moudle中的资源文件，所以要不要
替换这些资源文件取决于你自己实测的测试结果。以下的资源文件，最好测试一下，如果害怕可以直接替换

```java
res/drawable/jpush_notification_icon
//此文件为状态栏小图标，不是所有系统都有显示，推荐使用原生系统测试
res/values/colors
//对应的颜色，需要调整，根绝设计图验证颜色
```

## moudle-ft_newspaper

- 调用方法

```java
//显示对应的fragment即可->PaperListFragment
//当前演示在activity中显示，viewpager等对应显示即可
fragmentManager = this.getSupportFragmentManager();
transaction = fragmentManager.beginTransaction();
paperListFragment = new PaperListFragment();
transaction.add(R.id.fl_main, paperListFragment);
transaction.commit();
```

## moudle-lib_pullalive

```java
//调用对应方法拉起保活服务
//当前项目里，实在启动页权限申请后拉起，具体问题具体分析
AliveJobService.start(this);
```

## moudle-ft_videoplayer

- 此moudle后期如果可以，会改变成class文件

- 在对应的界面导入布局

```xml
<spa.lyh.cn.ft_videoplayer.SpaPlayer
    android:id="@+id/spaplayer"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

- 使用方法需参考jiaozi视频的api：https://github.com/Jzvd/JiaoZiVideoPlayer

## moudle-ft_location

- 初始化方法

```java
//在Application.onCreate中调用百度地图的初始化方法
//初始化百度地图
SDKInitializer.initialize(getApplicationContext());
```

- 使用方法

```java
CnsLocationInfo.getInstance().getloc(getApplicationContext(), new CnsLocationListener() {
                @Override
                public void onSuccess(LocateInfo info) {
                }

                @Override
                public void onFailure(LocateError error) {
                    Log.e("LocateError", error.getErrorMsg());
                }
            });
```

- 显示地图组件,布局导入地图显示框架

```xml
<spa.lyh.cn.ft_location.location.map.CnsMapView
    android:id="@+id/mapview"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

```java
mapView.ReadyListener(new CnsMapView.Ready() {
            @Override
            public void ready() {
                //地图初始化成功后回调
                    });

//在地图上显示当前位置
mapView.showMap(info);
//地图状态改变
mapView.getMap().setOnMapStatusChangeListener(new BaiduMap.OnMapStatusChangeListener() {
                    @Override
                    public void onMapStatusChangeStart(MapStatus mapStatus) {
                        //Log.e("liyuhao","onMapStatusChangeStart");
                    }

                    @Override
                    public void onMapStatusChangeStart(MapStatus mapStatus, int i) {
                    }

                    @Override
                    public void onMapStatusChange(MapStatus mapStatus) {
                        //Log.e("liyuhao","onMapStatusChange");
                    }

                    @Override
                    public void onMapStatusChangeFinish(MapStatus mapStatus) {
                        //Log.e("liyuhao","onMapStatusChangeFinish");
                    }
                });
//在地图上显示marker
mapView.showMark(info,list,true);
//地图mark的点击事件
mapView.getMap().setOnMarkerClickListener(new BaiduMap.OnMarkerClickListener() {
                    @Override
                    public boolean onMarkerClick(Marker marker) {

                    }
                });                         
```

- MapUtil的使用

```java
//转跳高德地图
MapUtil.jumpToGaodeMap(MapActivity.this, bean);
//转跳百度地图
MapUtil.jumpToBaiduMap(MapActivity.this, bean);
//转跳腾讯地图
MapUtil.jumpToTengxunMap(MapActivity.this, bean);
//转跳谷歌地图
MapUtil.jumpToGoogleMap(MapActivity.this, bean);
```

## moudle-ft_jpush

- 只需要在build.gradle中引用就可以直接生效，不需要在代码里进行实际的调用

- 需要在configs.gradle中配置对应的JPUSH_KEY,否则项目会报错

- 需要注意的资源文件

- 前言：经过我自己的测试，理论上app结构下的重名文件会直接覆盖moudle中的资源文件，所以要不要
替换这些资源文件取决于你自己实测的测试结果。以下的资源文件，最好测试一下，如果害怕可以直接替换

```java
res/drawable/jpush_notification_icon

此文件为状态栏小图标，不是所有系统都有显示，推荐使用原生系统测试

res/drawable-xxhdpi/ic_launcher.png

此文件用来控制app通知的大图标显示。
因为在原生系统下大部分国外App默认都是使用的<圆形图标>。
所以这里将对应<ic_launcher_round.png>复制到此文件夹并改名成为<ic_launcher.png>
```

## moudle-share_sdk

- 只需要在build.gradle中引用就可以直接生效，不需要在代码里进行实际的调用

- 配置方法

- share_sdk使用了集成方法，所以参数提取到配置文件里，意义不大，所以必须在share_sdk/build.gradle中进行修改

```gradle
//1.src/main/assets/ic_launcher.png需替换为项目的xxxhdpi对应的图片，否则无图稿件图标显示错误
//2.各个平台对应的key和secret按照上线要求修改，无用的平台不需要删除对应内容
//3.必须提供一个图标的网络访问路径，否则twitter分享无图稿件会有问题：buildConfigField('String','ICON_URL',"")
//4.buildConfigField('String','ACTIVITE_LIST',"")来控制分享的渠道，顺序

//注意事项
//必须提供图标的网络连接，否则就不用添加twitter渠道
//如果加入facebook渠道，必须使用https，需要前方（后台）提供支持ssl证书的域名，否则就不用添加facebook渠道
//下方的平台配置

MobSDK {
    appKey "输入appKey"
    appSecret "输入appSecret"
    gui false

    //从集成文件里移除无用的权限，避免华为等平台审核时审核到你并不想添加的权限。
    permissions {
        exclude "android.permission.READ_SMS",
                "android.permission.SEND_SMS",
                "android.permission.WRITE_SMS",
                "android.permission.RECEIVE_SMS",
                "android.permission.RECEIVE_WAP_PUSH",
                "android.permission.RECEIVE_MMS",
                "android.permission.CALL_PHONE",
                "android.permission.READ_CALL_LOG",
                "android.permission.WRITE_CALL_LOG",
                "android.permission.PROCESS_OUTGOING_CALLS",
                "android.permission.WRITE_EXTERNAL_STORAGE",
                "android.permission.READ_EXTERNAL_STORAGE"
    }

//如果没有的上的平台，他的信息可以无视，留着删除都行
    ShareSDK {
        //平台配置信息
        devInfo {
            SinaWeibo {
                appKey "appKey"
                appSecret "appSecret"
                callbackUri "callbackUri"
                shareByAppClient true
                enable true
            }
            Wechat {
                appKey "appKey"
                appSecret "appSecret"
                bypassApproval false
                enable true
            }
            WechatMoments {
                appKey "appKey"
                appSecret "appSecret"
                bypassApproval false
                enable true
            }
            QQ {
                appId "appId"
                appKey "appKey"
                shareByAppClient true
                bypassApproval false
                enable true
            }
            Facebook {
                appKey "appKey"
                appSecret "appSecret"
                callbackUri "callbackUri"
                shareByAppClient true
                enable true
            }
            Twitter {
                appKey "appKey"
                appSecret "appSecret"
                callbackUri "callbackUri"
                enable true
            }
        }
    }

}
```

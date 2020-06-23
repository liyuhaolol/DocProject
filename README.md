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

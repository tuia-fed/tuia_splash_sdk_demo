
# 推啊开屏广告Android SDK 初版

[TOC]

# 产品介绍

   [推啊广告SDK 其余产品介绍](https://yun.duiba.com.cn/tuia/sdk/推啊SDK介绍.pdf) 


# 1.1 准备

## 1.1.1 合作方媒体在 [推啊媒体平台](https://ssp.tuia.cn) 注册账号
## 1.1.2 在后台获取 appKey、appSecret 等参数
## 1.1.3 申请广告位ID

## 1.2 SDK包导入

### 1.2.1 项目的build.gradle文件中添加

    buildscript {
        repositories {
            google()
            //必须添加
            jcenter()
            //必须添加
            maven { url "https://jitpack.io" }
        }
        
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
        }
    }
    
    allprojects {
            repositories {
                google()
                //必须添加
                jcenter()
                //必须添加
                maven { url "https://jitpack.io" }
            }
    }

### 1.2.2 app下的build.gradle添加：(最小支持minSdkVersion 14)

    dependencies {
        implementation ('com.tuia:sdk_tuia_splash:1.0.8-alpha01'){
        	transitive = true
    	}
    }

## 1.3 AndroidManifest配置

### 1.3.1.权限

    //(sdk内部已经处理相关权限问题，如果遇到冲突咨询对应开发即可)
    <uses-permission android:name="android.permission.INTERNET"/>
    //原生插屏必须权限
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>

### 1.3.2.设置meta-data

        <!-- 推啊appkey和appSecret从媒体管理后台获取 咨询运营人员-->
        <!-- 推啊appkey-->
        <meta-data
            android:name="TUIA_APPKEY"
            android:value="kEzAJT4iRMMag29Z7yWcJGfcVgG" />
        <!-- 推啊appSecret -->
        <meta-data
            android:name="TUIA_APPSECRET"
            android:value="3Wq4afvvdPhyHBR29LgRwEC16gkrrFXZ5BRoL2E" />

## 1.4 运行环境配置
本SDK可运行于Android4.0.3(API Level 15) 及以上版本。

    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="28" />

## 1.6 初始化SDK

### 1.6.1.初始化(重要)

    在自定义的Application 的onCreate方法中，调用以下方法：（详细内容请参考demo中的代码示例）
    
    //基础SDK初始化
    TuiaAdConfig.init(this);

# 2.加载广告 

## 1 在开屏页初始化TuiaSplashAd对象

## 2 调用TuiaSplashAd的init方法初始化并开始加载数据,在onDestroy方法中调用对象中的destroy()方法


第二步：代码引入示例

    public class SplashActivity extends BaseActivity {
    
    private TuiaSplashAd tuiaSplashAd;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sbanner);
      
       tuiaSplashAd.init(this, 5000, currentAd.getSlotId(), userId, adLayout, skipTuia, new com.maiquan.sdk_tuia_splash.SplashAdListener() {
             //设置监听
            @Override
            public void onAdPresent() {
            
            }
    
            @Override
            public void onPageLoadFinish() {
            
            }
            
            @Override
            public void onAdDismissed() {
              
            }
    
            @Override
            public void onAdFailed(String var1) {
              
            }
    
            @Override
            public void onAdClick() {
            
            }
        });
    
    }
    
    @Override
    protected void onDestroy() {
        //销毁控件
        if (tuiaSplashAd != null) {
            tuiaSplashAd.destroy();
        }
        super.onDestroy();
    }

| 名称 | 类型 | 备注 |
| :---------------------: | :---------------------: | :----------------------: |
| activity | Activity | 用户接入的开屏activity |
| jumpMillisecond | long | 默认跳过广告时间，单位毫秒 |
| slotId | String | 用户传入的广告位id |
| userId | String | 用户 id，用户在媒体的唯一识别信息，来源于活动链接中的&userId=xxx，由媒体拼接提供 |
| adLayout | ViewGroup |广告显示区域 |
| jumpButton | View | 自定义跳过按钮 |
| splashAdListener | SplashAdListener | 回调参数 |

回调参数介绍

| 名称 | 回调时机 |
| :---------------------: | :---------------------: |
| onAdPresent | 广告请求数据成功 |
| onPageLoadFinish | 广告页面显示 |
| onAdDismissed | 广告关闭（请在此处实现跳转到主页逻辑） |
| onAdFailed | 广告请求失败 |
| onAdClick | 广告点击 |


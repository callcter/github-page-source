---
title: 'React Native 实现截图添加二维码分享功能'
---

## 截图捕捉功能已经发布到 NPM，欢迎使用
```
npm i react-native-screenshotcatch
```

对一个 JSer 来说，用原生来实现一个功能着实不容易。但是，随着APP开发的深入，在许多场景下RN现成的组件已经不能满足我们的需求，不想受制于人就要自己动手。图像绘制、文件系统、通知、模块封装等等，虽然难但是收获也多，希望自己能够更深入原生开发的领域。

### 效果展示
<!-- more -->
![效果展示GIF](https://cdn.dreamser.com/result-show-185.gif)

### 截屏监听功能

#### iOS 截屏监听实现

实现思路：添加iOS自带的UIApplicationUserDidTakeScreenshotNotification通知监听，捕捉到事件后绘制当前页面，保存返回文件地址

```objective-c
// ScreenShotShare.h
#import <React/RCTBridgeModule.h>
#import <React/RCTEventEmitter.h>

@interface ScreenShotShare : RCTEventEmitter <RCTBridgeModule>

@end
// ScreenShotShare.m
#import "ScreenShotShare.h"
#import <React/RCTConvert.h>
#import <React/RCTEventDispatcher.h>

#define PATH @"screen-shot-share"

@implementation ScreenShotShare
RCT_EXPORT_MODULE();

- (NSArray <NSString *> *)supportedEvents{
  return @[@"ScreenShotShare"];
}

RCT_EXPORT_METHOD(startListener){
  [self addScreenShotObserver];
}

- (void)addScreenShotObserver{
  [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(getScreenShot:) name:UIApplicationUserDidTakeScreenshotNotification object:nil];
}

- (void)removeScreenShotObserver{
  [[NSNotificationCenter defaultCenter] removeObserver:self name:UIApplicationUserDidTakeScreenshotNotification object:nil];
}

- (void)getScreenShot:(NSNotification *)notification{
  [self sendEventWithName:@"ScreenShotShare" body:[self screenImage]];
}

// 保存文件并返回文件路径
- (NSDictionary *)screenImage{
  @try{
    UIImage *image = [UIImage imageWithData: [self imageDataScreenShot]];

    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    NSFileManager *fileManager = [NSFileManager defaultManager];
    NSString *path =[[paths objectAtIndex:0]stringByAppendingPathComponent:PATH];
    if (![fileManager fileExistsAtPath:path]) {
      [fileManager createDirectoryAtPath:path withIntermediateDirectories:YES attributes:nil error:nil];
    }
    long time = (long)[[NSDate new] timeIntervalSince1970];
    NSString *filePath = [path stringByAppendingPathComponent: [NSString stringWithFormat:@"screen-capture-%ld.png", time]];

    @try{
      BOOL result = [UIImagePNGRepresentation(image) writeToFile:filePath atomically:YES]; // 保存成功会返回YES
      if (result == YES) {
        NSLog(@"agan_app 保存成功。filePath：%@", filePath);
        [[[UIApplication sharedApplication] keyWindow] endEditing:YES]; // 获取截屏后关闭键盘
        return @{@"code": @200, @"uri": filePath};
      }
    }@catch(NSException *ex) {
      NSLog(@"agan_app 保存图片失败：%@", ex.description);
      filePath = @"";
      return @{@"code": @500, @"errMsg": @"保存图片失败"};
    }
  }@catch(NSException *ex) {
    NSLog(@"agan_app 截屏失败：%@", ex.description);
    return @{@"code": @500, @"errMsg": @"截屏失败"};
  }
}

// 截屏
- (NSData *)imageDataScreenShot{
  CGSize imageSize = [UIScreen mainScreen].bounds.size;
  
  UIGraphicsBeginImageContextWithOptions(imageSize, NO, 0);
  CGContextRef context = UIGraphicsGetCurrentContext();
  for(UIWindow *window in [[UIApplication sharedApplication] windows]){
    CGContextSaveGState(context);
    CGContextTranslateCTM(context, window.center.x, window.center.y);
    CGContextConcatCTM(context, window.transform);
    CGContextTranslateCTM(context, -window.bounds.size.width*window.layer.anchorPoint.x, -window.bounds.size.height * window.layer.anchorPoint.y);
    if ([window respondsToSelector:@selector(drawViewHierarchyInRect:afterScreenUpdates:)]){
      NSLog(@"agan_app 使用drawViewHierarchyInRect:afterScreenUpdates:");
      [window drawViewHierarchyInRect:window.bounds afterScreenUpdates:YES];
    }else{
      NSLog(@"agan_app 使用renderInContext:");
      [window.layer renderInContext:context];
    }
    CGContextRestoreGState(context);
  }
  UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
  UIGraphicsEndImageContext();
  
  return UIImagePNGRepresentation(image);
}

@end
```

#### Android 截屏监听实现

实现思路：通过 ContentObserver 获取文件变化捕获截图事件，捕获后为了去掉状态栏以及虚拟导航栏使用 normalShot 方法自己绘制当前页面然后保存，返回文件路径。

```java
// ScreenShotSharePackage.java
public class ScreenShotShareModule extends ReactContextBaseJavaModule {
    private static final String TAG = "screenshotshare";
    private static final String NAVIGATION= "navigationBarBackground";
    private static final String[] KEYWORDS = {
            "screenshot", "screen_shot", "screen-shot", "screen shot",
            "screencapture", "screen_capture", "screen-capture", "screen capture",
            "screencap", "screen_cap", "screen-cap", "screen cap"
    };

    private static Activity ma;
    private ReactContext reactContext;
    /** 读取媒体数据库时需要读取的列 */
    private static final String[] MEDIA_PROJECTIONS =  {
            MediaStore.Images.ImageColumns.DATA,
            MediaStore.Images.ImageColumns.DATE_TAKEN,
    };
    /** 内部存储器内容观察者 */
    private ContentObserver mInternalObserver;
    /** 外部存储器内容观察者 */
    private ContentObserver mExternalObserver;
    private HandlerThread mHandlerThread;
    private Handler mHandler;

    public ScreenShotShareModule(ReactApplicationContext reContext){
        super(reContext);
        this.reactContext = reContext;
    }

    @Override
    public String getName() {
        return "ScreenShotShare";
    }

    public static void initScreenShotShareSDK(Activity activity){
        ma = activity;
    }

    @ReactMethod
    public void startListener(){
        mHandlerThread = new HandlerThread("Screenshot_Observer");
        mHandlerThread.start();
        mHandler = new Handler(mHandlerThread.getLooper());

        // 初始化
        mInternalObserver = new MediaContentObserver(MediaStore.Images.Media.INTERNAL_CONTENT_URI, mHandler);
        mExternalObserver = new MediaContentObserver(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, mHandler);

        // 添加监听
        this.reactContext.getContentResolver().registerContentObserver(
                MediaStore.Images.Media.INTERNAL_CONTENT_URI,
                false,
                mInternalObserver
        );
        this.reactContext.getContentResolver().registerContentObserver(
                MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
                false,
                mExternalObserver
        );
    }

    @ReactMethod
    public void stopListener(){
        this.reactContext.getContentResolver().unregisterContentObserver(mInternalObserver);
        this.reactContext.getContentResolver().unregisterContentObserver(mExternalObserver);
    }

    @ReactMethod
    public void hasNavigationBar(Promise promise){
        boolean navigationBarExisted =  isNavigationBarExist(ma);
        promise.resolve(navigationBarExisted);
    }

    private void handleMediaContentChange(Uri contentUri) {
        Cursor cursor = null;
        try {
            // 数据改变时查询数据库中最后加入的一条数据
            cursor = this.reactContext.getContentResolver().query(
                    contentUri,
                    MEDIA_PROJECTIONS,
                    null,
                    null,
                    MediaStore.Images.ImageColumns.DATE_ADDED + " desc limit 1"
            );

            if (cursor == null) {
                return;
            }
            if (!cursor.moveToFirst()) {
                return;
            }

            // 获取各列的索引
            int dataIndex = cursor.getColumnIndex(MediaStore.Images.ImageColumns.DATA);
            int dateTakenIndex = cursor.getColumnIndex(MediaStore.Images.ImageColumns.DATE_TAKEN);

            // 获取行数据
            String data = cursor.getString(dataIndex);
            long dateTaken = cursor.getLong(dateTakenIndex);

            // 处理获取到的第一行数据
            handleMediaRowData(data, dateTaken);
        } catch (Exception e) {
            WritableMap map = Arguments.createMap();
            map.putInt("code", 500);
            sendEvent(this.reactContext, "ScreenShotShare", map);
            e.printStackTrace();
        } finally {
            if (cursor != null && !cursor.isClosed()) {
                cursor.close();
            }
        }
    }

    /**
     * 处理监听到的资源
     */
    private void handleMediaRowData(String data, long dateTaken) {
        if (checkScreenShot(data, dateTaken)) {
            Log.d(TAG, data + " " + dateTaken);
            saveBitmap(normalShot(ma));
        } else {
            Log.d(TAG, "Not screenshot event");
            WritableMap map = Arguments.createMap();
            map.putInt("code", 500);
            sendEvent(this.reactContext, "ScreenShotShare", map);
        }
    }

    /**
     * 判断是否是截屏
     */
    private boolean checkScreenShot(String data, long dateTaken) {
        data = data.toLowerCase();
        // 判断图片路径是否含有指定的关键字之一, 如果有, 则认为当前截屏了
        for (String keyWork : KEYWORDS) {
            if (data.contains(keyWork)) {
                return true;
            }
        }
        return false;
    }

    private class MediaContentObserver extends ContentObserver {

        private Uri mContentUri;

        public MediaContentObserver(Uri contentUri, Handler handler) {
            super(handler);
            mContentUri = contentUri;
        }

        @Override
        public void onChange(boolean selfChange) {
            super.onChange(selfChange);
            Log.d(TAG, mContentUri.toString());
            handleMediaContentChange(mContentUri);
        }

    }

    public void sendEvent(ReactContext reactContext, String eventName, @Nullable WritableMap params) {
        reactContext.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class).emit(eventName, params);
    }

    // 判断全面屏虚拟导航栏是否存在
    public static  boolean isNavigationBarExist(Activity activity){
        ViewGroup vp = (ViewGroup) activity.getWindow().getDecorView();
        if (vp != null) {
            for (int i = 0; i < vp.getChildCount(); i++) {
                vp.getChildAt(i).getContext().getPackageName();
                if (vp.getChildAt(i).getId()!= NO_ID && NAVIGATION.equals(activity.getResources().getResourceEntryName(vp.getChildAt(i).getId()))) {
                    return true;
                }
            }
        }
        return false;
    }

    // 当前APP内容截图
    private static Bitmap normalShot(Activity activity) {
        View decorView = activity.getWindow().getDecorView();
        decorView.setDrawingCacheEnabled(true);
        decorView.buildDrawingCache();

        Rect outRect = new Rect();
        decorView.getWindowVisibleDisplayFrame(outRect);
        int statusBarHeight = outRect.top;//状态栏高度

        Bitmap bitmap = Bitmap.createBitmap(decorView.getDrawingCache(),
                0, statusBarHeight,
                decorView.getMeasuredWidth(), decorView.getMeasuredHeight() - statusBarHeight);

        decorView.setDrawingCacheEnabled(false);
        decorView.destroyDrawingCache();
        return bitmap;
    }

    // 获取当前APP图片存储路径
    private String getSystemFilePath() {
        String cachePath;
        if (Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())
                || !Environment.isExternalStorageRemovable()) {
            cachePath = reactContext.getExternalFilesDir(Environment.DIRECTORY_PICTURES).getAbsolutePath();
//            cachePath = context.getExternalCacheDir().getPath(); // 返回文件 uri，而非path
        } else {
            cachePath = reactContext.getFilesDir().getAbsolutePath();
//            cachePath = context.getCacheDir().getPath(); // 返回文件 uri，而非path
        }
        return cachePath;
    }

    // 保存截屏的bitmap为图片文件并返回路径
    private void saveBitmap(Bitmap bitmap){
        Long time = System.currentTimeMillis();
        String path = getSystemFilePath() + "/screen-capture-" + time + ".png";
        Log.d(TAG, path);
        File filePic;
        WritableMap map = Arguments.createMap();
        try{
            filePic = new File(path);
            if (!filePic.exists()) {
                filePic.getParentFile().mkdirs();
                filePic.createNewFile();
            }
            FileOutputStream fos = new FileOutputStream(filePic);
            bitmap.compress(Bitmap.CompressFormat.PNG, 100, fos);
            fos.flush();
            fos.close();
            map.putInt("code", 200);
            map.putString("uri", filePic.getAbsolutePath());
            sendEvent(this.reactContext, "ScreenShotShare", map);
            // 强制关闭软键盘
            ((InputMethodManager) ma.getSystemService(reactContext.INPUT_METHOD_SERVICE)).hideSoftInputFromWindow(ma.getCurrentFocus().getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);
        }catch(IOException e){
            e.printStackTrace();
            map.putInt("code", 500);
            sendEvent(this.reactContext, "ScreenShotShare", map);
        }
    }
}
```

### 分享功能

我集成了Umeng的share SDK，但是没有现成的纯图片分享接口，需要自己封装

#### iOS
```objective-c
// UMShareModule.m 中自定义 shareImage
RCT_EXPORT_METHOD(shareImage:(NSString *)url icon:(NSString *)icon platform:(NSInteger)platform completion:(RCTResponseSenderBlock)completion){
  
  UMSocialPlatformType plf = [self platformType:platform];
  if (plf == UMSocialPlatformType_UnKnown) {
    if (completion) {
      completion(@[@(UMSocialPlatformType_UnKnown), @"invalid platform"]);
      return;
    }
  }
  UIImage *image = [UIImage imageWithContentsOfFile:url];
  
  //创建分享消息对象
  UMSocialMessageObject *messageObject = [UMSocialMessageObject messageObject];
  //创建图片内容对象
  UMShareImageObject *shareObject = [[UMShareImageObject alloc] init];
  //如果有缩略图，则设置缩略图
  shareObject.thumbImage = [UIImage imageNamed:icon];
  [shareObject setShareImage:image];
  //分享消息对象设置分享内容对象
  messageObject.shareObject = shareObject;
  //调用分享接口
  [[UMSocialManager defaultManager] shareToPlatform:plf messageObject:messageObject currentViewController:nil completion:^(id data, NSError *error) {
    if (error) {
      NSLog(@"appppp %@", error);
      if (completion) {
        completion(@[@-1, error]);
      }
    }else{
      if (completion) {
        completion(@[@200, data]);
      }
    }
  }];
}
```

#### Android
```java
// ShareModule.java 中自定义 shareImage
@ReactMethod
public void shareImage(final String url, final String icon, final int sharemedia, final Callback successCallback){
    runOnMainThread(new Runnable() {
        @Override
        public void run() {
            Uri uri = Uri.parse(url);
            File imageFile = new File(getPath(contect, uri));
            UMImage image = new UMImage(ma, imageFile);
            new ShareAction(ma)
                .withMedia(image)
                .setPlatform(getShareMedia(sharemedia))
                .setCallback(getUMShareListener(successCallback))
                .share();
        }
    });
}
// uri 转 path
private String getPath(Context context, Uri uri) {
    String[] projection = {MediaStore.Video.Media.DATA};
    Cursor cursor = context.getContentResolver().query(uri, projection, null, null, null);
    int column_index = cursor.getColumnIndexOrThrow(MediaStore.Audio.Media.DATA);
    cursor.moveToFirst();
    return cursor.getString(column_index);
}
```

### 封装调用

#### 封装
```javascript
// ScreenShotShareUtil.js
import { NativeModules, NativeEventEmitter, DeviceEventEmitter } from 'react-native'
let screenCaptureEmitter = undefined
export default class ScreenShotShareUtil {
  static startListener(callback){
    const ScreenShotShare = NativeModules.ScreenShotShare
    screenCaptureEmitter && screenCaptureEmitter.removeAllListeners('ScreenShotShare')
    screenCaptureEmitter = Adapter.isIOS ? new NativeEventEmitter(ScreenShotShare) : DeviceEventEmitter
    screenCaptureEmitter.addListener('ScreenShotShare', (data) => {
      if(callback){
        callback(data)
      }
    })
    ScreenShotShare.startListener()
    return screenCaptureEmitter
  }
  static stopListener () {
    screenCaptureEmitter && screenCaptureEmitter.removeAllListeners('ScreenShotShare')
    const screenCaptureEmitter = NativeModules.ScreenShotShare
    return screenCaptureEmitter.stopListener()
  }
  static hasNavigationBar(){
    if(!Adapter.isIOS){
      screenCaptureEmitter && screenCaptureEmitter.removeAllListeners('ScreenShotShare')
      const screenCaptureEmitter = NativeModules.ScreenShotShare
      return screenCaptureEmitter.hasNavigationBar()
    }else{
      return false
    }
  }
}

// ShareUtil.js
// 分享图片
export const shareImage = (url, platform) => {
  platform = platform || 'weixin'
  let pl_int = 2
  switch(platform){
    case 'weixin':
      pl_int = 2
      break
    case 'timeline':
      pl_int = 3
      break
    case 'qq':
      pl_int = 0
      break
    case 'qzone':
      pl_int = 4
      break
    case 'weibo':
      pl_int = 1
      break
    default:
      pl_int = 2
      break
  }
  return new Promise((resolve, reject) => {
    UMShare.shareImage(url, IMAGE_URL, pl_int, (code, message) => {
      if(__DEV__){
        console.log(`分享图片到${platform}`, code, message)
      }
    })
  })
}
```

#### 调用
```javascript
// index.js
import ScreenShotShareModal from './ScreenShotShareModal'
import { ToastComponent } from 'react-native-pickers'
// ...
componentWillMount(){
  ScreenShotShareUtil.startListener(res => {
    if(res && res.code === 200){
      this.screenShotShareModal.show(res.uri)
    }else{
      ToastComponent.show('获取截图失败');
    }
  })
}
componentWillUnmount(){
  ScreenShotShareUtil.stopListener()
}
render(){
  return (
    <View style={{flex: 1, backgroundColor: '#fff', justifiContent: 'center', alignItems: 'center'}}>
      <Text>...</Text>
      <ScreenShotShareModal ref={ref => this.screenShotShareModal = ref} />
    </View>
  )
}
// ...
// ScreenShotShareModal.js
import { BaseDialog } from 'react-native-pickers'
import { shareImage } from './ShareUtil'
import QRCode from 'react-native-qrcode-svg'
import ViewShot from 'react-native-view-shot'

export default class ScreenShotShareModal extends BaseDialog {
  constructor(props) {
    super(props)
    this.state = {
      image: null,
      logoUri: 'base64://xxxxx',
      text: 'xxx'
    }
    this.viewShot = React.createRef()
  }
  show(uri){
    this.setState({
      image: uri
    }, () => {
      super.show()
    })
  }
  renderContent(){
    return (
      <View>
        <View>
          <ViewShot ref={this.viewShot}>
            <View>
              <Image source={{uri: (isIOS ? this.state.image : `file://${this.state.image}`)}} />
              <QRCode
                value={this.state.text}
                size={60}
                logo={{uri: this.state.logoUri}}
                logoSize={15}
                logoBackgroundColor='white'
                logoBorderRadius={3} />
              <Text>扫描二维码下载《XXX》</Text>
            </View>
          </ViewShot>
        </View>
        <View>
          <Text>分享至</Text>
          <View style={styles.itemGroup}>
            <View style={styles.item}>
              <TouchableOpacity activeOpacity={0.9} onPress={ () => this._shareImage('weixin') }>
                <Text>微信</Text>
              </TouchableOpacity>
            </View>
            <View style={styles.item}>
              <TouchableOpacity activeOpacity={0.9} onPress={() => this._shareImage('timeline') }>
                <Text>朋友圈</Text>
              </TouchableOpacity>
            </View>
            <View style={styles.item}>
              <TouchableOpacity activeOpacity={0.9} onPress={() => this._shareImage('weibo') }>
                <Text>微博</Text>
              </TouchableOpacity>
            </View>
            <View style={styles.item}>
              <TouchableOpacity activeOpacity={0.9} onPress={() => this._shareImage('qq') }>
                <Text>QQ</Text>
              </TouchableOpacity>
            </View>
            <View style={styles.item}>
              <TouchableOpacity activeOpacity={0.9} onPress={() => this._shareImage('qzone') }>
                <Text>空间</Text>
              </TouchableOpacity>
            </View>
          </View>
        </View>
      </View>
    )
  }
  _shareImage(plf){
    this.viewShot.current.capture().then(imageUri=>{
      if(!isIOS){
        CameraRoll.saveToCameraRoll(imageUri).then(res => {
          if(res){
            shareImage(res, plf)
          }
        }).catch(err => {
          if(__DEV__){ console.log(err) }
        })
      }else{
        shareImage(imageUri, plf)
      }
    })
  }
}

const styles = StyleSheet.create({
  itemGroup: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 15
  },
  item: {
    justifyContent: 'center',
    alignItems: 'center',
    flexDirection: 'column'
  }
})
```

### 参考文章

- [Umeng Share 文档](https://developer.umeng.com/docs/66632/detail/66639)
- [react-native-lewin-screen-capture](https://github.com/LewinJun/react-native-lewin-screen-capture)

#### iOS

- [iOS捕捉截屏事件并展示截图](https://blog.csdn.net/SL_ideas/article/details/73332058)
- [【ReactNative】与iOS组件间的相互调用](http://blog.xigulu.com/2016/09/10/ReactNative-iOS-communication/)
- [IOS原生模块向ReactNative发送事件消息](https://www.jianshu.com/p/c91d343009b3)
- [IOS 本地图片加载](https://blog.csdn.net/ws_752958369/article/details/80477150)

#### Android

- [Android 截屏监听（截图分享功能实现）](https://www.jianshu.com/p/d7aba5a03b0f)
- [Android 截屏事件监听](https://juejin.im/entry/58647ee9128fe1006d0f4454)
- [React Native之Android原生通过DeviceEventEmitter发送消息给js](https://blog.csdn.net/u011068702/article/details/82808195)
- [react native 中的ReadableMap和WritableMap的使用](https://www.cnblogs.com/summary-2017/p/7709400.html)
- [Android -- 超全的 File，Bitmap，Drawable，Uri, FilePath ,byte[]之间的转换方法](https://blog.csdn.net/wyg1230/article/details/79153641)
- [Android之uri、file、path相互转化](https://blog.csdn.net/hust_twj/article/details/76665294)
- [Android开发managedQuery方法过时如何解决](https://blog.csdn.net/qq_36317441/article/details/79278888)
- [将bitmap对象保存到本地，返回保存的图片路径](https://blog.csdn.net/qq_31028313/article/details/55251370)
- [Android普通截屏(不包括状态栏内容无状态栏占位仅包含应用程序)](https://blog.csdn.net/Kikitious_Du/article/details/78403892)
- [Android全面屏虚拟导航栏适配](https://juejin.im/post/5bb5c4e75188255c72285b54)
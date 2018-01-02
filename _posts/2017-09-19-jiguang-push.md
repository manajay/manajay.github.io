---
layout: post
title: 极光推送的使用
tag: [iOS, 推送]
date: 2017-05-19 00:42:46 +09:00
---

#### 苹果的APNS

![image.png](http://upload-images.jianshu.io/upload_images/1435355-7bee00450e451e8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


1. 用户的应用注册了`APNS` 消息推送功能
2. 用户`iOS`设备通过SSL长连接到APNS苹果服务器,收到设备应用的注册信息后,下发给设备一个`DeviceToken` 给 应用
3. 应用收到这个`DeviceToken` 然后推送给 自己应用的服务器 (应用到推送服务器的流程完毕)
4. 推送服务器 发送消息到一个用户的时候, 会首先查找到 `DeviceToken`,然后将消息和`DeviceToken` 发送给 苹果的 APNS 服务器
5. 苹果根据 `DeviceToken` 找到唯一的那台设备, 然后将消息 传递过去
6. 设备收到了消息后, 会根据`DeviceToken` 找到应用  (推送服务器到设备应用的流程完毕)

#### 极光推送的流程

![极光推送](http://upload-images.jianshu.io/upload_images/1435355-fb225c66ca651fe5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里和上面唯一不同的就是, 应用的服务器改为了 极光的服务器

1. 设备获取到`DeviceToken` 后 需要将这个 _信息_ 上传到 极光的 服务器上面
2. 然后 极光的服务器会 生成一个`registrationID`给应用
3. 这个时候, 我们可以给应用注册一个别名, 就相当于一个 键值对, 极光服务器根据这个别名查找到`registrationID`, 然后又根据`registrationID`找到 `DeviceToken`, 这样 _发送消息_的时候 , 就可以将 `DeviceToken` 和 信息一起发送给 苹果的`APNS服务器`了. 


#### 实际编程的注意点

![image.png](http://upload-images.jianshu.io/upload_images/1435355-79c43e85c70a71f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1、极光后台的生成环境和发布环境证书必须有效
![image.png](http://upload-images.jianshu.io/upload_images/1435355-305da2ee3a0d8d45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2、 应用需要先注册和激活极光服务

![image.png](http://upload-images.jianshu.io/upload_images/1435355-1b84ac49938762ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 应用获取到APNS下发的`DeviceToken`后,需要上传到极光服务器
4. 应用的极光登录成功方法回调,说明服务器已经收到的应用的设备信息,并且返回了`registrationID`
5. 只有在`registrationID`返回之后,才可以设置别名


#### 补充信息

> kJPFNetworkDidLoginNotification

``` ----- login result -----
uid:939xxx3544 
registrationID:141fe1daxxx927a72d
```

#### 代码中方法的调用顺序 , 一定要严格按照如下的顺序
1、添加初始化APNs代码添加初始化APNs代码

```
-(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  //Required
  //notice: 3.0.0及以后版本注册可以这样写，也可以继续用之前的注册方式
  JPUSHRegisterEntity * entity = [[JPUSHRegisterEntity alloc] init];
  entity.types = JPAuthorizationOptionAlert|JPAuthorizationOptionBadge|JPAuthorizationOptionSound;
  if ([[UIDevice currentDevice].systemVersion floatValue] >= 8.0) {
    // 可以添加自定义categories
    // NSSet<UNNotificationCategory *> *categories for iOS10 or later
    // NSSet<UIUserNotificationCategory *> *categories for iOS8 and iOS9
  }
  [JPUSHService registerForRemoteNotificationConfig:entity delegate:self];
```

2、 APNS后 启动极光服务

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions { 
// Required
  // init Push
  // notice: 2.1.5版本的SDK新增的注册方法，改成可上报IDFA，如果没有使用IDFA直接传nil
  // 如需继续使用pushConfig.plist文件声明appKey等配置内容，请依旧使用[JPUSHService setupWithOption:launchOptions]方式初始化。
  [JPUSHService setupWithOption:launchOptions appKey:appKey
                        channel:channel
               apsForProduction:isProduction
          advertisingIdentifier:advertisingId]; 
}
                                    
```

2、 注册APNs成功并上报DeviceToken 到极光的后台

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    [JPUSHService registerDeviceToken:deviceToken];
}
```
2.5 、 实现注册APNs失败接口（可选）

```
- (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error {
  //Optional
  NSLog(@"did Fail To Register For Remote Notifications With Error: %@", error);
}
```

3 、 添加处理APNs通知回调方法
请在AppDelegate.m实现该回调方法并添加回调方法中的代码

```
#pragma mark- JPUSHRegisterDelegate

// iOS 10 Support
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(NSInteger))completionHandler {
  // Required
  NSDictionary * userInfo = notification.request.content.userInfo;
  if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
  }
  completionHandler(UNNotificationPresentationOptionAlert); // 需要执行这个方法，选择是否提醒用户，有Badge、Sound、Alert三种类型可以选择设置
}

// iOS 10 Support
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
  // Required
  NSDictionary * userInfo = response.notification.request.content.userInfo;
  if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
  }
  completionHandler();  // 系统要求执行这个方法
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {

  // Required, iOS 7 Support
  [JPUSHService handleRemoteNotification:userInfo];
  completionHandler(UIBackgroundFetchResultNewData);
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {

  // Required,For systems with less than or equal to iOS6
  [JPUSHService handleRemoteNotification:userInfo];
}
```

4、如果项目中需要接收自定义信息的功能, 则需要添加监听自定义消息的方法 (可选)

```
只有在前端运行的时候才能收到自定义消息的推送。
从jpush服务器获取用户推送的自定义消息内容和标题以及附加字段等

获取iOS的推送内容需要在delegate类中注册通知并实现回调方法。
在方法didFinishLaunchingWithOptions 加入下面的代码：

```
```

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *) launchOptions {
     NSNotificationCenter *defaultCenter = [NSNotificationCenter defaultCenter];
    [defaultCenter addObserver:self selector:@selector(networkDidReceiveMessage:) name:kJPFNetworkDidReceiveMessageNotification object:nil];
}
并且实现通知的回调方法 networkDidReceiveMessage
- (void)networkDidReceiveMessage:(NSNotification *)notification {
        NSDictionary * userInfo = [notification userInfo];
        NSString *content = [userInfo valueForKey:@"content"];
        NSDictionary *extras = [userInfo valueForKey:@"extras"]; 
        NSString *customizeField1 = [extras valueForKey:@"customizeField1"]; //服务端传递的Extras附加字段，key是自己定义的
}


```

5、 监听极光登录成功的通知，查看是否入库极光成功， 在这个方法中可以 设置别名； 不过应用的逻辑 是 用户登录成功后才可以推送，所以在这里增加了判断用户是否登录的方法。

```
 (void)networkDidLogin:(NSNotification *)notification {
    DDLog(@"jpush已登录,通知详情为%@",notification.userInfo);
    //向极光注册别名
    User *user = [UserManager sharedInstace].usr;
    if( user.isLogin && user.device.tmpPushAliasName ){
        user.device.pushAliasName = user.device.tmpPushAliasName;
    }
}
```

6、真机调试该项目，如果控制台输出以下日志则代表您已经集成成功。

```
2017-05-17 11:24:05.968565+0800 DDRide[22015:4672102]  | JIGUANG | I - [JIGUANGRegistration] 
----- register result -----
uid: 942xxxx1223
registrationID:13165fxxxx97541104 
2017-05-17 11:24:05.970270+0800 DDRide[22015:4671830] [函数名:-[JpushManager networkDidRegister:]]----[行号:74] 
jpush已注册
2017-05-17 11:24:06.108945+0800 DDRide[22015:4672119]  | JIGUANG | I - [JIGUANGLogin] 
----- login result -----
uid:942xx61223 
registrationID:13165ffxxxx7541104
2017-05-17 11:24:06.112224+0800 DDRide[22015:4671830] [函数名:-[JpushManager networkDidLogin:]]----[行号:78] 
jpush已登录,通知详情-(null)
2017-05-17 11:24:06.266877+0800 DDRide[22015:4672102]  | JIGUANG | I - [JIGUANGDeviceTokenReport] try to upload device token:152ff6a08ec7927xxxxxxxxxxxe603cd642f5f4cab8cae10ab8e
2017-05-17 11:24:06.437331+0800 DDRide[22015:4672102]  | JIGUANG | I - [JIGUANGBadgeNumberReport] set badge:0 succeed
2017-05-17 11:24:06.535148+0800 DDRide[22015:4672119]  | JIGUANG | I - [JIGUANGDeviceTokenReport] upload device token success
```

7、 新增功能 接收本地通知 , 该代理方法已经适配iOS10

```
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^) (NSInteger))completionHandler; 
   // if (![notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) { 
   // 本地通知为notification 
   // }

- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler: (void (^)())completionHandler; 
  // if (![response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) { 
  // 本地通知为response.notification 
  // }

```

#### 测试结果
1. 模拟器不能测试极光推送,因为只有真机具备`UDID`, 才能够生成`DeviceToken`
2. 生产环境证书失效,测试失败
3. 登录成功后,查看 `registrationID`为`141fe1dxxxx7a72d` ,入库极光成功,马上测试别名推送(前台),成功
4. 后面等待一段时间后,进入后台测试根据`registrationID` 推送成功, 推送别名`149498xxxx0546`  成功.
5. 更换设备后, 没有登录的情况下, `registrationID`变化为`13165ffxxxx7541104`
6. 测试 没有登录的时候 后台`registrationID`推送成功 , 别名`14949xxxx12183516`
7.  这里有个需要注意的地方是,如果服务器是根据别名推送,且每次登录会产生新的别名,那么情况就比较麻烦.

对于单点登录的问题, 
环境: 设备A已经登录, 设备B同账户登录, 更新了别名, 服务器根据同用户的旧别名发送一个通知, 设备A接收到通知后,退出登录. 
可以这样处理.服务器保留上一次的别名和最新的别名. 
如果自己的服务器保存的是最新的别名, 根据最新的别名推送,则最新登录的设备是能够收到消息,而使用旧的别名,新设备不会接受到, 只有上一个登录的设备会接收. 
因为一个别名对于一个注册ID, 一个注册ID则对于一个 `DeviceToken`,而一个`DeviceToken`则能够寻找到唯一设备的唯一应用, 这种一一对应的关系,只要某一环出了问题,推送就不可以送达,更不要说设备如果网络环境差的话,同样APNS不会推送到设备应用上. 

#### 常见问题
1、 iOS 9系统，应用卸载重装，APNs返回的devicetoken会发生变化，开发者需要获取设备最新的Registration id。请在kJPFNetworkDidLoginNotification的实现方法里面调用"RegistrationID"这个接口来获取 RegistrationID。
2、 为什么iOS收不到推送消息？

* iOS接受消息必要条件是：应用程序的证书要和你上传到jpush portal上的证书对应，如果你的程序是直接在xcode上运行的，你的应用部署环境必须是开发状态才能收到APNS消息。
* 确认 appKey 在 SDK 客户端与 Portal 上设置是一致，其他环节也按照文档正确地操作。
* 检查 Portal 上上传的证书，是 APNs (Push) 证书。
* 再次检查证书选择是否正确，参考iOS 证书设置指南89。
* 推送时选择的环境与测试设备的打包环境必须一致。测试说明109
* 严肃说明：api推送的时候通过参数 apns_production 来指定推送环境，false为开发环境，true为生产环境。V3 api不带此参数则默认为生产环境，V3 api封装的 sdk 默认为开发环境。如果api有传apns_production则以此值为准。
* 另外，请确认设备网络是否正常，如果设备离线，期间推送多条，apns服务器只会离线保留一条

3、 应用产生的设备信息是否变化
自iOS9开始，卸载重装、长时间关闭推送后又打开等情况 会产生新的token，因此有对应的新的registrationID。（如果用了idfa，在用户没有选择限制广告跟踪，并且未点击『还原广告标识』时，这个值是不会变的，对应的token也不会改变。）

```
 温馨提示：
 * Registration id 需要在执行到kJPFNetworkDidLoginNotification的方法里获取

 * extern NSString * const kJPFNetworkDidReceiveMessageNotification; // 收到自定义消息(非APNS)
 其中，kJPFNetworkDidReceiveMessageNotification传递的数据可以通过NSNotification中的userInfo方法获取，包括标题、内容、extras信息等
 * 主要是在项目的登录成功或者自动登录后，使用用户的唯一标示进行绑定，或者根据需求添加一些前缀
 
 * 用户进行退出登录的方法里添加去除绑定即可，值得注意的是用到即时通讯的话，被挤下线也要去除绑定，已被坑，贴代码：
 //没有值就代表去除
 [JPUSHService setTags:[NSSet set]callbackSelector:nil object:self];
 [JPUSHService setAlias:@"" callbackSelector:nil object:self];
 [JPUSHService setTags:[NSSet set] alias:@"" callbackSelector:nil target:self];
```

#### 参考链接

[iOS远程推送通知 APNs远程推送,极光推送](http://www.jianshu.com/p/c623c2b5966a)
[做推送，怎么能不了解推送的 4 种消息形式呢](http://blog.jiguang.cn/4-push-ios/)
[ios 极光推送的集成及注意事项](http://www.jianshu.com/p/1d37efd7d65c)
[常见问题 - JPush 合集（持续更新](https://community.jiguang.cn/t/jpush/5145/4)
[iOS App卸载重装后生成了新的registrationID，导致很多无效的ID](https://community.jiguang.cn/t/ios-app-registrationid-id/11497)
[04.iOS远程推送通知 APNs远程推送,极光推送](http://www.jianshu.com/p/c623c2b5966a)
[「使用心得」使用 JPush iOS SDK 注意事项](https://community.jiguang.cn/t/jpush-ios-sdk/3443)




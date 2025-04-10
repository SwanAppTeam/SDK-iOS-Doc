# 集成 SDK

下载SDK并手动配置项目以使用它。

## SDK

### SWMusicKit SDK

SWMusicKit SDK 是 Swan Home Audio方案的app端SDK；通过它，您可以快速的将我们的解决方案实现到您的产品里。

[下载 SWMusicKit SDK](https://github.com/SwanAppTeam/SWMusicKitDemoiOS/tree/main/SWMusicKitDemo/Frameworks)

以下是 SWMusicKit SDK 的主要功能:

- 设备发现
  
  ```
  [[SWDeviceManager sharedInstance] search:@""];
  ```
  
- 设备上线和下线代理
  
  ```
  [[SWDeviceManager sharedInstance] addObserver:self];
  // delegate
  onSWDeviceOnline
  onSWDeviceOffline
  onSWDeviceUpdate
  ```
  
- 设备的播控设置 ： 通过 SWDevicePlayer 实现设备的播放处理
  
- 设备信息展示： 通过 SWDeviceInfo 和 SWDeviceStatus 展示设备的信息
  
- 设备媒体信息展示 ： 通过 SWMediaInfo 展示设备媒体信息
  

## SDK Demo

- [SWMusicKitiOSDemo](https://github.com/SwanAppTeam/SWMusicKitDemoiOS)

# 设备配网

# Overview

设备配网是用户配置设备的第一步。设备配网SDK提供了把设备配置到路由器的能力，目前提供以下配网方式：

- SoftAP配网模式

> iOS13及以上需要提供定位权限，拿到手机所连的Wi-Fi的名称。

### SoftAP 配网模式

SWWiFiSetupManager 提供了Wi-Fi配网的能力。

- `SWApItem`
  
  getApList 返回的Wi-Fi对象，包含以下信息
  

| 名称  | 类型  | 描述  |
| --- | --- | --- |
| SSID | NSString | SSID 名称 |
| RSSI | NSString | 信号强度 |
| BSSID | NSString | 信号地址 |
| Channel | NSString | 信道  |
| Auth | NSString | 授权  |
| Encry | NSString | 加密方式 |

- `SWWiFiSetupFailed` errorCode
  
  errorCode 所对应的含义
  

| 名称  | 类型  | 描述  |
| --- | --- | --- |
| 1001 | int | 手机当前连接的Wi-Fi和用户选择的Wi-Fi不一致 |
| 1002 | int | 其他错误 |

#### 检测手机是否连接到设备热点

- 接口说明
  
  Wi-Fi配网模式需要手机的Wi-Fi连接到设备的热点网络上，所以需要检测手机的Wi-Fi是否连接到设备热点，可以设定检测的时间
  
  ```
    - (void)isLinkplayHotspotWithCheckTime:(int)time block:(void(^)(BOOL isDirect))block;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| time | int | 是否连接到设备热点的检测时间 |

- 返回值
  
  BOOL
  
- 示例代码
  
  ```
    [[SWWiFiSetupManager sharedInstance] isLinkplayHotspotWithCheckTime:10 block:^(BOOL isDirect) {
        if (!isDirect) {
            NSLog(@"The WiFi connected to the phone is not a hotspot from the device");
        }else {
            NSLog(@"The WiFi connected to the phone is a hotspot from the device");
        }
    }];
  ```
  

#### 获取 Wi-Fi 列表

- 接口说明
  
  手机Wi-Fi连接到设备热点后，可以获取到周边的Wi-Fi列表。
  
  ```
    - (void)getApList:(SWApListBlock)block;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  
- 示例代码
  
  ```
    __weak __typeof(self)weakSelf = self;
    [[SWWiFiSetupManager sharedInstance] getApList:^(NSMutableArray * _Nonnull SWApList) {
        weakSelf.wlanDetailsArray = SWApList;
    }];
  ```
  

#### 开始配网接口

- 接口说明
  
  ```
    - (void)connectToWiFi:(SWApItem *)apItem pass:(NSString *)pass success:(SWWiFiSetupSuccess)success failed:(SWWiFiSetupFailed)failed;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| item | SWApItem | 选择的目标Wi-Fi对象 |
| pass | NSString | Wi-Fi的密码 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    SWApItem *apItem = [_wlanDetailsArray objectAtIndex:indexPath.row];
    [[SWWiFiSetupManager sharedInstance] connectToWiFi:apItem pass:passwordText.text success:^(SWDevice * _Nonnull device) {
        NSLog(@"SWWiFiConnect success");
    } failed:^(int errorCode) {
        NSLog(@"SWWiFiConnect failed");
    }];
  ```
  

####

# 设备发现

# Overview

设备管理包含了，设备的上线、下线、删除等操作，同时也可以通过Id来获取到对应的设备, SWDeviceManager 提供了管理设备的API

# API Reference

### 设备搜索

#### 搜索设备

- 接口说明
  
  搜索周边的设备
  
  ```
    - (void)start:(NSString *)searchKey;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| key | NSString | 默认传空 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[SWDeviceManager sharedInstance] search:@""];
  ```
  

#### 停止搜索设备

- 接口说明
  
  ```
    - (void)stop;
  ```
  
- 参数
  
  无
  

### 添加代理

#### 添加观察者

- 接口说明
  
  ```
    - (void)addObserver:(id<SWDeviceManagerObserver>)observer;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| observer | SWDeviceManagerObserver | 要接收设备上下线通知的对象 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[SWDeviceManager sharedInstance] addObserver:self];
  ```
  

#### 移除观察者

- 接口说明
  
  ```
    - (void)removeObserver:(id<SWDeviceManagerObserver>)observer;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| observer | SWDeviceManagerObserver | 要接收设备上下线通知的对象 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[SWDeviceManager sharedInstance] removeObserver:self];
  ```
  

### 获取设备

#### 根据设备Id获取设备

- 接口说明
  
  ```
    - (SWDevice *)deviceForID:(NSString *)UUID;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| UUID | NSString | 设备UUID |

- 返回值

| 类型  | 接口说明 |
| --- | --- |
| SWDevice | 设备对象 |

- 示例代码
  
  ```
    SWDevice *device = [[SWDeviceManager sharedInstance] deviceForID:UUID];
  ```
  

#### 获取主设备列表

- 接口说明
  
  ```
    - (NSArray<SWDevice *> *)getMasterDevices;
  ```
  
- 参数
  
  无
  
- 返回值
  

| 类型  | 接口说明 |
| --- | --- |
| NSArray | 主设备列表 |

- 示例代码
  
  ```
    self.deviceListArray = [[SWDeviceManager sharedInstance] getMasterDevices];
  ```
  

#### 根据设备IP获取设备

- 接口说明
  
  ```
    - (SWDevice *)deviceForIP:(NSString *)IP;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| IP  | NSString | 设备IP |

- 返回值

| 类型  | 接口说明 |
| --- | --- |
| SWDevice | 设备对象 |

- 示例代码
  
  ```
    SWDevice *device = [[SWDeviceManager sharedInstance] deviceForIP:IP];
  ```
  

#### 根据设备MAC获取设备

- 接口说明
  
  ```
    - (SWDevice *)deviceForMAC:(NSString *)MAC;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| MAC | NSString | 设备MAC |

- 返回值

| 类型  | 接口说明 |
| --- | --- |
| SWDevice | 设备对象 |

- 示例代码
  
  ```
    SWDevice *device = [[SWDeviceManager sharedInstance] deviceForIP:MAC];
  ```
  

#### 根据主设备Id获取子设备列表

- 接口说明
  
  ```
    -(NSArray<SWDevice *> *)slaveDeviceArray:(NSString *)UUID;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| UUID | NSString | 设备UUID |

- 返回值

| 类型  | 接口说明 |
| --- | --- |
| NSArray | 子设备列表 |

- 示例代码
  
  ```
    NSArray *slaveListArray = [[SWDeviceManager sharedInstance] slaveDeviceArray:UUID];
  ```
  

####

### 设备上下线代理

- SWDeviceManagerObserver

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| onSWDeviceOnline | SWDevice | 设备上线 |
| onSWDeviceOffline | SWDevice | 设备下线 |
| onSWDeviceUpdate | SWDevice | 设备更新 |

#### - (SWDevicePlayer *)getPlayer;

- 接口说明
  
  Get Device's player.
  
- 参数
  
  无
  
- 返回值
  

| 类型  | 接口说明 |
| --- | --- |
| SWDevicePlayer | Player object |

####

## SWMediaInfo

### Function

```
无
```

### Property

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| title | NSString | Title |
| artist | NSString | Artist |
| album | NSString | Album |
| ... | ... | ... |

## SWDeviceInfo

### Property

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| playStatus | NSString | Current play status |
| playMode | int | Play mode |
| mediaType | NSString | Media Type |
| trackSource | NSString | Track source |
| ... | ... | ... |

## SWDeviceStatus

### Function

```
无
```

### Property

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| UUID | NSString | UUID |
| IP  | int | IP  |
| WiFiStrength | float | Wi-Fi signal strength |
| MAC | NSString | MAC |
| SSID | NSString | SSID |
| friendlyName | NSString | Device's friendly name |
| version | NSString | Firmware version |
| language | int | Prompt tone language |
| release | NSString | Compiled date |

## Other Definitions

### SWPlayMode

| 类型  | 接口说明 |
| --- | --- |
| SW_LISTREPEAT | Loop playback |
| SW_SINGLEREPEAT | Single cycle |
| SW_SHUFFLE | Shuffle |
| SW_SHUFFLEREPEAT | Shuffle repeat |
| SW_DEFAULT | Default |

### SWDeviceChannel

| 类型  | 接口说明 |
| --- | --- |
| SWChannel_stereo | Stereo |
| SWChannel_left | Left channel |
| SWChannel_right | Right channel |

### SWPlayStatus

| 类型  | 接口说明 |
| --- | --- |
| SW_PLAYER_STATE_PLAYING | Playing |
| SW_PLAYER_STATE_STOPPED | Stop |
| SW_PLAYER_STATE_PAUSED_PLAYBACK | Pause |
| SW_PLAYER_STATE_TRANSITIONING | Transitioning |
| SW_PLAYER_STATE_NO_MEDIA_PRESENT | No media present |

###

# 设备设置

# Overview

设备对象包含了播控、设备设置等有关设备的功能

### 播放控制

#### 获取设备播控对象

- 接口说明
  
  ```
    - (SWDevicePlayer *)getPlayer;
  ```
  
- 参数
  
  无
  
- 返回值
  

| 类型  | 接口说明 |
| --- | --- |
| SWDevicePlayer | 设备播控对象 |

#### 播放

- 接口说明
  
  ```
    - (void)play:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  

#### 暂停

- 接口说明
  
  ```
    - (void)pause:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  

#### 停止播放

- 接口说明
  
  ```
    - (void)stop:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  

#### 下一首

- 接口说明
  
  ```
    - (void)next:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  

#### 上一首

- 接口说明
  
  ```
    - (void)previous:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  

#### 播放音乐

播放音乐指的是从音源SDK中获取要播放的信息 musicDictionary, 然后传递给设备，从而让设备播放音源信息，从音源中获得 SWPlayMusicList 对象后，可以通过 SWMDPKitManager SDK 转换成接口需要的播放信息 musicDictionary

- 接口说明
  
  ```
    - (void)playAudioWithMusicArray:(NSArray<SWPlayItem *> *)musicArray completionHandler:(SWPlayerBlock  _Nullable __strong)completionHandler;
  ```
  
- 参数
  
  **SWPlayItem**
  
  | 名称  | 类型  | 接口说明 |
  | --- | --- | --- |
  | trackId | NSString | Title of the track |
  | trackName | NSString | Track identifier |
  | trackArtist | NSString | Author |
  | trackUrl | NSString | Link to play track |
  | trackImage | NSString | Track cover |
  
- 返回值
  
  无
  
- 示例代码
  
  [[self.boxInfo getPlayer] playAudioWithMusicArray:arr completionHandler:^(BOOL isSuccess, NSString * _Nullable result) {
  
  ```
        if (isSuccess) {
  
            NSLog(@"OK");
  
        }
  ```
  
  }];
  

#### 播放当前正在播放歌单中的歌曲

- 接口说明
  
  ```
    - (void)playCurrentPlayListWithIndex:(int)index completionHandler:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| index | int | 歌曲在歌单列表中的位置 |

- 返回值
  
  无
  
- 示例代码
  
  ```
   [self.devicePlayer playCurrentPlayListWithIndex:1 completionHandler:^(BOOL isSuccess, NSString * _Nullable result) {
  
     NSLog(@"Play successfully");
  
   }];
  ```
  

#### 删除当前播放歌单中的歌曲

从正在播放的歌单列表中，删除指定的歌曲。

- 接口说明
  
  ```
    - (void)deleteWithIndex:(NSInteger)index completionHandler:(SWPlayerBlock _Nullable)completionHandler
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| index | NSInteger | 歌曲在歌单列表中的位置 |

- 返回值
  
  无
  
- 示例代码
  
  ```
   [[device getPlayer] deleteWithIndex:index completionHandler:^(BOOL isSuccess, NSString * _Nullable result) {
            if (isSuccess) {
                NSLog(@"Delete index success");
            }
        }];
  
    }];
  ```
  

### 设置设备名称

- 接口说明
  
  ```
  - (void)setDeviceName:(NSString *)deviceName completionHandler:(SWSDKReturnBlock _Nullable)completionHandler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| deviceName | NSString | 设备名称 |

- 返回值
  
  无
  
- 示例代码
  
  ```
  [device.deviceSettings setDeviceName:@"Music" completionHandler:^(NSURLResponse * _Nullable response, id  _Nullable responseObject, NSError * _Nullable error) {
      if ([responseObject[@"result"] isEqualToString:@"OK"]) {
         NSLog(@"Set device name successfully");
      }
  }];
  ```
  

### 恢复出厂设置

- 接口说明
  
  ```
  - (void)resetDeviceWithHandler:(SWSDKReturnBlock _Nullable)completionHandler;
  ```
  
- 参数
  
  无
  
- 返回值
  
  无
  
- 示例代码
  
  ```
  [device.deviceSettings resetDeviceWithHandler:^(NSURLResponse * _Nullable response, id  _Nullable responseObject, NSError * _Nullable error) {
      if ([responseObject[@"result"] isEqualToString:@"OK"]) {
         NSLog(@"Reset device successfully");
      }
  }];
  ```
  

###

# 多设备同步播放音乐

# 简介

Multi-Room Music 是指同一个局域网内的多台设备同步播放、操控的技术。

每个房间有一个“主设备”（master device），用来同步房间内的播放、控制信息，而其他被动接收同步指令的设备称为“子设备”(slave device)。

你可以通过SWMultiroomManager 来管理多台设备同步播放音

#### 同步多台设备

- 接口说明
  
  masterDevice “主设备”，slaveDeviceList 是房间内“子设备”列表。
  
  如果传入的masterDevice 不为nil，则slaveDeviceList中的设备，会和masterDevice组成房间，同步播放音乐。
  
  ```
    - (void)deviceMultiroomWithSlaveDeviceList:(NSArray<SWDevice *> * _Nonnull)slaveDeviceList masterDevice:(SWDevice * _Nullable)masterDevice handler:(SWMultiroomBlock)handler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| slaveDeviceList | NSArray | 如果masterDevice为nil,列表中的设备从房间内删除，否则，列表中的设备会和masterDevice组成房间 |
| masterDevice | SWDevice | 设备对象 |
| handler | void (^SWMultiroomBlock)(BOOL isSuccess) |     |

- 返回值
  
  无
  
- 示例代码
  
  ```
  [[SWMultiroomManager sharedInstance] deviceMultiroomWithSlaveDeviceList:self.CurrChildSelectedArray masterDevice:self.boxInfo handler:^(BOOL isSuccess, NSString * _Nullable result) {
  
          }];
  ```
  

#### 解绑多台设备

- 接口说明
  
  masterDevice “主设备”，slaveDeviceList 是房间内需要解绑的“子设备”列表。
  
  如果传入的masterDevice 不为nil，则slaveDeviceList中的设备，会退出masterDevice组成房间。
  
  ```
    - (void)deviceMultiroomWithSlaveDeviceList:(NSArray<SWDevice *> * _Nonnull)slaveDeviceList masterDevice:(SWDevice * _Nullable)masterDevice handler:(SWMultiroomBlock)handler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| slaveDeviceList | NSArray | 需要退出房间的子设备列表 |
| masterDevice | SWDevice | 设备对象 |
| handler | void (^SWMultiroomBlock)(BOOL isSuccess) |     |

- 返回值
  
  无
  
- 示例代码
  
  ```
  [[SWMultiroomManager sharedInstance] deviceMultiroomWithSlaveDeviceList:self.CurrChildSelectedArray masterDevice:self.boxInfo handler:^(BOOL isSuccess, NSString * _Nullable result) {
  
          }];
  ```
  

# 设备播放音乐设置

# 简介

对设备播放音乐的信息设置，如声道、播放模式、进度等信息。

#### 播放进度

- 接口说明
  
  ```
    - (void)setProgress:(NSTimeInterval)progress completionHandler:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| progress | NSTimeInterval | 播放进度 |

- 返回值
  
  无
  

#### 播放声道

- 接口说明
  
  ```
    - (void)setChannel:(SWDeviceChannel)channel completionHandler:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| channel | SWDeviceChannel | 播放声道 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    SWDeviceChannel channel = SWChannel_left;
    [[device getPlayer] setChannel:channel completionHandler:^(BOOL isSuccess, NSString * _Nullable result) {
        if (isSuccess) {
            NSLog(@"Channel setup successful");
        }
    }];
  ```
  

#### 播放模式

- 接口说明
  
  ```
    - (void)setPlayMode:(SWPlayMode)playMode completionHandler:(SWPlayerBlock _Nullable)completionHandler;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| playMode | SWPlayMode | 播放模式 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[self.boxInfo getPlayer] setPlayMode:mode completionHandler:^(BOOL isSuccess, NSString * _Nullable result) {
  
    }];
  ```
  

####

#### 音量

##### 设置主设备音量

- 接口说明
  
  ```
    - (void)setVolume:(CGFloat)volume single:(BOOL)single;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| volume | CGFloat | 设备音量 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[[[SWDeviceManager sharedInstance] deviceForIP:_model.mainIp] getPlayer] setVolume:volume];
  ```
  

##### 设置子设备音量

- 接口说明
  
  ```
    - (void)setVolumeWithSrcIp:(NSString *)srcip :(CGFloat)volume;
  ```
  
- 参数
  

| 名称  | 类型  | 接口说明 |
| --- | --- | --- |
| volume | CGFloat | 设备音量 |

- 返回值
  
  无
  
- 示例代码
  
  ```
    [[[[SWDeviceManager sharedInstance] deviceForIP:_model.mainIp] getPlayer] setVolumeWithSrcIp:_model.ip :volume];
  ```
  

## 作者

珠海惠威科技有限公司，[david.li@swanspeakers.com](mailto:david.li@swanspeakers.com)

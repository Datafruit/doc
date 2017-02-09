# JavaScript SDK API 集成

数果星盘 提供 JS SDK,将代码插入到客户的网页中以后我们就可以接收用户的行为数据了。

我们的 JS SDK 支持 IE9 以上的 IE 浏览器、360 浏览器、谷歌浏览器、搜狗浏览器、火狐浏览器、QQ 浏览器、Safari 浏览器、Maxthon、Mobile 端浏览器，并且全面支持 H5，覆盖市面上主流的浏览器。

# 获取JS SDK
1.登录 数果星盘 
2.点击项目管理菜单下的“数据接入”
3.点击项目列表的“数据接入”按钮 (如果未创建项目，先点击“新建项目”)
![image](https://cloud.githubusercontent.com/assets/5144010/22733044/5db7e49a-ee2b-11e6-8c32-4d551ab00454.png)
4.点击主分析表下的 “添加数据接入方式”
5. 选择平台「网站」
![image](https://cloud.githubusercontent.com/assets/5144010/22734137/a290493c-ee2f-11e6-8ea5-29414459102d.png)
![image](https://cloud.githubusercontent.com/assets/5144010/22738759/b2968a08-ee43-11e6-84b3-1390fe237ad5.png)
# 整合JS SDK至web 环境
将我们提供给您的 JS SDK 加入到您所需要分析的页面，请将我们给您提供的 JS SDK 复制到 <head> 和 </head> 标签之间即可, 请务必更改“YOUR_TOKEN” , “YOUR_PROJECT_ID” 参数。您可以在数果星盘 Web应用程序的项目管理的具体项目下的编辑界面的“查看设置”看到对应的TOKEN和PROJECT_ID。例如：
![image](https://cloud.githubusercontent.com/assets/5144010/22738714/77dfac78-ee43-11e6-9676-9498073fc26f.png)

# 异步载入
```
<head>
...
<!-- start sugoio -->
  <script type='text/javascript'>
    (function(e,a){if(!a.__SV){var b=window;try{var c,n,k,l=b.location,g=l.hash;c=function(a,b){return(n=a.match(new RegExp(b+"=([^&]*)")))?n[1]:null};g&&c(g,"state")&&(k=JSON.parse(decodeURIComponent(c(g,"state"))),"mpeditor"===k.action&&(b.sessionStorage.setItem("_mpcehash",g),history.replaceState(k.desiredHash||"",e.title,l.pathname+l.search)))}catch(p){}var m,h;window.sugoio=a;a._i=[];a.init=function(b,c,f){function e(b,a){var c=a.split(".");2==c.length&&(b=b[c[0]],a=c[1]);b[a]=function(){b.push([a].concat(Array.prototype.slice.call(arguments,
    0)))}}var d=a;"undefined"!==typeof f?d=a[f]=[]:f="sugoio";d.people=d.people||[];d.toString=function(b){var a="sugoio";"sugoio"!==f&&(a+="."+f);b||(a+=" (stub)");return a};d.people.toString=function(){return d.toString(1)+".people (stub)"};m="disable time_event track track_pageview track_links track_forms register register_once alias unregister identify name_tag set_config reset people.set people.set_once people.increment people.append people.union people.track_charge people.clear_charges people.delete_user".split(" ");
    for(h=0;h<m.length;h++)e(d,m[h]);a._i.push([b,c,f])};a.__SV=1.2;b=e.createElement("script");b.type="text/javascript";b.async=!0;"undefined"!==typeof SUGOIO_CUSTOM_LIB_URL?b.src=SUGOIO_CUSTOM_LIB_URL:b.src="file:"===e.location.protocol&&"//localhost:8080/_bc/sugo-sdk-js/libs/sugoio-latest.min.js".match(/^\/\//)?"https://localhost:8080/_bc/sugo-sdk-js/libs/sugoio-latest.min.js":"//localhost:8080/_bc/sugo-sdk-js/libs/sugoio-latest.min.js";c=e.getElementsByTagName("script")[0];c.parentNode.insertBefore(b,
    c)}})(document,window.sugoio||[]);
    
    sugoio.init('YOUR_TOKEN', {'project_id': 'YOUR_PROJECT_ID'});
  </script>
<!-- end sugoio -->
...
</head>
```
以上片段将异步方式加载我们库到页面，并提供了一个全局变量名为sugoio，你可以用它来代码埋点上报数据。
成功加载SDK后数果星盘会自动采集页面浏览量。我们还提供一些高级设置：用户属性、页面属性、代码注入等。具体设置方法请参考下面的详细说明。
# init参数配置
```
 sugoio.init('YOUR_TOKEN',  {                      // 项目TOKEN
   project_id: 'YOUR_PROJECT_ID',              // 项目ID
   api_host: '',                                                 // sugoio-latest.min.js文件以及数据上报的地址
   app_host: '',                                                // 可视化配置时服务端地址
   decide_host: '',                                           // 加载已埋点配置地址
   loaded: function(lib) { },                             // sugoio sdk 加载完成回调函数
   dimensions: { },                                          // 上报维度自定义映射配置参数
   DEBUG: false                                              // 是否启用debug
});
```
- **YOUR_TOKEN：** 为项目TOKEN。
- **YOUR_PROJECT_ID：**  为项目ID。
- **api_host：** sugoio-latest.min.js文件以及数据上报的地址。
- **app_host：**  可视化配置时服务端地址。
- **decide_host：**  加载已埋点配置地址。
- **loaded：**  sugoio sdk 加载完成回调函数。
- **dimensions：**  上报维度自定义映射配置参数， 下文详细说明。
- **DEBUG：**  是否启用debug。

# 用户自定义维度
数果星盘 的数据分析工具本身提供了例如 “访问来源”，“城市”,“操作系统"，”浏览器“等等这些维度。这些维度都可以和用户创建的指标进行多维的分析。但是往往不能满足用户对数据多维度分析的要求，因为每个公司的产品都有各自的用户维度，比如客户所服务的公司，用户正在使用的产品版本等等。
数果星盘 为了能够让数据分析变得更加的灵活，我们在 JS SDK 中提供了用户自定义维度的API接口:

## 自定义事件上报(代码埋点上报)
第一次接入 数果星盘时，建议先追踪 3~5 个关键的事件，只需要几行代码，便能体验 数果星盘 的分析功能。例如：
- 图片社交产品，可以追踪用户浏览图片和评论事件。
- 电商产品，可以追踪用户注册、浏览商品和下订单等事件。

数果星盘 SDK 初始化成功后，即可以通过 sugoio.track(event_name, [properties], [callback]) 记录事件：
- **event_name**: `string`，必选。表示要追踪的事件名。
-  **properties**: `object`，可选。表示这个事件的属性。
- **callback**: `function`，可选。表示已经发送完数据之后的回调。
```
    // 追踪浏览商品事件
    sugoio.track('ViewProduct', {
        'l|ProductId': 123456，
        'ProductCatalog': "Laptop Computer",
        'ProductName': 'MacBook Pro',
        'f|ProductPrice': 888.88,
        'd|ViewDateTime': +new Date()
    }); 
```
# 数据类型说明
- **object:** 上面 properties 是 object 类型，但是里面必须是 key: value 格式。
- **key**的格式为：字段类型+字段名称，字段类型与字段名称以|分割。
- **value**数据类型只能是string/long/float/date这几种类型。默认为string类型，key可以省略前缀数据类型如上：`ProductName`为`string` 等价于 `s|ProductName`。
``` 
* `s`代表string；  
* `l`代表long；  
* `d`代表date；  
* `f`代表float； 
```
## 事件公共属性(超级属性)
sugoio.register
提供全局设置为每条上报记录都设置共有属性，在 Cookie 中永久保存属性，永久有效，如果存在这个属性了则覆盖
```
sugoio.register({
  Custom1: 'Custom1',
  Custom2: 'Custom2'
  ...
});
数据类型跟上面提到的一样
```

# 预置属性
```
['sugo_nation', '国家'],                        //用户所在国家(ip反解析)
['sugo_province', '省份'],                      //用户所在省份(ip反解析)
['sugo_city', '城市'],                          //用户所在城市(ip反解析)
['sugo_district', '地区'],                      //用户所在地区(ip反解析)
['sugo_area', '区域'],                          //用户所在区域(ip反解析)
['sugo_latitude', '纬度'],                      //纬度(ip反解析)
['sugo_longitude', '经度'],                     //经度(ip反解析)
['sugo_city_timezone', '城市时区'],             //所在时区代表城市(ip反解析)
['sugo_timezone', '时区'],                      //所在时区(ip反解析)
['sugo_phone_code', '国际区号'],                //国际区号(ip反解析)
['sugo_nation_code', '国家代码'],               //国家代码(ip反解析)
['sugo_continent', '所在大洲'],                 //所在大洲(ip反解析)
['sugo_administrative', '行政区划代码'],        //中国行政区划代码(ip反解析)
['sugo_operator', '运营商'],                    //用户所在运营商(ip反解析)
['sugo_ip', '客户端IP'],                        //客户端IP(nginx)
['sugo_http_forward', '客户端真实IP'],          //客户端真实ip(nginx)
['sugo_http_refer', 'Referer'],                //Referer(nginx)
['sugo_user_agent', '浏览器标识'],              //浏览器标识(nginx)
['brower, '浏览器名称'],  	             //浏览器名称
['brower_version', '浏览器版本'],        //浏览器版本
['sugo_args', '请求参数'],                      //请求参数(nginx)
['sugo_http_cookie', 'HttpCookie'],            //HttpCookie(nginx)
['app_name', '系统名称'],                      //系统或app的系统名称
['app_version', '系统版本'],                   //系统或app的系统版本
['app_build_number', 'android build number'], //android build number
['session_id','会话ID'],                      //会话id
['network', '网络类型'],                      //用户使用的网络
['device_id','设备ID'],                       //浏览器cookies（首次访问时生成)/Android或ios device id
['bluetooth_version', '蓝牙版本'],            //用户蓝牙版本
['has_bluetooth', '蓝牙版本'],                //用户是否有蓝牙
['device_brand', '品牌'],                     //用户电脑、平板、或手机牌子
['device_model', '品牌型号'],                 //用户电脑、平板、或手机型号
['system_name', '浏览器/android/ios'],       //用户系统、平板或手机OS
['system_version', '操作系统版本'],           //用户平板或手机OS版本
['radio', '通信协议'],                       //通信协议
['carrier', '运营商'],                       //运营商
['screen_dpi', '分辨率'],                    //客户端分辨率
['screen_height', '屏幕高度'],               //客户端屏幕高度
['screen_width', '屏幕宽度'],                //客户端屏幕宽度
['event_time', '客户端事件时间', 4],          //客户端事件发生时间（unix毫秒数)
['current_url', '当前请求地址'],              //客户端当前请求地址
['referrer', '客户引荐'],                    //客户引荐
['referring_domain', '客户引荐域名'],        //客户引荐域名
['host', '客户端域名'],                      //客户端域名
['distinct_id', '用户唯一ID'],               //用户唯一ID
['has_nfc', 'NFC功能'],                      //NFC功能
['has_telephone', '电话功能'],               //电话功能
['has_wifi', 'WIFI功能'],                    //wifi功能
['manufacturer', '设备制造商'],              //设备制造商
['duration', '停留时间', 1],                 //页面停留时间
['sdk_version', 'SDK版本'],                  //sdk版本
['page_name', '页面名称'],                   //页面名称或屏幕名称
['path_name', '页面路径'],                   //页面路径
['event_id', '事件ID'],                      //事件ID
['event_name', '事件名称'],                  //事件名称
['event_type', '事件类型'],                  //事件类型click、focus、submit、change
['event_label', '事件源文本'],                //事件源文本
['sugo_lib', 'sdk类型'],                     //sdk类型 web、ios、android
['token', '应用ID'],                         //应用ID
['from_binding', '是否绑定事件'],             //是否绑定事件（用于区分是绑定的，还是系统自动上报的，比如浏览、启动为自动上报，绑定取值1，自动上报取值0）
['google_play_services', 'GooglePlay服务'],  //GooglePlay服务
```

# 维度映射
```
   sugoio.init('YOUR_TOKEN', {
      project_id: 'YOUR_PROJECT_ID',
      dimensions: {
        has_wifi: 'wifi',
        sdk_version: 'appVersion'
        ...
      }
     ...
    });
```

# 页面停留事件
```
    sugoio.init('YOUR_TOKEN', {
       project_id: 'YOUR_PROJECT_ID',
       loaded: function(lib) {
        sugoio.time_event('停留')
        sugoio._.register_event(window, 'beforeunload', function(){
          sugoio.track('停留', {path_name: location.pathname})
        }, false, true)
      }
      ....
    });
```
##  sugoio.register_once(object)
在 Cookie 中永久保存属性，如果存在这个属性了则不覆盖


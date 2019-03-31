---
layout: post
title:  "使用shell脚本连接机器人发送消息"
date:   2019-04-01
excerpt: "十六说：“这是一篇关于‘钉钉开放平台——自定义机器人’的抄录笔记，并附上了个人项目”"
project: true
tag:
- 钉钉 
- shell
- 消息
- 脚本
comments: true
---
# 1.获取自定义机器人
&emsp;点击左上角自己的头像，点击`机器人管理`，在机器人管理页面选择`自定义机器人`，输入机器人名字并选择要发送消息的群。如果需要的话，可以为机器人设置一个头像，点击`完成`。并复制机器人对应的Webhook地址。

![获取机器人]()

![获取机器人2]()

![获取机器人3]()

# 2.使用自定义机器人
* 获取到Webhook地址后，用户可以使用任何方式访问这个地址发起HTTP POST请求，即可实现给群组发送消息。注意，发起POST请求时，必须要将字符集编码设置成UTF-8.
* 当前机器人支持文本（text）、连接（link）、markdown（markdown）三种消息类型，大家可以根据自己的使用场景选择合适的消息类型，达到最好的展示样式。
* 自定义机器人发送消息时，可以通过手机号指定“被@人列表”。在“被@人列表”里面的人员，在收到该消息时，会有@消息提醒。

# 3.消息类型及数据格式
## 3.1 文本类型（text）
&emsp;文本类型
```
{
     "msgtype": "text",
     "text": {
         "content": "我是陈十六，请多指教@156****16**"
     },
     "at": {
         "atMobiles": [
             "1825718XXXX"
         ], 
         "isAtAll": false
     }
 }
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定text
content | true | string |内容消息
atMobiles| false | string | 被@人手机号
isAtall | false | bool | @所有人时为ture，否则为false
![text消息]()

## 3.2 link类型
```
{
    "msgtype": "link", 
    "link": {
        "text":"link是以连接的形式发送消息。", 
       "title": "连接的主题", 
        "picUrl": "", 
        "messageUrl": "复制的机器人的Webhoot"
    }
}
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定link
title | true | string | 消息标题
text | true | string |内容消息，如果太长只会显示一部分
messageUrl | true | string | 点击消息跳转的url
picUrl | false | string | 图片url
![line消息]()

## 3.3 markdown类型
```
{
     "msgtype": "markdown",
     "markdown": {"title":"杭州天气",
"text":"#### 杭州天气  \n > 9度，@1825718XXXX 西北风1级，空气良89，相对温度73%\n\n > ![screenshot](http://i01.lw.aliimg.com/media/lALPBbCc1ZhJGIvNAkzNBLA_1200_588.png)\n  > ###### 10点20分发布 [天气](http://www.thinkpage.cn/) "
     },
    "at": {
        "atMobiles": [
            "1825718XXXX"
        ], 
        "isAtAll": false
    }
 }
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定markdown
title | true | string | 首屏会话透出的展示内容
text | true | string | markdown消息格式
atMobiles| false | string | 被@人手机号（在text内容里要有@手机号）
isAtall | false | bool | @所有人时为ture，否则为false
![markdown消息]()

说明：目前只支持markdown语法的子集，具体支持的元素如下：
```
标题
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
 
引用
> A man who stands for nothing will fall for anything.
 
文字加粗、斜体
**bold**
*italic*
 
链接
[this is a link](https://www.together16.com/)
 
图片
![](http://together16.com/陈十六.jpg)
 
无序列表
- item1
- item2
 
有序列表
1. item1
2. item2
```

## 3.4 ActionCard
### 3.4.1 整体跳转ActionCard类型
```
{
    "actionCard": {
        "title": "乔布斯 20 年前想打造一间苹果咖啡厅，而它正是 Apple Store 的前身", 
         "text": "![screenshot](@lADOpwk3K80C0M0FoA) \n #### 乔布斯 20 年前想打造的苹果咖啡厅 \n\n Apple Store 的设计正从原来满满的科技感走向生活化，而其生活化的走向其实可以追溯到 20 年前苹果一个建立咖啡馆的计划", 
        "hideAvatar": "0", 
        "btnOrientation": "0", 
        "singleTitle" : "阅读全文",
        "singleURL" : "https://www.dingtalk.com/"
    }, 
    "msgtype": "actionCard"
}
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定actionCard
title | true | string | 首屏会话透出的展示内容
text | true | string | markdown消息格式
singleTitle | true | string | 单个按钮的方案（设置此项和singleURL后btns无效）
singleURL | true | string | 点击singleTitle按钮触发的URL
btnOrientation | false | string |0-按钮竖直排列，1-按钮横向排列
hideAvatar | false | string | 0-正常发消息者头像，1-隐藏发消息者头像
![]()

### 3.4.2 独立跳转ActionCard类型
```
{
    "actionCard": {
        "title": "乔布斯 20 年前想打造一间苹果咖啡厅，而它正是 Apple Store 的前身", 
        "text": "![screenshot](@lADOpwk3K80C0M0FoA) \n\n #### 乔布斯 20 年前想打造的苹果咖啡厅 \n\n Apple Store 的设计正从原来满满的科技感走向生活化，而其生活化的走向其实可以追溯到 20 年前苹果一个建立咖啡馆的计划", 
        "hideAvatar": "0", 
        "btnOrientation": "0", 
        "btns": [
            {
                "title": "内容不错", 
                "actionURL": "https://www.dingtalk.com/"
            }, 
            {
                "title": "不感兴趣", 
                "actionURL": "https://www.dingtalk.com/"
            }
        ]
    }, 
    "msgtype": "actionCard"
}
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定actionCard
title | true | string | 首屏会话透出的展示内容
text | true | string | markdown消息格式
btns | true | array | 按钮的信息：title-按钮方案，actionURL-点击按钮触发URL
btnOrientation | false | string | 0-按钮竖直排列，1-按钮横向排列
hideAvatar | false | string | 0-正常发消息者头像，1-隐藏发消息者头像
![]()
## 3.5 FeedCard类型
```
{
    "feedCard": {
        "links": [
            {
                "title": "时代的火车向前开", 
                "messageURL": "https://mp.weixin.qq.com/s?__biz=MzA4NjMwMTA2Ng==&mid=2650316842&idx=1&sn=60da3ea2b29f1dcc43a7c8e4a7c97a16&scene=2&srcid=09189AnRJEdIiWVaKltFzNTw&from=timeline&isappinstalled=0&key=&ascene=2&uin=&devicetype=android-23&version=26031933&nettype=WIFI", 
                "picURL": "https://www.dingtalk.com/"
            },
            {
                "title": "时代的火车向前开2", 
                "messageURL": "https://mp.weixin.qq.com/s?__biz=MzA4NjMwMTA2Ng==&mid=2650316842&idx=1&sn=60da3ea2b29f1dcc43a7c8e4a7c97a16&scene=2&srcid=09189AnRJEdIiWVaKltFzNTw&from=timeline&isappinstalled=0&key=&ascene=2&uin=&devicetype=android-23&version=26031933&nettype=WIFI", 
                "picURL": "https://www.dingtalk.com/"
            }
        ]
    }, 
    "msgtype": "feedCard"
}
```
参数 | 必选 | 类型 | 说明
:-: | :-: | :-: | :-: |
msgtype | true | string |此消息类型为固定feedCard
title | true | string | 单条信息文本
messageURL | true | string |点击单挑信息到跳转链接
picURL | true | string | 单条信息后面图片的URL
![]()

# 4.测试自定义机器人
* 使用命令行工具
```
curl 'https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxx' \
   -H 'Content-Type: application/json' \
   -d '
  {"msgtype": "text", 
    "text": {
        "content": "十六的机器人"
     }
  }'
```
* Java程序测试
```
import org.apache.http.HttpResponse;
import org.apache.http.HttpStatus;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
 
 
public class ChatbotSend {
 
    public static String WEBHOOK_TOKEN = "https://oapi.dingtalk.com/robot/send?access_token=xxxxxx";
 
    public static void main(String args[]) throws Exception{
 
        HttpClient httpclient = HttpClients.createDefault();
 
        HttpPost httppost = new HttpPost(WEBHOOK_TOKEN);
        httppost.addHeader("Content-Type", "application/json; charset=utf-8");
 
        String textMsg = "{ \"msgtype\": \"text\", \"text\": {\"content\": \"我就是我, 是不一样的烟火\"}}";
        StringEntity se = new StringEntity(textMsg, "utf-8");
        httppost.setEntity(se);
 
        HttpResponse response = httpclient.execute(httppost);
        if (response.getStatusLine().getStatusCode()== HttpStatus.SC_OK){
            String result= EntityUtils.toString(response.getEntity(), "utf-8");
            System.out.println(result);
        }
    }
}
```
* PHP程序测试
```
<?php  
  
function request_by_curl($remote_server, $post_string) {  
    $ch = curl_init();  
    curl_setopt($ch, CURLOPT_URL, $remote_server);
    curl_setopt($ch, CURLOPT_POST, 1); 
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 5); 
    curl_setopt($ch, CURLOPT_HTTPHEADER, array ('Content-Type: application/json;charset=utf-8'));
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_string);  
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);  
    // 线下环境不用开启curl证书验证, 未调通情况可尝试添加该代码
    // curl_setopt ($ch, CURLOPT_SSL_VERIFYHOST, 0); 
    // curl_setopt ($ch, CURLOPT_SSL_VERIFYPEER, 0);
    $data = curl_exec($ch);
    curl_close($ch);  
               
    return $data;  
}  
 
$webhook = "https://oapi.dingtalk.com/robot/send?access_token=xxxxxx";
$message="我就是我, 是不一样的烟火";
$data = array ('msgtype' => 'text','text' => array ('content' => $message));
$data_string = json_encode($data);
 
$result = request_by_curl($webhook, $data_string);  
echo $result;
 
?>
```
# 5.注
* 每个机器人每分钟最多发送20条。
* 当前机器人尚不支持应答机制

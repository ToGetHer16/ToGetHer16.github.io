---
layout: post
title:  "使用shell脚本连接钉钉机器人发送消息"
date:   2019-04-01
excerpt: "十六说：这是一篇关于‘钉钉开放平台——自定义机器人’的抄录笔记，并附上了个人项目。"
project: true
tag:
- 钉钉 
- shell
- 消息
- 脚本
feature: "https://user-images.githubusercontent.com/45778381/55421187-45c7c780-55ab-11e9-922d-ce698c67094c.png"
comments: true
---
<center><a href="https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7386797.0.0.EtVzwK&source=search&treeId=257&articleId=105735&docType=1#s0"><b>十六的传送门</b></a></center>

# 1.获取自定义机器人
&emsp;点击左上角自己的头像，点击`机器人管理`，在机器人管理页面选择`自定义机器人`，输入机器人名字并选择要发送消息的群。如果需要的话，可以为机器人设置一个头像，点击`完成`。并复制机器人对应的Webhook地址。

![获取机器人](https://user-images.githubusercontent.com/45778381/55420612-0482e800-55aa-11e9-997b-0e33ad351e98.png)

![获取机器人2](https://user-images.githubusercontent.com/45778381/55420650-18c6e500-55aa-11e9-9448-7a96556924d0.png)

![获取机器人3](https://user-images.githubusercontent.com/45778381/55420674-2d0ae200-55aa-11e9-8674-6b04667e467a.png)

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

|参数 | 必选 | 类型 | 说明|
|:-: | :-: | :-: | :-: |
|msgtype | true | string |此消息类型为固定text|
|content | true | string |内容消息|
|atMobiles| false | string | 被@人手机号|
|isAtall | false | bool | @所有人时为ture，否则为false|
{: rules="groups"}

![Snipaste_2019-04-01_23-31-17](https://user-images.githubusercontent.com/45778381/55420740-5592dc00-55aa-11e9-945a-34edcc21fef3.png)

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
{: rules="groups"}

![Snipaste_2019-04-01_23-32-14](https://user-images.githubusercontent.com/45778381/55420833-82df8a00-55aa-11e9-8bf4-263be0717766.png)

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
{: rules="groups"}

![Snipaste_2019-04-01_23-34-04](https://user-images.githubusercontent.com/45778381/55420866-94c12d00-55aa-11e9-842c-7989ee3f1748.png)

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
{: rules="groups"}

![Snipaste_2019-04-01_23-35-11](https://user-images.githubusercontent.com/45778381/55420957-c508cb80-55aa-11e9-878d-c54693a12581.png)

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
{: rules="groups"}

![Snipaste_2019-04-01_23-36-05](https://user-images.githubusercontent.com/45778381/55420997-d520ab00-55aa-11e9-8842-8512085799b7.png)

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
{: rules="groups"}

![Snipaste_2019-04-01_23-37-20](https://user-images.githubusercontent.com/45778381/55421036-ec5f9880-55aa-11e9-9c47-ba0e5b30b833.png)

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

# 6.个人项目
&emsp;编写脚本定时执行，监控这一天中的某种指定类型的任务是否成功，前提是改任务会在开始和成功的时候向指定文件中增加一条指定格式的日志。
&emsp;监控的文件格式：当任务启动时，会向`/data/exportData/${year}/${month}/${day}/import-data-taskflow.${time}`中插入一条以0开头的日志，如果成功了就会插入一条以1开头的日志，如果未成功，则不会插入数据。

```
0,任务一,2019-03-17_03:11:16,陈十六,156xxxx16xx
0,任务二,2019-03-16_16:16:10,小可乐,156xxxx16xx
1,任务一,2019-03-17_03:16:16,陈十六,156xxxx16xx
```
&emsp;执行脚本

```
#!/bin/bash

time=$(date -d "-1 day" "+%Y%m%d")
year=$(date "+%Y")
month=$(date "+%m")
day=(date -d "-1 day" "+%d")
#所要读取文件
filename="/data/exportData/${year}/${month}/${day}/import-data-taskflow.${time}"
#成功表
table0=""
#失败表
table1=""
#按行切分文件
for line in $(cat $filename)
do
    #将行切分为数组
    line=${line//,/ }
    arr=($line)
    #读取开始记录并操作
    if [ ${arr[0]} -eq 0 ]; then
        #按行切分文件
        for line1 in $(cat $filename)
        do
            #将行切分为数组
	    line1=${line1//,/ }
            arr1=($line1)
            #设置失败文本格式
            text="${arr[1]}\\t${arr[2]}\\t${arr[3]}\\t执行失败\\t@${arr[4]}"
            #读取结束记录并操作
            if [ ${arr1[0]} -eq 1 ]; then
                #如果开始记录名在结束记录中也有那么就是执行成功的
                 if [ ${arr[1]} = ${arr1[1]} ]; then
                    #设置成功文本格式
                    text0="${arr[1]}\\t${arr[2]}\\t${arr1[2]}\\t${arr[3]}\\t执行成功"
                    #将所有成功记录拼接成表格
                    table0="${table0}\n${text0}"
                    #并将失败文本格式改为空值
                    text=""
                        break
                 fi
            fi
        done
        #当失败文本格式不为空值进行将失败文本拼接成表格
        if [ -n "${text}" ]; then
            table1="${table1}\n${text}"
        fi
    fi
done
#向钉钉发送消息方法
function SendMessageToDingding(){
    # 发送钉钉消息
    curl "${Dingding_Url}" -H 'Content-Type: application/json' -d "
    {
     \"msgtype\":\"text\",
     \"text\":{\"content\":\"${Body}\"},
     \"at\":{\"atMobiles\":[\"156xxxx16xx\",\"178xxxx83xx\",\"151xxxx17xx\",\"176xxxx95xx\"],\"isAtAll\":false}
    }"
}
#钉钉机器人url
Dingding_Url="https://oapi.dingtalk.com/robot/send?access_token=bde9bdd5287ea9a1757dc7e1a517a5a72d87468a0771a14f3757f708008570"
#钉钉机器人内容格式
Body="嘿~小伙伴们 小艾提醒您：\n\n------------------${time}同步执行成功名单------------------继续加油哦~\n|\t表名称\t|\t开始时间\t|\t结束时间\t|\t执行人\t|\t是否成功\t|${table0}\n\n------------------${time}同步执行失败名单------------------及时处理哦~\n|\t表名称\t|\t开始时间\t|\t执行人\t|\t是否成功\t|\t请您回复\t|${table1} "
#调用发送方法
SendMessageToDingding $Body $Dingding_Url
```

&emsp;结果展示

```
嘿~小伙伴们 小艾提醒您：

-----------------------20190331同步执行成功表-----------------------继续加油哦~
|  表名称  |  开始时间  |  结束时间  |  执行人  |  是否成功  |
任务一  2019-03-17_03:11:16  2019-03-17_03:16:16  陈十六  执行成功

-----------------------0190331同步执行失败表-----------------------及时处理哦~
|  表名称  |  开始时间  |  执行人  |  是否成功  |  请您回复  |
任务二  2019-03-16_16:16:10  小可乐  执行失败  @小可乐
```

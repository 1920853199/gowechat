# gowechat
一款由Go语言开发的微信公众号后台框架，通过本框架可快速搭建起微信公众号后台服务器，支持订阅号和服务号。
由于公众号后台服务在用户交互时大部分情景需要交互响应，此处涉及的模板推荐参考接下来会开源的开发样例。

# 功能
| 功能 | 描述 | 完成状态 |
|:-------:|:----------- |:------:|
| TOKEN定时刷新 | 定时刷新微信服务器Token，默认刷新周期3600秒。 | ✔ |
| 自定义菜单 | 支持自定义菜单设置，启动刷新调整 | ✔ |
| 接收普通消息 | 接收并保存用户返回的普通消息(文本、图片、语音、视频、小视频、地理位置) | ✔ |
| 接收事件推送 | 接收并保存用户与公众号交互产生的事件 | ✔ |
| 被动消息回复 | 支持被动消息回复功能 | ✔ |
| 客服消息 | 支持外部客服消息接入 | ✔ |
| 安全模式| 支持安全模式的加密解密功能 | ✔ |
| 模板消息| 支持模板消息发送 | ✘ |
| 安全模式| 支持安全模式的加密解密功能 | ✘  |


# 使用方法
获取gowechat库
```
go get github.com/dennismao/gowechat
```
程序调用
```
import(
	"github.com/dennismao/wechat"
)
```

# 使用示例

```
package main

import (
	"fmt"
	_ "teaer/routers"

	"github.com/astaxie/beego"
	"github.com/dennismao/gowechat"
)

const (
	Appid     = ""
	Appsecret = ""
	Token     = ""
)

func main() {

	//	Substantialize server
	myWechat, err := gowechat.New("", "", "")
	if err != nil {
		fmt.Println(err)
		return
	}

	//	refresh token
	go myWechat.Token_Refresh()

	//	Initialization
	//	Create custom menu
	err = myWechat.CustomMenuCreate([]gowechat.CustomButton{
		wechat.CustomButton{
			Name: "主菜单",
			SubButton: []gowechat.Button{
				wechat.Button{
					Type: "views",
					Name: "百度搜索",
					Url:  "www.baidu.com",
				},
				gowechat.Button{
					Type: "click",
					Name: "点击事件",
					Key:  "V1000_button1",
				},
			},
		},
	})
	if err != nil {
		fmt.Println(err)
	}

	//	Run the http server
	beego.Run()
}
```

# License
gowechat source code is licensed under the [MIT Licence](./LICENSE)

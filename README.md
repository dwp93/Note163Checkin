# Note163Checkin

## 一、Fork 仓库

**点击右上角的`Fork`**
![fork](https://img.guoqianfan.com/note/2020/08/fork.png)

## 二、添加 Secret

**`Settings`->`Secrets`->`New secret`，添加以下Secret：**
- `Conf`：其值如下：
    ```json
    {
    	"Users": [{
    			"Task": "CC", //自定义名字，选填
    			"Username": "abc@163.com", //账号
    			"Password": "aaa" //密码
    		}, {
    			"Task": "MM",
    			"Username": "123@163.com",
    			"Password": "111"
    		}
    	],
    	"ScKey": "", //见 通知方式
    	"ScType": "Failed", //通知类型. Always:始终通知; Failed:失败时通知; 不填/其他:不通知;
    	"RdsServer": "xxx.redislabs.com:1234", //redis地址，选填
    	"RdsPwd": "ppp" //redis密码，选填
    }
    ```
    - `RdsServer`和`RdsPwd`是选填的，用于配置redis，来存储cookie。后续可以重用这个cookie，避免频繁登录账号。建议配置一下，可以使用[redislabs](https://app.redislabs.com/)的免费套餐。
    - `JsUrl`和`LoginStr`这2个字段是用来登录账号的，已经设置好了默认值，**不建议**修改，所以上面的配置中没有列出来。详细请查看源码。

### 通知方式

2025-8-22 支持**执行命令**进行通知，你可以使用自己喜欢的通知方式，只需要把`通知命令`填写到`Conf`的`ScKey`字段。

**占位符**会被替换为`实际的消息`（当然你也可以写死）：
- `$title`：标题，默认是`Note163Checkin`。
- `$msg`：运行结果。

下面是2个例子：
- server酱（`$title`被写死为`有道云笔记签到`了）：
    ```
	"ScKey": "curl -G \"https://sctapi.ftqq.com/你的Token.send\" --data-urlencode \"title=有道云笔记签到\" --data-urlencode \"desp=$msg\"",
	"ScType": "Always",
    ```

- pushplus：
    ```
	"ScKey": "curl -G \"https://www.pushplus.plus/send?token=你的Token&title=$title\" --data-urlencode \"content=$msg\"",
	"ScType": "Always",
    ```

**步骤图示如下：**
![添加secret](https://img.guoqianfan.com/note/2020/08/添加secret.png)

## 三、运行

**`Actions`->`Run`->`Run workflow`**：
![run-workflow](https://img.guoqianfan.com/note/2020/08/run-workflow.png)

**注意**：本项目**不会**自动运行，需要自行在`.github/workflows/main.yml`添加定时任务。

## 四、查看运行结果

**`Actions`->`Run`->`build`**，能看到下图，表示运行成功
![查看action运行记录](https://img.guoqianfan.com/note/2020/08/查看action运行记录.png)

## 注意事项

24小时内频繁登录可能会触发验证，程序就会登录失败。此时需要在网页上手动登录一次（需要输入验证码），登录成功后再次运行本程序即可。

## 参考

参考了以下项目：
- [ydao](https://github.com/yygtboy/ydao/)
- [node-script](https://github.com/SunSeekerX/node-script)
- [youdaoyun](https://github.com/hezhizheng/youdaoyun)
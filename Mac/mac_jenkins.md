---
tags: mac, jenkins
---

# Jenkins

[Jenkins官网网址](https://www.jenkins.io/zh/)

## 安装

-   安装最新LTS版本: `brew install jenkins-lts`
-   安装指定LTS版本: `brew install jenkins-lts@YOUR_VERSION`
-   启动Jenkins服务: `brew services start jenkins-lts`
-   重启Jenkins服务: `brew services restart jenkins-lts`
-   更新Jenkins版本: `brew upgrade jenkins-lts`

## 插件

### AnsiColor控制台log字体颜色

#### 1 安装

![](assets/imgs/mac/jenkins/jenkins_ansi_color.png)

#### 2 配置

![](assets/imgs/mac/jenkins/jenkins_ansi_color_setup.png)

#### 3 使用

```shell
echo -e "\033[颜色值m 文本"
```

举例：

```shell
echo -e "\033[41;30m红底黑字\033[0m"
echo -e "\033[30m 黑色字 \033[0m"
echo -e "\033[31m 红色字 \033[0m"
echo -e "\033[32m 绿色字 \033[0m"
echo -e "\033[33m 黄色字 \033[0m"
echo -e "\033[46;30m 天蓝底黑字 \033[0m"
echo -e "\033[4;31m 下划线红字 \033[0m"
echo -e "\033[5;34m 红字在闪烁 \033[0m"
```

| 颜色              | 前景色 | 背景色 |
| ----------------- | ------ | ------ |
| 黑色（black） |  30      |  40  |
| 红色（red）|   31    |   41 |
| 绿色（green）|   32   |   42  |
| 黄色（yellow）|   33   |   43  |
| 蓝色（blue）|  34  |  44   |
| 紫红色（magenta）|   35  |  45   |
| 青色（cyan）|   36   |   46   |
| 白色（white）|   37  |  47  |

## API

### 查看Jenkins API的方法

每一个Jenkins页面右下角的**REST API**链接查看

![](assets/imgs/mac/jenkins/jenkins_api_search.png)

### 创建API Token

**用户 > 设置 > API Token** 创建Token并记录下来

![](assets/imgs/mac/jenkins/create_token.png)

Jenkins API的验证采用Basic Auth，**username**是Jenkins用户名，**password**是API Token。例如：

```javascript
const request = axios.create({
	timeout: 60000,
	baseURL: JENKINS_BASE_URL,
	auth: {
		username: 'yinzhongzheng0514',
		password: '1110c53d6f12a5d1a6068815d13302e15a'
	},
	transformRequest: [function (data, headers) {
		console.log("transformRequest", data)
		return Qs.stringify(data)
	}]
})
```

### 查询Job详情

```
/job/Job名称/api/json?tree=name
```

- 方法：GET

例如

```javascript
try {
	await request.get(`/job/${jobName}/api/json?tree=name`)
	// appKey Job存在
} catch {
	// appKey Job不存在 去创建
}
```

### 复制Job

```
/createItem
```

- 方法：POST
- 参数：
	- name：Job名称
	- mode：复制Job的mode参数是**copy**
	- from：被复制的Job名称

例如：

```javascript
await request.post("/createItem", {name: jobName, mode: "copy", from: JENKINS_MODULE_JOB_NAME})
```

### 设置Job状态为Enable

```
/job/Job名称/enable
```

- 方法：POST

### 设置Job状态为Disable

```
/job/Job名称/disable
```

- 方法：POST

### 参数构建

```
/job/Job名称/buildWithParameters
```

- 方法：POST
- 参数：自定义构建的参数

## 定时任务

### 创建定时任务

![](assets/imgs/mac/jenkins/periodically_setup.png)

![](assets/imgs/mac/jenkins/periodically_fair.png)

### 规则语法

#### 1 定时构建语法：* * * * *  
  
五颗星，多个时间点，中间用逗号隔开

- 第一个\*表示分钟，取值0~59  
- 第二个\*表示小时，取值0~23  
- 第三个\*表示一个月的第几天，取值1~31  
- 第四个\*表示第几月，取值1~12  
- 第五个\*表示一周中的第几天，取值0~7，其中0和7代表的都是周日

|                               | *     | *    | *      | *    | *     |
| ----------------------------- | ----- | ---- | ------ | ---- | ----- |
| 含义                          | 分钟  | 小时 | 日期   | 月份 | 星期  |
| 取之范围                      | 0-59  | 0-23 | 1-31   | 1-12 | 0-7   |
|                               |       |      |        |      |       |
| 事例                          |       |      |        |      |       |
| 每隔15分钟执行一次            | H/15  | *    | *      | *    | *     |
| 每隔2个小时执行一次           | H     | H/2  | *      | *    | *     |
| 每隔3天执行一次               | H     | H    | H/3    | *    | *     |
| 每个3天执行一次(每月的1-15号) | H     | H    | 1-15/3 | *    | *     |
| 每周1，3，5执行一次           | H     | H    | *      | *    | 1,3,5 |
|                               |       |      |        |      |       |
| 规则                          |       |      |        |      |       |
| 指定时间范围                  | a-b   |      |        |      |       |
| 指定时间间隔                  | /     |      |        |      |       |
| 指定变量取值                  | a,b,c |      |        |      |       |

#### 2 常用定时构建举例：  

- 每5分钟构建一次：H/5 * * * *  
- 每15分钟运行一次：H/15 * * * *  
- 每30分钟构建一次：H/30 * * * *  
- 每2小时构建一次：H H/2 * * *  
- 每天早上8点构建一次：0 8 * * *  
- 每天中午下班前定时构建一次：0 12 * * *  
- 每天下午下班前定时构建一次：0 18 * * *  
- 每天的8点，12点，22点，一天构建3次：0 8,12,22 * * *  
- 一个小时的前30分钟，每10分钟运行一次 (30分钟, 可能在 4分，14分，24分)：H(0-29)/10 * * * *  
- 每周一至周五，上午9:45到下午3:45，每隔2小时45分钟运行一次：45 9-15/2 * * 1-5  
- 每两小时一次，每个工作日上午9点到下午5点(也许是上午10:38，下午12:38，下午2:38，下午4:38)：H H(9-17)/2 * * 1-5  
- 除12月外，每月1号和15号每天一次：H H 1,15 1-11 *

## 节点

### ssh

### 服务方式

#### 环境问题

添加PATH参数

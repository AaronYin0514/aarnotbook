# Jenkins-Gitlab-Webhook

自动构建

## Jenkins配置

### 源码管理

![mac-jenkins-gitlab-webhook-code.png](mac-jenkins-gitlab-webhook-code.png)

### 触发构建

![](mac-jenkins-gitlab-webhook-build.png)

### 获取Token

点击上一步的“高级”按钮，在下面菜单生成token

![](mac-jenkins-gitlab-webhook-token.png)

### 配置构建脚本

这一步跟普通的构建配置相同

## GitLab

### 配置Webhook

![](mac-jenkins-gitlab-webhook-config.png)

## 测试

在代码中提交一次，就可以触发webhook了

![](mac-jenkins-gitlab-webhook-log.png)

![](mac-jenkins-gitlab-webhook-log2.png)


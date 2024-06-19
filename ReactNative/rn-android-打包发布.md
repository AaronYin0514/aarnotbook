---
tags: rn
---

# Android打包发布

## 生成一个签名密钥

用`keytool`命令生成一个私有密钥。

```shell
keytool -genkeypair -v -storetype PKCS12 -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

运行命令，**输入密码**，和一些一些发行相关的信息。最后生成一个`my-release-key.keystore`的密钥库文件。

密钥有效期为 10000 天。`--alias`参数设置的别名在应用签名时会用到，需要记住这个别名。

> **注意：妥善地保管密钥库文件，一般不要上传到版本库或者其它的地方。**

## 设置 gradle 变量

1.  把`my-release-key.keystore`文件放到工程中的`android/app`目录下。
2.  编辑`~/.gradle/gradle.properties`（全局配置，对所有项目有效）或是项目目录`/android/gradle.properties`（项目配置，只对此项目有效）。如果没有`gradle.properties`文件你就自己创建一个，
3. 添加如下代码（注意把`****`替换为相应密码）

```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```

上面的这些会作为 gradle 的变量，在后面的步骤中可以用来给应用签名。

## 把签名配置加入到项目的 gradle 配置中

编辑项目目录下的`android/app/build.gradle`，添加如下的签名配置：

```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

## 生成发行 APK 包

只需在终端中运行以下命令：

```shell
cd android
./gradlew assembleRelease
```

生成的 APK 文件位于`android/app/build/outputs/apk/release/app-release.apk`，它已经可以用来发布了。
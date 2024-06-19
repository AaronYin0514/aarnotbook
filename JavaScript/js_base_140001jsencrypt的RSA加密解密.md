---
tags: [javascript, rsa]
---

# 基于jsencrypt的RSA加密解密

## RSA非对称加密算法

-   加密和解密使用同样规则的为对称加密, 相比较非对称加密显得更安全
-   非对称加密使用公钥对信息加密, 再通过私钥对公钥加密过的信息进行解密
-   可以在不直接传递密钥的情况下，完成解密

## jsencrypt

一个基于RSA加解密的js库

## 安装

```shell
npm i jsencrypt
```

## 引用

```javascript
import JSEncrypt from 'jsencrypt'
```

## 加密

使用公钥结合`jsencrypt`提供的`encrypt`方法(需要加密的内容)进行加密

```javascript
var encryptor = new JSEncrypt()  // 创建加密对象实例
//之前ssl生成的公钥，复制的时候要小心不要有空格
var pubKey =  `公钥`
encryptor.setPublicKey(pubKey)//设置公钥
var rsaPassWord = encryptor.encrypt('需加密的内容....')  // 对内容进行加密
```

## 解密

使用私钥结合`jsencrypt`提供的`decrypt`方法进行解密

```javascript
var decrypt = new JSEncrypt()//创建解密对象实例
//之前ssl生成的秘钥
var priKey  = `私钥`
decrypt.setPrivateKey(priKey)//设置秘钥
var uncrypted = decrypt.decrypt(rsaPassWord)//解密之前拿公钥加密的内容
console.log('解密后:',uncrypted);
```

title: android-application-security
date: 2016-02-29 10:50:07
tags: android_application_security
---
```
1. RSA 加密算法是一种基于非对称加密的算法，在两台机器之间传递的是Public　key．
原理是若干个素数相乘容易得到一个乘积，但该乘积不容易被分解若干个素数．公开密钥用
来加密，私钥用来解密．公开密钥由私钥得到，但是由公开密钥确很难得到私钥，这就是RSA
加密的基本原理．
2. ssh登录的原理：
在远端产生一对密钥，将密钥对存储到远程端，在本地申请登录的时候，远程端发过来一
个公开密钥，若本地加密信息，并把解密信息返回远端，远端解密验证通过后，成功
登录．
3. http与https的区别
https需要在SSL(安全套接字层)实现信息加密和身份验证．一般来说，服务器需要一个由机构颁发的
CA证书（相当于我们的身份证），表示server的身份合法性．在客户端创建密钥，并且由Server的CA,
来换取密钥．CA实质上是public　crt和private crt;当客户端发送请求时，用户端把public　crt,
发给客户端，如果public　crt 是真实的，那么，客户端产生一个随机key,并且由public crt加密
该key并且发送给服务端，服务端把key用private crt以解密，用key加密response的内容，发给客
户端，客户端用key解密．
http使用的默认端口80,https使用的端口是443．

```

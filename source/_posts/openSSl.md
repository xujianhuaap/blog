---
title: openSSl
date: 2017-12-08 17:38:39
tags: TLS
---
```
1. TLS (传输层安全）是SSL的接任者, 目的是实现网络通信双方的数据加密和完整性。
	3.0 SSL
	3.1 TLSv1.0
	3.2 TLSv1.1
	3.3 TLSv1.3 (提案）
2. TLS的握手操作
	1> clienthello 由客户端向服务端发送一下信息
		客户端所支持的TLS的版本号
		一个随机对象‘
		所支持的加密套件
		压缩方法
		extension
	2> serverhello 向客户端传送一下信息：
		服务端所选择的TLS 的版本号
		随机对象
		加密套件（cipher suit)
		extension
		压缩方法
	3> Certificate(server) 如果所选择的加密算法需要certificate
		服务端需要将自己的certificate 信息（certificate chain)
		发给客户端 
	4> Server-key-Exchange 如果所选择的算法,在3>的基础上还不能产生PreMasterSecrete
		那么这个消息就会被客户端发送
	5> Certificate-Request 告诉客户端，自己需要客户端的信息(可选项,紧跟3>或4>）
	6> HelloDone 服务端发出这个信息意味着前期的沟通，已经完成
	7> Certificate (client) 客户端发送证书的信息给服务端(紧跟6>)
	8> Client-key-Exchange ( 补充7>的信息）PremasterSecrete 已经为双方所接受
	9> Certificate-Verfy (紧跟8） 只有在Certificate(client)有的情况下才需要
	10> Finieshed 成功以下数据将是加密和完整性保护的
3. Resumed的握手操作
	1> newTicket
	2> Change-cipher-spec 双方都会发出,告诉接收到的一方，以后收到的数据将是可信的
	
4. ps
	HelloRequest 这个消息服务端会随时发出
```

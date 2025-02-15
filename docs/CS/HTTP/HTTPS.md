在HTTP协议中有可能存在信息窃听或身份伪装等安全问题,用HTTPS通信机制可以有效地防止这些问题

## HTTP的缺点

1. 明文通信
2. 不验证通信方的身份，因此有可能遭遇伪装
3. 无法证明报文的完整性，所以有可能已遭篡改

## HTTPS

HTTP加上SSL和TLS组合使用,加密HTTP的通信内容

虽然使用HTTP协议无法确定通信方，但如果使用SSL则可以.SSL不仅提供加密处理，而且还使用了一种被称为证书的手段，可用于确定方

证书由值得信任的第三方机构颁发，用以证明服务器和客户端是实际存在的

通过使用证书，以证明通信方就是意料中的服务器。这对使用者个人来讲，也减少了个人信息泄露的危险性

HTTP+加密+认证+完整性保护=HTTPS

>通常，HTTP直接和TCP通信。当使用SSL时，则演变成先和 SSL 通信，再由SSL和TCP通信了。简言之，所谓HTTPS，其实就是身披SSL协议这层外壳的HTTP


## 加密

### 共享密钥的困境

加密和解密同用一个密钥的方式成为共享密钥加密,叫做对称密钥加密

以共享密钥方式加密时必须将密钥也发给对方,但是这中间也会被窃听

## 使用两把密钥的公开密钥加密

公开密钥加密使用一对飞对称的密钥,一方面叫私钥,另一把叫公钥

私钥不能让任何人知道,公钥则可以随意发布,任何人都可以获得

使用公开密钥加密之后,发送密文的一方使用对方的公开密钥进行加密处理,对方收到被加密的信息之后,再使用自己的私有密钥进行解密,也不必担心密钥 被攻击者窃听而盗走

![](HTTP/attachments/Pasted%20image%2020250124164107.png)
公开密钥加密与共享密钥加密相比，其处理速度要慢

所以应充分利用两者各自的优势，将多种方法组合起来用于通信。在交换密钥环节使用公开密钥加密方式，之后的建立通信交换报文阶段则使用共享密钥加密方式

	如何证明收到的公开密钥就是原本预想的那台服务器发行的公开密钥。或许在公开密钥传输途中，真正的公开密钥已经被攻击者替换掉了?
	可以使用由数字证书认证机构（CA，Certificate Authority）和其相关机关颁发的公开密钥证书,数字证书认证机构处于客户端与服务器双方都可信赖的第三方机构的立场上.

	此处认证机关的公开密钥必须安全地转交给客户端。使用通信方式 时，如何安全转交是一件很困难的事，因此，多数浏览器开发商发布版 本时，会事先在内部植入常用认证机关的公开密钥



HTTP通信过程包括从**客户端**发往**服务器端**的请求及从**服务器端**返回**客户端**的响应

HTTP报文:用于HTTP协议交互的信息被称为HTTP报文,请求端的HTTP报文叫做请求报文,响应端的报文叫做响应报文,一般由于多行数据构成的字符串文本

HTTP报文大致可以分为报文首部和报文主题两部分

两者最初由出现的空行来划分,通常不一定要有报文主体
![](HTTP/attachments/Pasted%20image%2020250121202234.png)
## 请求报文
请求报文和响应报文的首部内容都有以下数据组成,出现在各种首部字段和状态码

请求行:包含用于请求的方法,请求URI和HTTP版本

状态行:包含表明响应结果的状态码，原因短语和HTTP版本

首部字段:包含表示请求和响应的各种条件和属性的各类首部

其他:可能包含HTTP的RFC里未定义的首部


## 编码提升传输效率

HTTP在传输数据的时候可以按照数据原貌直接传输,但是也可以再传输的时候通过编码提升传输速率,通过在传输时编码能有效处理大量访问请求,但是这样编码的操作也需要计算机完成,消耗CPU资源

### 报文主体和实体主体的区别

报文:http通信的基本单位,由8位组字节流组成,通过http通信传输

实体:作为请求或者响应的有效载荷数据被传输,其内容通过实体首部或者实体主部组成

HTTP报文的主题用于传输请求或者响应的实体主体

通常报文主体等于实体主体.只有当传输中进行编码操作的时候,实体主题的内容发生变化,才导致它和报文主体产生差异


### 压缩传输的内容编码

HTTP中有一种被称为内容编码的功能也起到类似的作用

内容编码指明应用在实体内容上的编码格式,并且保持实体信息原样压缩,内容压缩后的实体有客户端接受并且负责解码

分为以下几种
1. GNU zip
2. compress(UNIX系统的标准压缩)
3. indentity(不进行压缩编码)
4. deflate(zlib)

### 分割发送的分块传输编码

在HTTP通信过程中，请求的编码实体资源尚未全部传输完成之 前，浏览器无法显示请求页面。在传输大容量数据时，通过把数据分割 成多块，能够让浏览器逐步显示页面

分块传输编码会将实体主体分成多个部分（块）。每一块都会用 十六进制来标记块的大小，而实体主体的最后一块会使用“0(CR+LF)” 来标记

使用分块传输编码的实体主体会由**接收的客户端负责解码**，恢复到 编码前的实体主体

## 发送多种数据的多部分对象集合

HTTP协议中也采纳了多部分对象集合，发送的一份报文 主体内可含有多类型实体。通常是在图片或文本文件等上传时使用

多部分对象集合包含的对象如下
- `multipart/form-data` 在`Web`表单文件上传时使用。
- `multipart/byteranges` 状态码`206（Partial Content，部分内容）`响应报文包含了多个范围的内容时使用


`multipart/form-data`的格式

```http
Content-Type: multipart/form-data; boundary = AaB03x
--AaB03
Content-Disposition: form-data; name="field1" Joe Blow
--AaB03x
Content-Disposition: form-data; name="pics"; filename="file1.txt" Content-Type: text/plain ...（file1.txt的数据）...
--AaB03x
```

在首部字段里加上Content-type,并且使用boundary来划分多部分对象指明的各类实体

在boundary字符串指定的各个实体的起始行之前插入`--`标记

在多部分对象集合对应的字符串中插入`--`标记：`--AaB03x--`作为结束

另外， 可以在某个部分中嵌套使用多部分对象集合

## 获取部分内容的范围请求

如果下载中突然网络中断,那么需要一种可以恢复的机制,能从中断处继续恢复下载

请求
```http
GET /tip.jpg HTTP/1.1 
Host: www.usagidesign.jp 
Range: bytes =5001-10000
```
响应
```http
HTTP/1.1 206 Partial Content 
Date: Fri, 13 Jul 2012 04:39:17 GMT Content-Range: bytes 5001-10000/10000 
Content-Length: 5000 
Content-Type: image/jpeg
```

`range`有很多的表现形式

例如
```http
Range:bytes=5001-
```
表示5001字节之后全部的

例如从一开始到3000字节,和5000字节到7000字节的多重范围

```http
Range: bytes=-3000, 5000-7000
```

## 内容协商返回最合适的内容

同一个web有多重语言的版本

当浏览器默认语言是中文时,访问相同的URI的web页面时,会显示对应的英语版或者中文版的web页面,这样的机制叫内容协商

内容协商技术有以下3种类型
- 服务器驱动协商:由服务器端进行内容协商.以请求的首部字段为参考，在服务器端自动处理.但对用户来说,以浏览器发送的信息作为判定的依据,并不一定能筛选出最优内容
- 客户端驱动协商:由客户端进行内容协商的方式.用户从浏览器显示的可选项列表中手动选择.还可以利用JavaScript脚本在Web页面上自动进行上述选择.比如按OS的类型或浏览器类型,自行切换成PC版页面或手机版页面
- 透明协商:是服务器驱动和客户端驱动的结合体,是由服务器端和客户端各自进行内容协商的一种方法


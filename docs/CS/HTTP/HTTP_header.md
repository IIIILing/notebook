学习HTTP首部的结构，以及首部中各字段的用法

## HTTP报文首部

报文首部在客户端和服务器处理的时候起到了至关重要的作用

报文主体则是所需要的用户和资源的信息

中间由一行空行分开

>HTTP协议的请求和响应报文中必定包含HTTP首部。首部内容为客户端和服务器分别处理请求和响应提供所需要的信息。对于客户端用户来说，这些信息中的大部分内容都无须亲自查看

### 请求报文

在请求中,HTTP报文由方法,URI,HTTP版本,HTTP首部字段等部分构成

1. **请求行**(第一行):方法,URI,HTTP版本

2. HTTP**首部字段**:(其他行):包括了请求首部字段,通用首部字段,和实体首部字段

### 相应报文

在相应中,HTTP报文由HTTP版本,状态码(数字和原因短语),HTTP首部三部分构成

1. **状态行**:HTTP版本,状态码

2. **HTTP首部字段**分为:响应首部字段,通用首部字段,实体首部字段

在报文众多的字段中,HTTP首部的字段包含信息最为丰富,首部字段同时存在于请求和响应报文当中,并且涵盖了HTTP报文相关的内容信息

## HTTP首部字段

使用首部字段是为了给浏览器和服务器提供报文主题大小,所使用的语言和认证信息等内容


### HTTP首部字段结构

HTTP首部字段是由于首部字段名和字段值构成的,中间使用冒号分割

```http
首部字段名:字段值
```

例如，在HTTP首部中以Content-Type这个字段来表示报文主体的对象类型。

```http
Content-Type: text/html
```

就以上述示例来看，首部字段名为 Content-Type，字符串text/html 是字段值

另外，字段值对应单个HTTP首部字段可以有多个值

```http
Keep-Alive: timeout=15, max=100
```

	
	当HTTP报文首部中出现了两个或两个以上具有相同首部字段名时会怎么样？这种情况在规范内尚未明确，根据浏览器内部处理逻辑的不同，结果可能并不一致。有些浏览器会优先处理第一次出现的首部字段，而有些则会优先处理最后出现的首部字段

### 四种HTTP首部字段类型

1. 通用首部字段
	1. 请求报文和响应报文两方都会使用的首部
2. 请求首部字段:
	1. 从客户端向服务器端发送请求报文时使用的首部
	2. 补充了请求的附 加内容、客户端信息、响应内容相关优先级等信息
3. 响应首部字段
	1. 从服务器端向客户端返回响应报文时使用的首部
	2. **补充了响应的附加内容**，也会*要求客户端附加额外的内容信息*
4. 实体首部字段:
	1. 针对请求报文和响应报文的实体部分使用的首部
	2. **补充了资源内容,更新时间等与实体有关的信息**

### 首部字段表一览

HTTP/1.1规定了47种首部字段

| 首部字段名               | 说明                              |
| ------------------- | ------------------------------- |
| Cache-Control       | 控制缓存的行为                         |
| Connection          | 逐跳首部、连接的管理                      |
| Date                | 创建报文的日期时间                       |
| Pragma              | 报文指令                            |
| Trailer             | 报文末端的首部一览                       |
| Transfer-Encoding   | 指定报文主体的传输编码方式                   |
| Upgrade             | 升级为其协议                          |
| Via                 | 代理服务器的相关信息                      |
| Warning             | 错误通知                            |
| Accept              | 用户代理可处理的媒体类型                    |
| Accept-Charset      | 优先的字符集                          |
| Accept-Encoding     | 优先的内容编码                         |
| Accept-Language     | 优先的语言（自然语言）                     |
| Authorization       | Web认证信息                         |
| Expect              | 期待服务器的特定行为                      |
| From                | 用户的电子邮箱地址                       |
| Host                | 请求资源所在服务器                       |
| If-Match            | 比较实体标记（ETag）                    |
| If-Modified-Since   | 比较资源的更新时间                       |
| If-None-Match       | 比较实体标记（与If-Match相反）             |
| If-Range            | 资源未更新时发送实体Byte的范围请求             |
| If-Unmodified-Since | 比较资源的更新时间（与If-Modified-Since相反） |
| Max-Forwards        | 最大传输逐跳数                         |
| Proxy-Authorization | 代理服务器要求客户端的认证信息                 |
| Range               | 实体的字节范围请求                       |
| Referer             | 对请求中URI的原始获取方                   |
| TE                  | 传输编码的优先级                        |
| User-Agent          | HTTP客户端程序的信息                    |
| Accept-Ranges       | 是否接受字节范围请求                      |
| Age                 | 推算资源创建经过时间                      |
| ETag                | 资源的匹配信息                         |
| Location            | 令客户端重定向至指定URI                   |
| Proxy-Authenticate  | 代理服务器对客户端的认证信息                  |
| Retry-After         | 对再次发起请求的时机要求                    |
| Server              | HTTP服务器的安装信息                    |
| Vary                | 代理服务器缓存的管理信息                    |
| WWW-Authenticate    | 服务器对客户端的认证信息                    |
| Allow               | 资源可支持的HTTP方法                    |
| Content-Encoding    | 实体主体适用的编码方式                     |
| Content-Language    | 实体主体的自然语言                       |
| Content-Length      | 实体主体的大小（单位：字节）                  |
| Content-Location    | 替代对应资源的URI                      |
| Content-MD5         | 实体主体的报文摘要                       |
| Content-Range       | 实体主体的位置范围                       |
| Content-Type        | 实体主体的媒体类型                       |
| Expires             | 实体主体过期的日期时间                     |
| Last-Modified       | 资源的最后修改日期时间                     |

### 非HTTP/1.1首部字段

还有Cookie、Set-Cookie和Content-Disposition 等在其他RFC中定义的首部字段，它们的使用频率也很高

这些非正式的首部字段统一归纳在RFC4229 HTTP Header Field Registrations中

### End-to-end 首部和Hop-by-hop 首部

HTTP 首部字段将定义成缓存代理和非缓存代理的行为，分成2种类型

1. 端到端首部(End-to-end Header)
2. 逐跳首部(Hop-by-hop Header)

p100(其实没看懂什么意思)

## HTTP/1.1通用首部字段详解

### Cache-Control

通过指定首部字段Cache-Control的指令，就能操作缓存的工作机制

指令的参数是可选的，多个指令之间通过“,”分隔。首部字段 Cache-Control 的指令可用于请求及响应时

```http
Cache-Control: private, max-age=0, no-cache
```

常见指令

1. Public
	1. 明确表示其他用户也可以利用缓存
2. private
	1. 只能提供给特定的对象,这与public的指令的行为相反
	2. 缓存服务器会对特定用户提供缓存服务,其他用户发送信息则不回缓存
3. no-cache
	1. 防止缓存中返回过期的资源
	2. 客户端不会接受缓存过的响应,中间的缓存服务器必须报客户端请求转发给源服务器
	3. 如果服务器返回的响应中包含no-cache指令，那么缓存服务器不能 对资源进行缓存。源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认,且禁止其对响应资源进行缓存操作。
		1. Cache-Control: no-cache=Location
		2. 由服务器返回的响应中，若报文首部字段Cache-Control中对no cache 字段名具体指定参数值，那么客户端在接收到这个被指定参数值 的首部字段对应的响应报文后，就不能使用缓存
		3. 换言之，无参数值的首部字段可以使用缓存。只能在响应指令中指定该参数
4. no-store:控制可执行缓存的对象的指令
	1. 当使用no-store 指令时，暗示请求（和对应的响应）或响应中包 含机密信息
	2. 因此，该指令规定缓存不能在本地存储请求或响应的任一部分。
5. s-maxage指令:指定缓存期限和认证的指令
	1. Cache-Control: s-maxage=604800（单位：秒）
	2. Cache-Control: max-age=604800
		1. s-maxage 指令的功能和 max-age 指令的相同.它们的不同点是 s-maxage 指令只适用于供多位用户使用的公共缓存服务器
		2. 当客户端发送的请求中包含max-age指令时，如果判定缓存资源缓存时间数值比指定时间的数值更小，那么客户端就接收缓存的资源。 另外，当指定max-age值为0，那么缓存服务器通常需要将请求转发给源服务器
		3. 当服务器返回的响应中包含max-age指令时，缓存服务器将不对资 源的有效性再作确认，而max-age数值代表资源保存为缓存的最长时间
6. min-fresh指令
	1. min-fresh 指令要求缓存服务器返回至少还未过指定时间的缓存资源
	2. 比如，当指定min-fresh为60秒后，过了60秒的资源都无法作为 响应返回了`Cache-Control: min-fresh=60`
7. max-stale,`Cache-Control: max-stale=3600`
	1. 使用max-stale 可指示缓存资源，即使过期也照常接收
	2. 如果指令未指定参数值，那么无论经过多久，客户端都会接收响应
	3. 如果指令中指定了具体数值，那么即使过期，只要仍处于max-stale指定的时间内，仍旧会被客户端接
8. only-if-cached:`Cache-Control: only-if-cached`
	1. 使用 only-if-cached 指令表示客户端仅在缓存服务器本地缓存目标资源的情况下才会要求其返回
	2. 若发生请求缓存服务器的本地缓存无响应，则返回状态码504 Gateway Timeout
9. must-revalidate:`Cache-Control: must-revalidate`
	1. 使用must-revalidate 指令，代理会向源服务器再次验证即将返回的响应缓存目前是否仍然有效
	2. 若代理无法连通源服务器再次获取有效资源的话，缓存必须给客户 端一条504（Gateway Timeout）状态码
	3. 另外，使用must-revalidate 指令会忽略请求的max-stale指令（**即使已经在首部使用了max-stale，也不会再有效果**）
10. proxy-revalidate
	1. proxy-revalidate 指令要求所有的缓存服务器在接收到客户端带有该 指令的请求返回响应之前，必须再次验证缓存的有效性
11. no-transform
	1. Cache-Control: no-transform
	2. 使用no-transform 指令规定无论是在请求还是响应中，缓存都不能改变实体主体的媒体类型
	3. 这样做可防止缓存或代理压缩图片等类似操作

 
#### cache-extension token

```http
Cache-Control: private, community="UCI"
```

扩展Cache-Control首部字段内的指令

如上例，`Cache-Control` 首部字段本身没有`community`这个指令。借 助`extension tokens` 实现了该指令的添加。如果缓存服务器不能理解 `community` 这个新指令，就会直接忽略。因此，`extension tokens`**仅对能理解它的缓存服务器来说是有意义的**

### Connection

1. 控制不再转发给代理的首部字段
2. 管理持久连接

#### 控制不再转发给代理的首部字段

```http
Connnection : 不再转发的首部字段名
```

#### 管理持久连接

HTTP/1.1默认的连接都是持久连接,位次客户端会在持久连接上发送请求

服务器发送
```http
Connection: close
```
表示持久连接要结束了

之前的版本会一直发送

```http
Connection : Keep-Alive
```

### Date

表面创建http报文的时间

对格式有严格要求
```http
Date: Tue, 03 Jul 2012 04:40:59 GMT
```

### Trailer

首部字段Trailer会事先说明在报文主体后记录了哪些首部字段

可应用在HTTP/1.1版本分块传输编码时

### Transfer-Encoding

首部字段Transfer-Encoding 规定了传输报文主体时采用的编码方式

HTTP/1.1 的传输编码方式仅对分块传输编码有效

```http
Transfer-Encoding: chunked 
Connection: keep-alive 
cf0 　　←16进制(10进制为3312) 
...3312字节分块数据... 
392 　　←16进制(10进制为914) 
...914字节分块数据... 
0
```
**以上用例中，正如在首部字段Transfer-Encoding中指定的那样，有 效使用分块传输编码，且分别被分成3312字节和914字节大小的分块数据**
### Upgrade

首部字段Upgrade用于检测HTTP协议及其他协议是否可使用更高 的版本进行通信，其参数值可以用来指定一个完全不同的通信协议

对于附有首部字段`Upgrade`的请求，服务器可用`101 Switching Protocols`状态码作为响应返回

使用首部字段`Upgrade`时，还需要额外指定`Connection: Upgrade`

`Upgrade`首部字段产生作用的`Upgrade`对象仅限于客户端和邻接服务器

用`Connection`才可以确保每一个服务器都`Upgrade`

### Via

使用首部字段Via是为了追踪客户端与服务器之间的请求和响应报文的传输路径

报文经过代理或网关时，会先在首部字段Via中附加该服务器的信息，然后再进行转发
![](HTTP/attachments/Pasted%20image%2020250124145533.png)


### Warning

通常会告知用户一些与缓存相关的问题的警告

Warning首部的格式如下

```http
Warning: [警告码][警告的主机:端口号]“[警告内容]”([日期时间])
```
定义了7种警告

## 请求首部字段

请求首部字段是从客户端网服务器发送请求报文

### Accept

```http
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
```


Accept首部字段可通知服务器,用户代理能够处理的媒体类型及媒体类型的相对优先级.可使用`type/subtype`这种形式,一次指定多种媒体类型

- 文本文件 text/html, text/plain, text/css ... application/xhtml+xml, application/xml ...
- 图片文件 image/jpeg, image/gif, image/png ...
- 视频文件 video/mpeg, video/quicktime ...
- 应用程序使用的二进制文件 application/octet-stream, application/zip ...

若想要给显示的媒体类型增加优先级,则使用`q=`来额外表示权重值`A`,用分号`(;)`进行分隔.权重值`q`的范围是`0~1`(可精确到小数点后3位),且1为最大值。不指定权重`q`值时，默认权重为`q=1.0`.当服务器提供多种内容时，将会首先返回权重值最高的媒体类型

### Accept-Charset

`Accept-Charset`首部字段可用来通知服务器用户代理支持的字符集,及字符集的相对优先顺序.另外,可一次性指定多种字符集.与首部字段`Accept`相同的是可用权重q值来表示相对优先级

### Accept-Encoding

`Accept-Encoding`首部字段用来告知服务器用户代理支持的内容编 码及内容编码的优先级顺序.可一次性指定多种内容编码

几种编码例子
1. gzip:由文件压缩程序gzip（GNU zip）生成的编码格式
2. compress:由UNIX文件压缩程序compress生成的编码格式
3. deflate:组合使用zlib格式（RFC1950）及由deflate压缩算法

采用权重q值来表示相对优先级，这点与首部字段Accept相同.另外,也可使用星号（\*）作为通配符,指定任意的编码格式


### Accept-Language

```http
Accept-Language: zh-cn,zh;q=0.7,en-us,en;q=0.3
```

用来告知服务器用户代理能够处理的自然语言集（指中文或英文等），以及自然语言集的相对优先级。可一次指定多种自然语言集

### Authorization

首部字段`Authorization`是用来告知服务器，用户代理的认证信息 （证书值）

通常,想要通过服务器认证的用户代理会在接收到返回的401状态码响应后，把首部字段`Authorization`加入请求中.共用缓存在接收到含有`Authorization` 首部字段的请求时的操作处理会略有差异

可参阅RFC2616

### Expect

客户端使用首部字段`Expect`来告知服务器，期望出现的某种特定 行为。因服务器无法理解客户端的期望作出回应而发生错误时，会返回状态码`417 Expectation Failed`

### From

首部字段From用来告知服务器使用用户代理的用户的电子邮件地址

显示搜索引擎等用户代理的负责人的电子邮件联系方式

### Host

首部字段`Host`会告知服务器，请求的资源所处的**互联网主机名**和**端口号**,Host首部字段在HTTP/1.1规范内是**唯一一个必须被包含在请求内的首部字段**

但如果这时，相同的IP地址下部署运行着多个域名，那么服务器就会无法理解究竟是哪个域名对应的请求

### If-match

```http
If-Match: "123456"
```
首部字段`If-Match`,属附带条件之一,它会告知服务器匹配资源所用的实体标记（ETag）值

不匹配返回`412 Precondition Failed`

还可以使用星号（\*）指定If-Match的字段值。针对这种情况，服务器将会忽略ETag的值，只要资源存在就处理请求

### If-Modified-Since

首部字段`If-Modified-Since`，属附带条件之一，它会告知服务器若 I`f-Modified-Since `字段值早于资源的更新时间，则希望能处理该请求

### If-None-Match

只有在`If-None-Match` 的字段值与`ETag`值不一致时，可处理该请求

与If-Match 首部字段的作用相反

### If-Range

若是跟ETag值或更新的日期时间匹配一致,就作为范围处理,不一致就返回全部资源

```http
Get /index.html
If-Range :"123456"
Range: bytes=5001-10000
```

P131


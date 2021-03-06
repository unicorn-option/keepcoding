[toc]

## 状态码

状态码描述了客户端向服务器端发送请求后，返回的请求结果。借助状态码，用户可以知道服务器端是正常处理了请求，还是出现了错误。

|      | 类别                         | 原因短语                   |
| ---- | ---------------------------- | -------------------------- |
| 1xx  | Infomational（信息性状态码） | 接受的请求正在处理         |
| 2xx  | Success（成功状态码）        | 请求正常处理完成           |
| 3xx  | Redirection（重定向状态码）  | 需要进行附加操作以完成请求 |
| 4xx  | Client Error（客户端错误）   | 服务器无法处理请求         |
| 5xx  | Server Error（服务器错误）   | 服务器处理请求出错         |

## 2xx 成功

### 200 OK

从客户端发来的请求在服务器端被正常处理了。

在响应报文内，随状态码一起返回的信息会因方法的不同而发生改变。比如，使用GET方法时，对应请求资源的实体会作为响应返回；而使用HEAD方法时，对应请求资源的实体主体不随报文首部作为响应返回（即在响应中只返回首部，不会返回实体的主体部分）。

### 203 No Content

该状态码代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。比如，当从浏览器发出请求处理后，返回204响应，那么浏览器显示的页面不发生更新。

一般在只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用。

### 206 Partial Content

该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。响应报文中包含由 Content-Range 指定范围的实体内容。

---

## 3xx 重定向

### 301 Moved Permanently

永久性重定向。该状态码表示请求的资源已被分配了新的URI，以后应使用资源现在所指的URI。也就是说，如果已经把资源对应的URI保存为书签了，这时应该按Location首部字段提示的URI重新保存。

### 302 Found

临时性重定向。该状态码表示请求的资源已被分配了新的URI，希望用户（本次）能使用新的URI访问。

和301 Moved Permanently状态码相似，但302状态码代表的资源不是被永久移动，只是临时性质的。换句话说，已移动的资源对应的URI将来还有可能发生改变。比如，用户把URI保存成书签，但不会像301状态码出现时那样去更新书签，而是仍旧保留返回302状态码的页面对应的URI。

### 303 See Other

该状态码表示由于请求对应的资源存在着另一个URI，应使用GET方法定向获取请求的资源。

303状态码和302 Found状态码有着相同的功能，但303状态码明确表示客户端应当采用GET方法获取资源，这点与302状态码有区别。

比如，当使用POST方法访问CGI程序，其执行后的处理结果是希望客户端能以GET方法重定向到另一个URI上去时，返回303状态码。虽然302 Found状态码也可以实现相同的功能，但这里使用303状态码是最理想的。

小结：

当301、302、303响应状态码返回时，几乎所有的浏览器都会把POST改成GET，并删除请求报文内的主体，之后请求会自动再次发送。301、302标准是禁止将POST方法改变成GET方法的，但实际使用时大家都会这么做。

即：**301永久重定向，302临时重定向，301、302不允许浏览器把post请求改为get。303和302都是临时重定向，但303允许浏览器把post请求改为get。**

### 304 Not Modified

表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但因发生请求未满足条件的情况后，直接返回304 Not Modified（服务器端资源未改变，可直接使用客户端未过期的缓存）。304状态码返回时，不包含任何响应的主体部分。304虽然被划分在3XX类别中，但是和重定向没有关系。

### 307 Temporary Redirect

临时重定向。该状态码与302 Found有着相同的含义。尽管302标准禁止POST变换成GET，但实际使用时大家并不遵守。307会遵照浏览器标准，不会从POST变成GET。但是，对于处理响应时的行为，每种浏览器有可能出现不同的情况。

---

## 4xx 客户端错误

4XX的响应结果表明客户端是发生错误的原因所在。

### 400 Bad Request

请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求。另外，浏览器会像200 OK一样对待该状态码。

### 401 Unauthorized

发送的请求需要有通过HTTP认证（BASIC认证、DIGEST认证）的认证信息。另外若之前已进行过1次请求，则表示用户认证失败。

返回含有401的响应必须包含一个适用于被请求资源的WWW-Authenticate首部用以质询（challenge）用户信息。当浏览器初次接收到401响应，会弹出认证用的对话窗口。

### 403 Forbidden

对请求资源的访问被服务器拒绝了。服务器端没有必要给出拒绝的详细理由，但如果想作说明的话，可以在实体的主体部分对原因进行描述，这样就能让用户看到了。

未获得文件系统的访问授权，访问权限出现某些问题（从未授权的发送源IP地址试图访问）等列举的情况都可能是发生403的原因。

### 404 Not Found

服务器上无法找到请求的资源。除此之外，也可以在服务器端拒绝请求且不想说明理由时使用。

---

## 5XX 服务器错误

### 500 Internal Server Error

服务器端在执行请求时发生了错误。也有可能是Web应用存在的bug或某些临时的故障。

### 503 Service Unavailable

服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。如果事先得知解除以上状况需要的时间，最好写入Retry-After首部字段再返回给客户端。

注：状态码和状况的不一致不少返回的状态码响应都是错误的，但是用户可能察觉不到这点。比如Web应用程序内部发生错误，状态码依然返回200 OK，这种情况也经常遇到。
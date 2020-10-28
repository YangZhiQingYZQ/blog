# HTTP缓存相关

## HTTP协议

HTTP协议（超文本传输协议），是一种网络传输协议，浏览器请求服务器获取内容就是基于http协议或https协议。使得计算机可以在浏览器和服务器之间传输文字、图片、二进制、音频、视频等资源

## HTTP缓存

http缓存策略主要分两种方式：

* 强制缓存（浏览器端执行）
* 协商缓存（服务端执行）

### 缓存优先级

强制缓存 > 协商缓存

强制缓存在第一次请求服务器的时候获取【Expires】、【Cache-Control】

### 强制缓存

浏览器在发送请求时，会先去获取资源缓存的header信息，若命中强缓存信息（cache-control或expires），浏览器就会直接从浏览器获取资源，而不向服务器发送请求了，返回的状态码时200 OK(from memory cache)或200 OK（from disk cache）；

* max-age：根据该值计算一个资源过期时间，如果请求时间在过期时间之前，则命中缓存
* no-cache：不使用本地缓存。需要使用缓存协商，先与服务器确认返回的相应是否被更改，如果之前的响应中存在ETag，那么请求的时候会与服务端验证，如果资源未被更改，则可以避免重新下载
* no-store：直接禁止浏览器缓存数据，每次用户请求该资源，都会向服务器发送一个请求，每次都会下载完整的资源
* public：可以被所有的用户缓存，包括终端用户和CDN等中间代理服务器
* private：只能被中断用户的浏览器缓存，不允许CDN等中继缓存服务器对其缓存

#### 强制缓存的【Expires】、【Cache-Control】

##### Expires

此字段是HTTP1.0版本提出，是浏览器访问服务器时，服务器在ResponseHeader字段中设置改资源过期时间：例如

> Expires:Mon,29 Jun 2020 11:10:23 GMT

上面表示该请求在29 Jun 2020 11：10：23前使用缓存资源，过后再次请求则会请求服务器获取新的资源。所以每次请求前浏览器都会判断Expires字段来决定使用缓存资源还是重新请求服务器

但是Expires的时间是在服务器的ResponseHeader中生成的，所以时间是相对于服务器时间的，一旦服务器时间跟浏览器本地时间不一致则会出现问题，所以Expores并不是一个很好的缓存方法，所以在HTTP1.1提出了【Cache-Control】字段

##### Cache-Control

该字段是HTTP1.1提出的，该字段的值是过期时长（类似一种倒计时的功能），这样即使服务器和浏览器日期时间不一致也不会导致Expires的问题，到了时间自动过期

> Cache-Control:max-age = 6000

上面表示该请求的资源在6000秒后过期，6000秒前使用缓存资源

注意事项：

* 当Expires和Cache-Control同时存在时，优先使用HTTP1.1的Cache-Control
* 当强缓存的【Expires】、【Catch-Content】都不命中时，则进入协商缓存

### 协商缓存

协商缓存是浏览器通过发送请求让服务器告知浏览器缓存是否可用，请求会携带第一次请求返回的有关缓存header字段信息（Last-Modified/If-Modified-Since 和 Etag/If-None-Match）。由服务器根据请求中的相关header信息来比对结果是否协商缓存命中，若命中浏览器会返回新的header信息更新缓存中对应的header信息，但不会返回资源内容

浏览器第一次请求服务器时，服务器会判断Request Headers是否带有缓存标识，若不存在缓存标识，则在Response Headers 添加缓存标识，并返回新的资源

缓存标识分为两种：【Last-Modified】、【ETag】

#### Last-Modified

表示最后修改时间，是指请求的资源的最后修改时间，在浏览器第二次访问服务器时，会在Request Header的If-Modified-Since中带上该字段的值（值来自服务器），服务器接到请求后，会对If-Modified-Since的值与该请求资源的最后修改时间进行对比，若请求的资源最后修改时间大于If-Modified-Sine的值，则返回新的资源，并重新设置Last-Modfied字段的值

以上就是协商缓存：【Last-Modified】的整个执行过程

#### ETag

ETag是对请求资源的内容进行MD5算法，生成一个唯一的标识（hash值）。只要资源文件有所改动，改值就会发生改变，其过程如下：

1. 浏览器第一次请求服务器的时候，服务器会判断Request Headers中的【If-None-Match】是否包含值，若没有该字段则返回新资源，并在Response Headers增加ETag字段，ETag值为请求对应资源的内容生成的hash值
2. 浏览器第二次请求时，会在Request Headers上添加【If-Node-Match】字段，值为服务器返回的ETag的值，服务器接受到请求后，会与请求资源的MD5算法生成的hash值做对比，若相同则返回304告知浏览器使用缓存的资源，若不相同则返回全新的资源给浏览器，并把新的资源hash值通过Response Header的ETag字段返回给浏览器，浏览器在下次请求时带上

#### Last-Modified与ETag对比

* 性能上：Last-Modified > ETag，因为Last-Modified记录的是资源最后修改时间，而ETag则是记录MD5算法生成文件内容的hash值
* 精度上：ETag > Last-Modified，因为ETag是根据内容生成的hash值，对内容极其敏感，而Last-Modified只是记录资源最后一次修改时间

## 总结

* Last-Modified：在服务器生成，存在Response Headers的【Last-Modified】中，浏览器通过Request Headers中的【If-Modified-Since】字段把Last-Modified的值带给服务器
* ETag：在服务器中生成，存在Response Headers的【ETag】字段带给浏览器，浏览器通过Request Headers中的【If-None-Match】字段把【ETag】的值带给服务器

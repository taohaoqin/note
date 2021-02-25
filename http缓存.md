###http缓存
缓存有max-age exprise etag等值，当用户发送http请求后，浏览器会先检查缓存是否过期，主要判断max-age 和exprise，会获得一个过期时间与当前时间做对比，如果缓存没有过期，不用发送请求，直接使用本地缓存。

####etag和if-none-match
服务器在响应报文头中加入etag字段，在资源更新时，服务器段的etag值也会随之更新，浏览器再次请求资源的时候，请求报文中会添加if-none-match字段，他就是上次响应报文中的etag值，服务器会比较etag与if-none-match是否一致，如果一致，表明资源未更新，返回状态码304的响应，继续使用本地缓存。如果不一致，服务器接受请求，返回更新后的资源。

####last-modified last-modified-since
它和etag类似，浏览器第一次向服务器请求资源后，服务器会在响应头上加上last-modified字段，表明资源的最后一次修改时间，浏览器再次请求，会在请求报文上加上last-modified-since，这是上次相应报文中的last-modified的值，服务器会比较是否一致，与etag不同的是，此时响应头不会添加last-modified字段
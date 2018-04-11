# 浏览器缓存

## 基本简介
### Expires(http1.0)
该字段表示缓存到期时间，即有效时间对应的当时服务器的时间，然后设置在header中返回给浏览器，在未过期之前不需要再次请求
```
Expires: Thu, 10 Nov 2017 08:45:11 GMT
```
### Cache-Control(1.1)
该字段表示资源缓存的最大有效时间，在该时间内，客户端不需要向服务器发送请求
```
Cache-Control: max-age=2592000
```
- max-age：即最大有效时间，在上面的例子中我们可以看到
- no-cache：表示没有缓存，即告诉浏览器该资源并没有设置缓存
- s-maxage：同max-age，但是仅用于共享缓存，如CDN缓存
- public：多用户共享缓存，默认设置
- private：不能够多用户共享，HTTP认证之后，字段会自动转换成private。

### Last-Modified
服务器告知客户端，资源最后一次被修改的时间
```
Last-Modified: Thu, 10 Nov 2015 08:45:11 GMT
```
If-Modified-Since：再次请求时，请求头中带有该字段，服务器会将If-Modified-Since的值与Last-Modified字段进行对比，如果相等，则表示未修改，响应304；反之，则表示修改了，响应200状态码，返回数据。
这个字段可以和Cache-Control配合使用。[GET HEAD]
```
If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT
```
缺陷：
1. 如果资源更新的速度是秒以下单位，那么该缓存是不能被使用的，因为它的时间单位最低是秒。
1. 如果文件是通过服务器动态生成的，那么该方法的更新时间永远是生成的时间，尽管文件可能没有变化，所以起不到缓存的作用。

### Etag
Etag存储的是文件的特殊标识(一般都是hash生成的)，服务器存储着文件的Etag字段，与每次客户端传送If-None-Match的字段进行比较，如果相等，则表示未修改，响应304；反之，则表示已修改，响应200状态码，返回数据。与每次客户端传送If-Match的字段进行比较,如果相等，返回数据；反之，表示修改返回412。
头
```
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
ETag: W/"0815"

If-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"
If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```


## 刷新触发
|用户操作|Expires/Cache-Control|Last-Modified/Etag|
|:---|:---|:---|
|地址栏回车|有效|有效|
|新开窗口|有效|有效|
|前进后退|有效|有效|
|F5刷新|无效(重置max-age=0)|有效|
|Ctrl+F5刷新|无效(重置Cache-Control=no-cache)|无效|


## 备注
- Etag是服务器自动生成或者由开发者生成的对应资源在服务器端的唯一标识符，能够更加准确的控制缓存，但是需要注意的是分布式系统里多台机器间文件的last-modified必须保持一致，以免负载均衡到不同机器导致比对失败，Yahoo建议分布式系统尽量关闭掉Etag(每台机器生成的etag都会不一样，因为除了 last-modified、inode 也很难保持一致)
- Last-Modified/If-Modified-Since要配合Cache-Control使用，Etag/If-None-Match也要配合Cache-Control使用
- 尽量使用Cache-Control少用Etag，304的回包对SEO有影响
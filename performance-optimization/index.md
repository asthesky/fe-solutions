# 前端性能优化

## 雅虎yslow的14条优化原则:
1. 尽可能的减少 HTTP 的请求数    content
2. 使用 CDN（Content Delivery Network）    server
3. 添加 Expires 头(或者 Cache-control )    server
4. Gzip 组件    server
5. 将 CSS 样式放在页面的上方    css
6. 将脚本移动到底部（包括内联的）    javascript
7. 避免使用 CSS 中的 Expressions    css
8. 将 JavaScript 和 CSS 独立成外部文件    javascript css
9. 减少 DNS 查询    content
10. 压缩 JavaScript 和 CSS (包括内联的)    javascript css
11. 避免重定向    server
12. 移除重复的脚本    javascript
13. 配置实体标签（ETags）    css
14. 使 AJAX 缓存

优化方向 | 优化手段
:----|:----
请求数量 | 合并脚本和样式表，CSS Sprites，拆分初始化负载，划分主域，字体图标，雪碧图片等
请求带宽 | 开启服务器GZip，精简JavaScript，移除重复脚本，图像优化（包括图片大小kb）
缓存利用 | 使用CDN，使用外部JavaScript和CSS，添加Expires头，减少DNS查找，配置ETag，使AjaX可缓存
页面结构 | 将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出
代码校验 | 避免CSS表达式，避免重定向

## 具体方案
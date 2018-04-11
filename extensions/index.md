# 浏览器扩展

## Manifest文件
```js
{
  // 必须的字段
  "name": "My Extension",//用来标识扩展的简短纯文本。这个文字将出现在安装对话框，扩展管理界面，和store里面
  "version": "versionString",//扩展的版本用一个到4个数字来表示，中间用点隔开
  "manifest_version": 2,//用整数表示manifest文件自身格式的版本号,指定版本号为2

  // 建议提供的字段
  "description": "A plain text description", //描述扩展的一段字符串
  "icons": { ... },//一个或者多个图标来表示扩展，app，和皮肤
  "default_locale": "en", //指定这个扩展保的缺省字符串的子目录：_lcoales

  // 多选一，或者都不提供
  "browser_action": {...}, //工具栏图标，点击打开一个popup层或新tab页面 描述（default_title），图标（default_icon），弹窗（popup）
  "page_action": {...},//地址栏图标，点击打开新tab页面或内容注入
  "theme": {...}, //主题
  "app": {...}, //应用程序列表，点击打开内置tab页面或任意域名的新网页
  
  // 根据需要提供
  "background": {...},//可以使扩展常驻后台，比较常用的是指定子属性 scripts，表示在扩展启动时自动创建一个包含所有指定脚本的页面
  "chrome_url_overrides": {...}, //定义的页面替换 Chrome 相应默认的页面，比如新标签页（newtab）、书签页面（bookmarks）和历史记录（history）
  "content_scripts": [...],//运行在Web页面的上下文的JavaScript文件
  "content_security_policy": "policyString",//网页安全政策
  "file_browser_handlers": [...],//允许用户上传文件，只支持Chrome OS  
  "homepage_url": "http://path/to/homepage",//这个扩展的主页 url
  "incognito": "spanning" or "split",//指定当扩展在允许隐身模式下运行时如何响应
  "intents": {...}//用于描述扩展或app所提供的全部intent handler
  "key": "publicKey", //扩展指定的唯一标识值,这个值并不是开发时显式指定的，而是Chrome在安装.crx时辅助生成的
  "minimum_chrome_version": "versionString",//扩展，app或皮肤需要的chrome的最小版本，
  "nacl_modules": [...],//一个或多个从MIME到处理这个MIME的本地客户端模块之间的映射。
  "offline_enabled": true,//指定本扩展或app是否支持脱机运行
  "omnibox": { "keyword": "aString" }, //搜索关键词推荐
  "options_page": "aFile.html",//定义了扩展的设置页面，配置后在扩展图标点击右键可以看到 选项，点击即打开指定页面
  "permissions": [...],//扩展或app将使用的一组权限
  "requirements": {...},//指定本app或扩展所需的特殊技术功能
  "update_url": "http://path/to/updateInfo.xml", //插件更新地址
  "web_accessible_resources": [...] //一组字符串，指定本扩展在注入的目标页面上所需使用的资源的路径
}  
```





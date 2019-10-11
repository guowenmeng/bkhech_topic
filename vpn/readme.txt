PAC 文件的安装和注意事项

在 Windows 系统中，通过「Internet选项 -> 连接 -> 局域网设置 -> 使用自动配置脚本」可以找到配置处，下放的地址栏填写 PAC 文件的 URI，这个 URI 可以是本地资源路径(file:///)，也可以是网络资源路径(http://)。

Chrome 中可以在「chrome://settings/ -> 显示高级设置 -> 更改代理服务器设置」中找到 PAC 填写地址。

需要注意的几点：

PAC 文件被访问时，返回的文件类型（Content-Type）应该为：application/x-ns-proxy-autoconfig，当然，如果你不写，一般浏览器也能够自动辨别
FindProxyByUrl(url, host) 中的 host 在上述函数对比时无需转换成小写，对大小写不敏感
没必要对 dnsResolve(host) 的结果做缓存，DNS 在解析的时候会将结果缓存到系统中
‘

备注：

说白了，PAC就是动态配置代理服务器的东东，比手动配置要更灵活方便。

PAC的配置方法：

（1）Windows平台中系统配置

在控制面板 | Internet选项，选择连接选项卡，点击“”局域网设置“按钮，在弹出的对话框中勾选”使用自动配置脚本“，在地址文本框中输入PAC文件的URL(http://xxx)或本地路径位置(file://xxx)

配置之后，可以在IE浏览器或Chrome中都可以使用代理服务器了。

（2）使用Chrome的Proxy SwitchySharp或Proxy SwitchyOmega也可以方便的配置PAC，但是这将针对Chrome有效了。



使用说明地址：https://blog.csdn.net/wangjianno2/article/details/54667178

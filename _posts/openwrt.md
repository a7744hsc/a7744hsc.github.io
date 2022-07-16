newifi mini(y1) 通过openwrt实现全局fq。

https://medium.com/@cnnbysy/openwrt-18-06-1-%E9%85%8D%E7%BD%AE%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91-30e231958c38

1. 首先刷breed（非必须）：
（breed下载地址)[https://www.right.com.cn/forum/thread-161906-1-1.html],我使用的是breed-mt7620-lenovo-y1.bin
按住reset键开机（大约5秒，电源灯变成连续闪两次），通过网线连接电脑，电脑ip地址设置成192.168.1.2，然后通过浏览器访问192.168.1.1进入默认uboot界面，刷入breed bin。

2. 通过breed刷入openwrt（如果没有安装breed则使用uboot安装，方法类似）
(下载地址)[https://openwrt.org/toh/views/toh_fwdownload]我是用的是openwrt-18.06.2-ramips-mt7620-y1-squashfs-sysupgrade.bin

按住reset键开机（大约5秒，电源灯变成连续闪两次），通过网线连接电脑，电脑ip地址设置成192.168.1.2，然后通过浏览器访问192.168.1.1进入breed界面（uboot界面），按提示刷入固件。

3. 安装shadowsocks-libev, chinadns (其实还需要dnsmasq，不过openwrt自带)

```
opkg install shadowsocks-libev-config shadowsocks-libev-ss-local shadowsocks-libev-ss-redir shadowsocks-libev-ss-rules shadowsocks-libev-ss-tunnel luci-app-shadowsocks-libev
```



4. 要使用gfwlist则需要删除原版dnsmasq，安装完全版

opkg update && opkg remove dnsmasq && opkg install dnsmasq-full




最后用了pandavan，还是很不错的，能满足我的需求
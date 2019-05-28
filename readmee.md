- pimd是一个轻量级，独立的组播PIM-SM/SSM路由进程，可在3-clause BSD license协议下自由使用。pimd的原始版本是由南加州大学的Ahmed Helmy, Rusty Eddy and Pavlin Ivanov Radoslavov编写。

- 如今，pimd在GitHub维护。利用其基础设施来访问源代码，报告错误和功能需求，并且可以发送补丁和拉取请求。官方的压缩包在作者自己维护的网站，在GitHub上可以访问源代码。

- pimd在主流的Linux发行版上都能运行。其余的Unix版本应该也能工作，但是没有严格测试。详细的内同可以参考configure脚本。可通过Change log查看历史更改和版本差异。

- 在/etc/pimd.conf中存放的是配置信息，配置中描述的顺序是重要的。

- PIM-SM是被设计为协议无关的组播路由协议。因为它是在单播路由协议的基础上生成的，比如ospf,RIP,或者静态路由表项。这些单播转发信息在组播发送者和接收者之间指定实际的转发者是必须的。

- 然而，现阶段的pimd还不支持从系统中提取单播路由的距离和度量，也不能像zebra从内核或路由管理者那样提取。因此，pimd当前需要按照静态的步骤配置每一个活跃接口的距离和度量值。如果不配置，则会按照默认的default-route-distance:101(1-255),default-route-metric:1024(1-1024)。

- 默认情况下，pimd开启所有能找到的接口并用上面的默认值配置它们。如果要配置单个接口可使用：phyint <address | ifname>。

- 你可以通过其本地ipv4地址或者名字引用该接口，名字比如eth0。一些常用接口配置如下：
	- disable：关闭该接口的pimd功能，比如不在收发pim-sm报文
	- dr-priority <1-4294967294>:DR的优先级，包含在PIM的hello报文中发送。如果在局域网中都使用此选项，则在DR选举中代替IP地址（其实应该是先比较优先级然后再比较IP地址）。值越大优先级越高，默认值为１。
	- distance　<1-２５５>:接口的管理距离值，用在PIM的断言消息。
#### 本地安装方法：
- 1）下载解压好以后，我们用cmd命令行,进入到你的文件目录，
- 2）使用命令pip install . 注意了（install后面有个点）
- 3）然后就会安装了，等一会就可以了。

#### 下面讲解一下实现过程：

**1）**首先导入pywifi模块，因为要启用wifi那么必须要有启用wifi的模块。

**2）**有了启用wifi的模块以后，我们首先要抓取网卡接口，
因为连接无线wifi，必须要有网卡才行。一台电脑可能有很多网卡，
但是一般都只有一个wifi网卡，我们使用第一个网卡就行了。

**3）**抓取到以后就进行连接测试，首选是要断开所有的wifi网卡上
的已连接成功的，因为有可能wifi上有连接成功的在。

**4）**断开所有的wifi以后，我们就可以进行破解了，
从（.txt）文档中一行一行读取我们的密码字典，
一遍一遍的刷密码，直到返回isOK为True,表示破解成功。

**5）**因为连接也是要时间的，不可能一秒钟尝试好多次，
所以我们设置了睡眠sleep.

大家可能还有疑问，那就是test_connect这个方法中的代码，

1） profile.ssid ="e2"表示你要破解的wifi的ssid也就是wifi名称，
我手机开了热点，热点名字是e2所以我写了e2，
大家可以自己更该要破解的名称

2） profile.key就是要输入的密码

3） 别的代码差不多就是固定写法了，还有加密算法可以更改，

========================================================

pywifi provides a cross-platform Python module for manipulating wireless
interfaces.

* Easy to use
* Supports Windows and Linux

Now pywifi runs under python 2.7 & 3.5

Prerequisites
-------------

On Linux, you will need to run wpa_supplicant to manipulate the wifi devices,
and then pywifi can communicate with wpa_supplicant through socket.


On Windows, the `Native Wifi`_ component comes with Windows versions greater
than Windows XP SP2.

Installation
------------

After installing the prerequisites listed above for your platform, you can
use pip to install from source:

::

    cd pywifi/
    pip install .
    
Example
-------------

::

    import pywifi

    wifi = pywifi.PyWiFi()

    iface = wifi.interfaces()[0]

    iface.disconnect()
    time.sleep(1)
    assert iface.status() in\
        [const.IFACE_DISCONNECTED, const.IFACE_INACTIVE]

    profile = pywifi.Profile()
    profile.ssid = 'testap'
    profile.auth = const.AUTH_ALG_OPEN
    profile.akm.append(const.AKM_TYPE_WPA2PSK)
    profile.cipher = const.CIPHER_TYPE_CCMP
    profile.key = '12345678'

    iface.remove_all_network_profiles()
    tmp_profile = iface.add_network_profile(profile)

    iface.connect(tmp_profile)
    time.sleep(30)
    assert iface.status() == const.IFACE_CONNECTED

    iface.disconnect()
    time.sleep(1)
    assert iface.status() in\
        [const.IFACE_DISCONNECTED, const.IFACE_INACTIVE]

How to contribute/ToDo
----------------------

Following items may be done in the future:

* OS-X Support
* Rewrite Linux part: communicate with wpa_supplicant via 
  sockets instead of D-bus
* Documentation

\(C) Jiang Sheng-Jhih 2016, `MIT License`_.

.. _Native Wifi: https://msdn.microsoft.com/en-us/library/windows/desktop/ms706556.aspx
.. _MIT License: https://opensource.org/licenses/MIT

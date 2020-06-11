有时候需要更改一些射频相关的参数，在导入到代码中，需要用到QRCT软件。

```
Wifi的配置文件可以通过QRCT导入/导出，导入后会覆盖手机侧的如下文件/persist/WCNSS_qcom_wlan_nv.bin
Xml文件的格式，硬件射频的同事会改

一：导入/导出前需要如下动作：
1：adb shell
2：先把ptt_socket_app的进程kill掉
3：然后ptt_socket_app –f

二：通过QRCT导入/导出wifi配置文件
1：打开QRCT，连接上手机
2：FTM command-〉-> WLAN -> Load DUT-〉ftm start-〉push xml nv to ../pull device nv to ..
Load DUT 之前，chipset选择WCN36XX

三：编译
导入XML后，在通过adb pull /persist/WCNSS_qcom_wlan_nv.bin . 命令导出bin文件，然后覆盖源码再编译
覆盖位置
	vendor/qcom/opensource/wlan/prima/firmware_bin/WCNSS_qcom_wlan_nv.bin
	device/honeywell/eda50/WCNSS_qcom_wlan_nv.bin
```
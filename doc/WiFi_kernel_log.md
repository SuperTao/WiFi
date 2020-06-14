## 增加WiFi Kernel的日志等级

在WCNSS_qcom_cfg.ini文件中添加

``` 
vosTraceEnableSME=255
vosTraceEnableWDI=255
vosTraceEnableWDA=255
vosTraceEnablePE=255
vosTraceEnableHDD=255
vosTraceEnableSAP=255
vosTraceEnablePMC=255
vosTraceEnableSVC=255
vosTraceEnableTL=255
vosTraceEnableSYS=255
vosTraceEnableVOSS=255
```
在push到/data/misc/wifi/目录中即可。

## 打开WiFi kernel详细日志

Settings -> Develop options -> Enable Wi-Fi Verbose Logging

## 获取WiFi kernel日志文件

默认文件位于/sdcard/wlan_logs目录

## 更改WiFi kernel日志文件位置

vendor/qcom/proprietary/wlan/common-tools/cnssdiag/cld-fwlog-netlink.c

```
char log_capture_loc[MAX_SIZE] = "/sdcard/wlan_logs/";
```

## cnss_diag服务定义

```
service cnss_diag /system/vendor/bin/cnss_diag -q -f
    class main
    user system
    group system wifi inet net_admin sdcard_rw media_rw diag
    oneshot
```

## seLinux权限添加

* sepolicy/common/file_contexts

```
/vendor/bin/cnss_diag u:object_r:cnss_diag_exec:s0
```

* sepolicy/common/cnss_diag.te

```
type cnss_diag, domain;
type cnss_diag_exec, exec_type, vendor_file_type, file_type;

init_daemon_domain(cnss_diag)

net_domain(cnss_diag)
unix_socket_connect(cnss_diag, netd, netd);

allow cnss_diag cnss_diag:netlink_socket { create setopt bind getattr setattr write read connect };
allow cnss_diag self:capability { setuid setgid dac_override net_admin dac_read_search chown fsetid }; 
allow cnss_diag dnsproxyd_socket:sock_file { write };
allow cnss_diag storage_file:dir { search read write add_name create open setattr remove_name rename rmdir };
allow cnss_diag storage_file:lnk_file { read write open append getattr create unlink setattr };
allow cnss_diag mnt_user_file:dir { search read write add_name create open setattr remove_name rename rmdir };
allow cnss_diag mnt_user_file:lnk_file { read write open append getattr create unlink setattr };
allow cnss_diag sdcardfs:dir { search append write add_name create read open remove_name rmdir};
allow cnss_diag tmpfs:lnk_file { read getattr write setattr create unlink };
allow cnss_diag media_rw_data_file:dir { search create setattr getattr write read open append add_name remove_name rmdir rename };
allow cnss_diag sdcardfs:file { create write open getattr append read setattr unlink};
allow cnss_diag media_rw_data_file:file { read write open append getattr create unlink setattr rename };
allow cnss_diag media_rw_data_file:lnk_file { read write open append getattr create unlink setattr };
allow cnss_diag logdr_socket:sock_file write;
allow cnss_diag logd:unix_stream_socket connectto;
allow cnss_diag fuse:dir { search read write add_name create open setattr remove_name rename rmdir };
allow cnss_diag fuse:file { create read write append open getattr setattr unlink };
allow cnss_diag fuse:lnk_file { create read write append open getattr setattr };
allow cnss_diag tmpfs:chr_file { read write getattr };
allow cnss_diag diag_device:chr_file { read write open ioctl};
allow cnss_diag cnss_diag:netlink_generic_socket {bind create setopt write read connect setattr getattr};
allow cnss_diag wifi_data_file:dir { search append write read add_name open remove_name rmdir rename create};
allow cnss_diag wifi_data_file:file { create write open getattr append read setattr unlink};
```

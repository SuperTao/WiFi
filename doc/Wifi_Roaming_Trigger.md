1. Beacon RSSI is the main trigger:

当当前已连接的SSID的RSSI低于所设置的阀值（gNeighborLookupThreshold）, roaming module 将会触发漫游扫描。

触发漫游扫描后，当扫描到的AP大于所设置的DIFF DB（gRoamRescanRssiDiff）,漫游流程将被触发。

2. Beacon miss:

如果beacon 包已经丢失所设置的gRoamBmissFirstBcnt包数，将会触发漫游扫描；

如果在丢了gRoamBmissFinalBcnt的beacon包前还未找到候选者则会断开连接。

3. STA_KICOUT

Data path tracks transmission failures for each retry. If too many transmission attempts fail in a row 

[current setting is “t”(gDroppedPktDisconnectTh) consecutive TX failures], it generates STA_KICKOUT event. 

It is treated as a heartbeat failure and leads to disconnection. STA_KICKOUT event is held during a roaming scan. 

Upon roaming scan completion, either we send BETTER_AP or STA_KICKOUT event. 

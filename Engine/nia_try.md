List<UniTopologyEntity> uniList = alCurrentDtlRepository.findByLinkId("192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2");
        LoggerPrint.infoLog(uniList.toString());
```
[UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2),
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2),
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2),
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.210/SH1-S.1--192.168.200.213/SH1-S.2, sysnamea=192.168.200.210, shelfa=SH1, ptpnamea=S.11-L1, sysnamez=192.168.200.213, shelfz=SH1, ptpnamez=S.1-P2)] <<<<<<<<<<<<<<<<<
```

System.out.println((uniShelfaList));
```
[UniTopologyEntity(linkId=192.168.200.220/SH1-S.3--192.168.200.218/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.13-L1, sysnamez=192.168.200.218, shelfz=SH1, ptpnamez=S.6-L1), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.3--192.168.200.218/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.13-L1, sysnamez=192.168.200.218, shelfz=SH1, ptpnamez=S.6-L1), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.3--192.168.200.218/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.13-L1, sysnamez=192.168.200.218, shelfz=SH1, ptpnamez=S.6-L1), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.5--192.168.200.216/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.11-L1, sysnamez=192.168.200.216, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.5--192.168.200.216/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.11-L1, sysnamez=192.168.200.216, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.5--192.168.200.216/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.11-L1, sysnamez=192.168.200.216, shelfz=SH1, ptpnamez=S.1-P2), 
UniTopologyEntity(linkId=192.168.200.220/SH1-S.5--192.168.200.216/SH1-S.2, sysnamea=192.168.200.220, shelfa=SH2, ptpnamea=S.11-L1, sysnamez=192.168.200.216, shelfz=SH1, ptpnamez=S.1-P2)]
```
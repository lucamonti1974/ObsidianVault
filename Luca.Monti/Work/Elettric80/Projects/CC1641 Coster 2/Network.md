## Coster2

### VPN

|Client|Connection with browser|
|---|---|
|URL|https://89.31.50.253/wabam/costergroup|
|Username|Montil@coster.com|
|Pwd|NC^/[C<rt6c&X97|
|MFA|Notification on DUO App|

### Users

| Domain      | User   | PWD                  | Note                           |
| ----------- | ------ | -------------------- | ------------------------------ |
| costergroup | E80Adm | E8O-4dM1,428         | Admin user server Elettric80   |
| costergroup | E80Srv | E8O?s3rV!91          | Service user server Elettric80 |
|             | sa     | StandardE80          | sa user SQL SERVER             |
| costergoup  | E80bck | BDX4t*<N,.!=h4/8#çeò | c2-3100 (backup server ) user  |

### IP Addresses

|Name|IP|Ruolo|
|---|---|---|
|C2-HWV01|10.4.1.180|SQL Server wms service, lgv manager ,ts,RA smart, kepware, ppt|
|C2-HWV02|10.4.1.181|SQL Server wms service|
|C2-HWV03|10.4.1.182|lgv manager ,ts, smart, kepware , - Other E80 services , ppt, ra|
|C2-E80CLST|10.4.1.183|Cluster|
|C2-E80AGSDM|10.4.1.184|Always on SDM|
|C2-E80AGLGVM|10.4.1.185|Always on LGV Manager|
|C2-E80AGTS|10.4.1.167|Always on Transport System|
|C2-E80AGPPT|10.4.1.168|Always on PPT|
|C2-E80SVCSDMT|10.4.1.186|Service SDM|
|C2-E80SVCMNGRT|10.4.1.187|Service LGV Manager + TS + PPT + RA|
|C2-E80SVCSMART|10.4.1.189|Service Smart - Smartevents - Other E80 services|
|c2-smbdb0|10.4.1.190|Server SQL scambio messaggi con MES|
|c2-3100|172.18.4.73\E80sqlbck|Backups server|

### Database

|Server|IP|Utente|Password|DB|Always ON|
|---|---|---|---|---|---|
|C2-E80AGSDM|10.4.1.184|sa|StandardE80|CC1641_Coster2_SDM|YES|
|C2-E80AGLGVM|10.4.1.185|sa|StandardE80|CC1641_Coster2_LGV|YES|
|C2-E80AGTS|10.4.1.167|sa|StandardE80|CC1641_Coster2_LGV|YES|
|C2-E80AGPPT|10.4.1.168|sa|StandardE80|PalletPatternTool_CC1641|YES|
|c2-smbdb0|10.4.1.190|E80CB02|E80$2021|CSTGTWCB02|NO|
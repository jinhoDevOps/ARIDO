이 PowerShell 명령어는 외부 IP 주소를 조회한 다음 네트워크 어댑터의 MAC 주소와 상세 정보를 표시하고,  일시 정지합니다.
```powershell
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -Command "& {Invoke-WebRequest -Uri 'http://ifconfig.me' -UseBasicParsing | Select-Object -ExpandProperty Content; getmac /v; pause}"
```

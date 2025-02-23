# ETC

Memory Bandwidth는 Memory Clock X Memory Bus로 결정된다.

> Reference   
> [blog - 메모리 반도체 대역폭 (클럭 / 버스 / 입출력 라인 / 노스브릿지 / FSB / GDDR / 듀얼 채널)](https://blog.naver.com/shakey7/221435517430)   


Memory에 접근하는데 소요되는 시간을 계산하려면, 먼저 Memory에 접근하는데 몇 clock이 소요되는 지 파악한다. 다음으로 Memory Clock 성능을 파악하여 특정 Memory에 접근하는데 몇초가걸리는지 알 수 있다.

예를 들어, Memory 접근하는데 10 clock이 필요하고, Memory Clock이 2GHZ면 Memory clock당 0.5ns가 필요함으로 memory 접근에는 총 5ns가 필요하다.

## File Encoding 알아보기
git bash 에서 file * 를 입력하면 해당 경로에 있는 파일들의 Encoding 을 출력해준다.

> Refernece
> [vhxpffltm.tistory - [Encoding] Windows 파일 인코딩 확인](https://vhxpffltm.tistory.com/243)

## File Encoding UTF8 로 일괄 변경하기
PowserShell 에서 다음 명령어를 입력한다.

```
# 현재 폴더와 하위 폴더에 있는 *.cpp, *.h 파일을 찾습니다.
Get-ChildItem -Recurse -Include *.cpp, *.h | ForEach-Object {
    $file = $_.FullName
    # 파일 전체 내용을 읽어옵니다.
    $content = Get-Content -Raw -Encoding Default $file
    # UTF-8 으로 다시 저장합니다.
    Set-Content -Path $file -Value $content -Encoding UTF8
    Write-Host "Converted: $file"
}
```

참고로, Windows PowerShell 로 할 경우  UTF-8 (BOM 포함) 으로 Encoding 된다. version 7.* 이상부터 BOM 을 기본으로 포함하지 않으며 
`$PSVersionTable` 로 버전을 확인해보면 Windows PowerShell은 5.1, PowerShell은 7.5 인걸로 나온다.

## WinSCP
Windows에서 WinSCP를 사용하여 Ubuntu로 파일을 전송(또는 그 반대)하려면, 기본적으로 Ubuntu에서 SSH 서버가 동작하고 있어야 합니다. 다음 단계를 참고해보세요.

1. **SSH 서버 설치**  
   Ubuntu 터미널에서 다음 명령을 입력해 OpenSSH 서버를 설치합니다.
   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```
   
2. **SSH 서버 상태 확인 및 서비스 활성화**  
   - 설치가 끝나면 SSH 서비스가 자동으로 실행됩니다.  
   - 만약 실행 중인지 확인하고 싶다면:
     ```bash
     systemctl status ssh
     ```
   - 혹시 비활성화되어 있다면 다음 명령으로 활성화해 줍니다.
     ```bash
     sudo systemctl enable ssh
     sudo systemctl start ssh
     ```
     
3. **Ubuntu IP 주소 확인**  
   - WinSCP에서 접속하기 위해서는 Ubuntu의 IP 주소가 필요합니다.  
   - Ubuntu 터미널에서 IP 주소를 확인하려면:
     ```bash
     ip addr
     ```
   - 무선(Wi-Fi) 또는 유선(Ethernet)을 사용한다면 `wlan0` 혹은 `eth0` 인터페이스 부분에 `inet` 항목으로 표시된 IP 주소(예: `192.168.0.xxx`)를 확인합니다.

4. **방화벽 설정 (필요한 경우)**  
   - Ubuntu가 UFW 방화벽을 사용 중이고 SSH 포트(기본 22번)가 막혀있다면, 다음 명령으로 열어줍니다.
     ```bash
     sudo ufw allow ssh
     ```
   - UFW가 비활성화 상태라면 신경 쓰지 않아도 됩니다.  

5. **IP 확인하기**
* ip addr
* hostname -I
* https://dongle94.github.io/ubuntu/ubuntu-check-ip/

6. **Ubuntu 버전 확인**
* lsb_release -a
* https://blog.naver.com/sw4r/221868948844* 

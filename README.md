# LowLatencyRaspCamWirelessStreaming
Low Latency Rasp Cam Wireless Streaming for wheelchair rider

 
휠체어용 후방 카메라 모듈 매뉴얼
1	Raspberry PI OS 설치	2
1.1	Raspberry PI Imager 설치	2
1.2	microSD 카드 포멧하기	3
1.3	Raspbian OS 설치하기	7
2	Raspberry PI 프로그램 설치	12
2.1	SSH 켜기 및 카메라 활성화	12
2.2	필수 모듈 프로그램 설치	13
2.3	RemoteServer 설치	13

 
1	Raspberry PI OS 설치
1.1	Raspberry PI Imager 설치
1.1.1	Windows환경에서 microSD카드에 Raspbian OS를 설치 할 수 있게 도와주는 프로그램인 Raspberry PI Imager를 설치하여야 합니다.
1.1.2	해당 소프트웨어는 Raspberry PI 공식사이트에서 다운로드 할 수 있습니다.
1.1.3	https://www.raspberrypi.org/software/
1.1.4	 
1.1.5	다운로드하여 설치하면 아래와 같은 프로그램이 열립니다.
1.1.6	 
 
1.2	microSD 카드 포멧하기
1.2.1	설치한 Imager 프로그램에서 Operating System 메뉴를 클릭하고 설정한다.
1.2.2	 
1.2.3	 
1.2.4	Storage 선택 버튼을 누르고 마운트한 microSD카드를 선택합니다.
1.2.5	 
1.2.6	 
1.2.7	Write 버튼을 눌러 SD카드를 포멧 합니다. 아래 이미지 순서에 따라 진행해 주세요
1.2.8	 
1.2.9	 
1.2.10	 
1.2.11	아래의 창이 표시가 되며 포멧이 완료됩니다.
1.2.12	 
 
1.3	Raspbian OS 설치하기
1.3.1	 Operating System 버튼을 클릭하고 Raspbian OS(32-Bit)를 선택하세요
1.3.2	 
1.3.3	 
1.3.4	 
1.3.5	Storage 버튼을 눌러 방금 포멧 완료한 microSD카드를 선택하세요
1.3.6	 
1.3.7	 
1.3.8	Write 버튼을 눌러 Raspbian OS를 설치하세요
1.3.9	 
1.3.10	 
1.3.11	 
1.3.12	아래와 같이 표시되면 OS설치가 모두 완료되었습니다.
1.3.13	 
1.3.14	microSD카드 마운트를 해제하고 라즈베리파이 기기에 마운트하고 전원을 켜면 라즈베리파이가 정상 실행됩니다.
 
2	Raspberry PI 프로그램 설치
2.1	SSH 켜기 및 카메라 활성화
2.1.1	최초 설치 시 계정은 아이디 : pi, 비밀번호 : raspberry 입니다.
2.1.2	Raspbery PI OS가 로딩되면 sudo raspi-config 명령어를 입력합니다.
2.1.3	파란색 설정창이 열리면 3. Interface Options 을 선택하고 들어갑니다.
2.1.4	P1 Camera 를 선택하고 Yes로 설정합니다.
2.1.5	P2 SSH 를 선택하고 Yes로 설정합니다.
2.1.6	설정창을 종료하고 자동 재 부팅합니다.
 
2.2	필수 모듈 프로그램 설치
2.2.1	apt-get 패키지를 최신버전으로 변경합니다.
2.2.1.1	sudo apt-get update 
2.2.1.2	sudo apt-get upgrade 
2.2.1.3	reboot 
2.2.2	OpenCV 설치
2.2.2.1	sudo apt install libopencv-dev
2.2.3	WiringPi 설치
2.2.3.1	sudo apt-get install wiringpi
2.3	RemoteServer 설치
2.3.1	라즈베리파이 시작 디렉토리 이동
2.3.1.1	cd /home/pi
2.3.2	다운로드
2.3.2.1	sudo wget -O RemoteServer.out http://sose.co.kr/wget/RemoteServer.out
*** 임시로 SOSE서버에 보관중입니다. 
배포할 서버에 해당 파일을 업로드 해놓으시면 됩니다.
2.3.2.2	sudo chmod 777 ./RemoteServer.out
*** Permission을 꼭 바꿔 주셔야 합니다.
2.3.3	실행하기
2.3.3.1	sudo ./RemoteServer.out
*** 정상적으로 서버가 실행이 되었다면 Android App에서 접속 가능해 집니다.

 
3	Remote Server 자동실행 설정
3.1	프로그램을 자동실행하기 위해서 crontab을 활용합니다.
3.2	크론탭 편집창을 오픈합니다.
3.2.1	sudo crontab -e
3.2.2	편집기를 선택하라 뜨면 원하는 편집기를 선택하면 됩니다.
3.2.3	마지막줄에 다음과 같이 추가합니다.
3.2.4	@reboot sleep 10 && /home/pi/RemoteServer.out
3.3	자동실행 확인을 위해 재부팅을 합니다.
3.3.1	sudo reboot
 
4	Raspberry PI AP모드 설정
4.1	모듈 프로그램 설치 (명령어 순차입력)
4.1.1	sudo apt install -y hostapd dnsmasq
4.1.2	sudo systemctl unmask hostapd
4.1.3	sudo systemctl enable hostapd
4.1.4	sudo DEBIAN_FRONTEND=noninteractive apt install -y netfilter-persistent iptables-persistent
*** hostapd : AP모드 설정을 해주는 패키지
*** dnsmasq : DNS, DHCP를 관리해주는 패키지
4.2	AP IP 설정
4.2.1	아래의 명령어를 입력하여 편집을 시작한다.
sudo nano /etc/dhcpcd.conf
4.2.2	마지막줄에 다음 내용을 추가한다
interface wlan0
          static ip_address=192.168.4.1/24
    nohook wpa_supplicant

 
4.3	DHCP 및 DNS 설정
4.3.1	디폴트 설정 백업을 우선 진행 한다.
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
4.3.2	다음 명령어를 입력하여 설정 편집을 시작한다.
sudo nano /etc/dnsmasq.conf
4.3.3	에디터가 열리면 다음과 같이 입력하고 저장한다.
interface=wlan0 
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
domain=wlan 
address=/gw.wlan/192.168.4.1
 
4.4	AP 국가 설정
4.4.1	다음 명령어를 입력하여 무선랜 차단을 해제한다.
sudo rfkill unblock wlan
4.4.2	Wifi모듈 대역폭을 알맞게 설정하기 위해서 다음 명령어를 입력하여 설정편집을 시작한다.
sudo nano /etc/hostapd/hostapd.conf
4.4.3	에디터가 열리면 다음과 같이 입력하고 저장한다\
country_code=KR
interface=wlan0
ssid=sose
hw_mode=a
channel=36
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=qw1as2zx3!@#
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
4.5	라즈베리 파이 재부팅
4.5.1	다음 명령어를 입력하여 재부팅
sudo systemctl reboot


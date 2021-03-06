# macOS-Starter
macOS에서 웹 개발에 필요한 환경 잡기 및 지식 정리

## Terminal 
### iTerm2 
> 설치: `brew install iterm2`  
> 
> Color Schemes 설치: `git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git`
> Color Schemes 설정: iterm2 앱을 연 후 Preference (단축키: `cmd + ,`) -> Profiles -> Colors -> 우측 하단의 DropBox `Color Presets` 클릭 -> import... 
> -> 위에서 git clone 한 경로 이동 -> schemes 디렉터리로 이동 -> 디렉터리 안에 있는 파일 모두 선택 후 Open 버튼 클릭 
> -> 다시 우측 하단의 DropBox `Color Presets` 클릭 -> `Adventure` 선택
>
> 참조사이트: [Oh My ZSH+ iTerm2로 터미널을 더 강력하게](https://medium.com/harrythegreat/oh-my-zsh-iterm2%EB%A1%9C-%ED%84%B0%EB%AF%B8%EB%84%90%EC%9D%84-%EB%8D%94-%EA%B0%95%EB%A0%A5%ED%95%98%EA%B2%8C-a105f2c01bec)

### zsh 플러그인
> zsh-syntax-highlighting : zsh 문법 표시
> 설치 
> ```shell
> cd ~/.oh-my-zsh/plugins 
> git clone https://github.com/zsh-users/zsh-syntax-highlighting.git 
> echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
> ```  
> .zshrc 수정 : `vi ~/.zshrc` -> `plugins=(git zsh-syntax-highlighting)`  
>  
> zsh-autosuggestions: shell 자동 완성
> 설치 
> ```shell
> cd ~/.oh-my-zsh/plugins 
> git clone https://github.com/zsh-users/zsh-autosuggestions.git 
> echo "source ${(q-)PWD}/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
> ```  
> .zshrc 수정 : `vi ~/.zshrc` -> `plugins=(git zsh-syntax-highlighting zsh-git-prompt zsh-autosuggestions)`  

### Oh-My-Zsh
> 설치 : `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`  
> 테마 설정: `vi ~/.zshrc` -> 기존 ZSH_THEME="robbyrussell" 주석 후 -> `ZSH_THEME="agnoster"` 추가  
> 
> agnoster 테마 한글 깨짐 보정: 
> ```shell 
> git clone https://github.com/powerline/fonts.git
> cd fonts
> ./install.sh
> ```
> iterm2 앱을 연 후 Preference (단축키: `cmd + ,`) -> Profiles -> text 
> -> 좌측하단 Font 아래 DropBox 에서 `Meslo LG M DZ for Powerline` 선택, Regular, 14, 101, 100    
> Unicode normalization form 옆에 DropBox `NFC` 설정
> 
> 참조사이트: [Oh-My-Zsh github](https://github.com/robbyrussell/oh-my-zsh)
  
### New line 적용
> `agnoster` 테마 적용시 경로를 표시하는 줄 때문에 명령어가 화면 밖으로 나갈 수 있다. 명령어가 화면 바깥으로 나가지 않고 
> 다음 줄에 표시되도록 New line 을 적용한다.  
> 테마 파일 수정: `vi ~/.oh-my-zsh/themes/agnoster.zsh-theme`
> ```text
> ...
> prompt_newline() {
>   if [[ -n $CURRENT_BG ]]; then
>     echo -n "%{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR 
>     %{%k%F{blue}%}$SEGMENT_SEPARATOR"
>   else
>     echo -n "%{%k%}"
>   fi
> 
>   echo -n "%{%f%}"
>   CURRENT_BG=''
> }
>
> build_prompt() {
>   RETVAL=$?
>   prompt_status
>   prompt_virtualenv
>   prompt_context
>   prompt_dir
>   prompt_git
>   prompt_bzr
>   prompt_hg
>   prompt_newline //이부분을 추가 꼭 순서 지켜서
>   prompt_end
> }
> ```

## SFTP 사용자 권한
### 특정 사용자가 특정 디렉터리 이상 접근 못하도록 권한 설정
> 특정 사용자 ID가 `viewUser` 로 가정  
> 해당 사용자의 루트 디렉터리를 `/Users/viewUser/Public`으로 가정  
> ssh_config 파일 수정 `sudo vi /etc/ssh/ssh_config`  
> 기존 `Subsystem      sftp    /usr/libexec/sftp-server` 를 주석처리  
> 신규 추가: `Subsystem sftp internal-sftp`  
> `Match User` 문장도 추가  
> ```
> #Subsystem      sftp    /usr/libexec/sftp-server
> Subsystem sftp internal-sftp
>
> # Example of overriding settings on a per-user basis
> #Match User anoncvs
> #       X11Forwarding no
> #       AllowTcpForwarding no
> #       PermitTTY no
> #       ForceCommand cvs server
>
> Match User viewUser
>        ChrootDirectory /Users/viewUser/Public
>        ForceCommand internal-sftp
>        X11Forwarding no
>        AllowTCPForwarding no
>
> ```
> 참조사이트: [SFTP(Chroot 완벽설정) 사용자 상위폴더 접근 못하게 하는 두가지 방법](https://studyforus.tistory.com/243)

## Safari Extension 
### Mac AppStore for CLI
Safari 확장 프로그램이 mac appstore로 편입되었다. mac appstore를 cli로 사용하기 위해서 mas를 설치한다.  
mas 설치: `brew install mas`  
mac appstore 로그인: `mas signin`  
mac appstore 로그아웃: `mas signout`  
mac appstore 설치 프로그램 목록 보기: `mas list`  
프로그램 찾기: `mas search 찾을 프로그램명`  
프로그램 설치: `mas install 프로그램명`  
프로그램 제거: `mas uninstall 프로그램명`  

## diskutil
### 목록/추출/삽입
* 연결된 디스크 목록 보기 - `diskutil list`  
결과 예시 : 
```bash
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                 Apple_APFS Container disk1         984.2 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +984.2 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD - 데이터   216.6 GB   disk1s1
   2:                APFS Volume Preboot                 90.1 MB    disk1s2
   3:                APFS Volume Recovery                528.5 MB   disk1s3
   4:                APFS Volume VM                      3.2 GB     disk1s4
   5:                APFS Volume Macintosh HD            11.0 GB    disk1s5

/dev/disk3 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk3
   1:                        EFI EFI                     209.7 MB   disk3s1
   2:                  Apple_HFS WD Elements             999.8 GB   disk3s2
```

* 특정 디스크 추출 : /dev/disk 옆에 external(외부)로 되어 있는 디스크만 추출 가능 - `diskutil unmount [IDENTIFIER]`   
예시 : `diskutil unmount disk3s2`

* 특정 디스크 재삽입 : `diskutil unmount [IDENTIFIER]`  
예시 : `diskutil mount disk3s2`

### 디스크 포맷
* 포맷 기본 - `diskutil eraseDisk [포맷] [새로 설정할 디스크 이름] [설계] [IDENTIFIER]`
* EFI 파티션 없이 디스크 포맷하기 - `diskutil eraseDisk JHFS+ [새로 설정할 디스크 이름] MBR [IDENTIFIER]`   
예시) `diskutil eraseDisk JHFS+ MYDISK MBR disk3`
* 참고사이트 : [Removing EFI partition from external Disk?](https://apple.stackexchange.com/questions/327283/removing-efi-partition-from-external-disk)

## macOS 시스템 설정
### 우편번호 검색 서비스
* 참고사이트 : [링크](https://macnews.tistory.com/4716)

### macOS backquote 설정하기 ( ₩ -> ` ) 
```bash
$ if ! [ -f ~/Library/KeyBindings/DefaultkeyBinding.dict ]; then mkdir -p ~/Library/KeyBindings && echo '{"₩" = ("insertText:", "\`");}' > ~/Library/KeyBindings/DefaultkeyBinding.dict; fi
```
* 맥 재시동
* 참조사이트 : [링크](https://gist.github.com/redism/43bc51cab62269fa97a220a7bb5e1103?fbclid=IwAR3LVQc21h0aSxZGCCFMoEvdxe15aywjmkXRVf7ECIkcWUttuPGT0J2oE_I)

### RNDIS 활성화
* SIP 비활성화
    - 맥 재부팅 시에 cmd + R 을 통한 복구모드 진입 -> 상위 메뉴 -> 유틸리티 -> 터미널
    - `csrutil disable` 후 재부팅
    - 일반 부팅 후에 터미널에서 `csrutil status`로 disable 됬는지 확인
* 맥용 rndis driver 설치 : `brew cask install horndis`
* kext 파일 로드 : `sudo kextload /Library/Extensions/HoRNDIS.kext`
* 시스템 환경설정 -> 네트워크에서 RNDIS 기기가 연결된 것으로 보이면 성공 안보이면 HoRNDIS.kext 파일 리로드
    - `sudo kextunload /Library/Extensions/HoRNDIS.kext`
    - `sudo kextload /Library/Extensions/HoRNDIS.kext`
* 참고 사이트 
    - [horndis 패키지 다운로드 사이트](https://joshuawise.com/horndis#mavericks)

### 시스템 정보 확인
* Hyper-V 지원 확인 - `sysctl kern.hv_support`
* CPU 확인 - `sysctl -n machdep.cpu.brand_string`
* GPU 확인 - `system_profiler SPDisplaysDataType`

### 키보드 프로그램간 자동 한영 전환 옵션
> 시스템 환경 설정 -> 키보드 -> 입력 소스 -> `문서의 입력 소스로 자동으로 전환` 에 체크박스

### Spotlight 키 변경
* 시스템 환경설정 -> 키보드 -> 단축키 -> Spotlight -> cmd + shift + space

### 키보드 지연시간 변경
* 시스템 환경설정 -> 키보드 -> 키 반복 빠르게(MAX) / 반복 지연시간 짧게 MAX에서 한칸 아래
* 좋은점 : 백스페이스 잘 들어서 빨리 지울 수 있음.

## Catalina VMWare Fusion 10 Black Screen 해결
### SIP 해제
* 재부팅시 cmd + R로 Recovery 모드 진입
* 상단 유틸리티 -> 터미널
* `csrutil disable` 후 재시작
* 재시작 후 터미널에서 `csrutil status`로 disable 됬는지 확인

### 명령어 실행
* 터미널에서 다음의 명령어들 모두 실행   
* `tccutil reset All com.vmware.fusion`  
* `sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'insert into access values ("kTCCServiceScreenCapture", "com.vmware.fusion", 0, 1, 1, "", "", "", "UNUSED", "", 0,1565595574)'`  
* `sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'insert into access values ("kTCCServiceListenEvent", "com.vmware.fusion", 0, 1, 1, "", "", "", "UNUSED", "", 0,1565595574)'`  
* `sudo sqlite3 "/Library/Application Support/com.apple.TCC/TCC.db" 'insert into access values ("kTCCServicePostEvent", "com.vmware.fusion", 0, 1, 1, "", "", "", "UNUSED", "", 0,1565595574)'`  

### VMWare Fusion 재실행
* 끝
* 참고사이트 : [링크](https://communities.vmware.com/message/2879755#2879755)

## 문제 해결
### Git
* macOS - OS 업데이트 후 git 등 오류 발생 시 솔루션 : `xcode-select --install`
* 참고자료 : [링크](http://redutan.github.io/2015/10/05/osx-after-update-git-error)

### VPN 사용 
* [Sierra IPTime PPTP VPN](https://geekcoders.tistory.com/entry/Mac-OS-X-Sierra-IPTime-PPTP-VPN)    

### Yosemite Optical disc 없이 Bootcamp 만들기
* 1. BootCamp Assistant 우클릭 -> 컨텐츠 보기 -> info.plist 에서 `PreUSBBootModels`를 `USBBootModels`로 변경  
-> 터미널에서 `sudo codesign -fs - /Applications/Utilities/Boot\ Camp\ Assistant.app` 후 BootCamp Assistant 실행 
* 참조 : [링크](https://www.hongkiat.com/blog/install-bootcamp-without-optical-drive/)

## macOS에서 유용한 단축키 정리
### Finder
설명       | 단축키
--------- | ---------
command + shift + A | 응용프로그램 폴더 열기
command + shift + D | 데스크탑 폴더 열기
command + shift + O | 도큐먼트 폴더 열기
command + option + 전원 버튼 | Mac을 잠자기 상태로 전환 
shift + control + 전원 버튼 | 디스플레이를 잠자기 상태로 전환
(터미널에서) command + shift + K | 원격연결 저장 및 연결
command + \ | 1password 찾기 

## Homebrew
* 설명 : macOS 전용 패키지 관리자
* 공식 페이지 : [링크](https://brew.sh/index_ko)
* Homebrew는 CLI 패키지 및 GUI 패키지를 설치할 수 있다.
    - CLI 패키지 관리자 : <b>brew</b>
    - GUI 패키지 관리자 : <b>brew cask</b>
* 참조페이지 
    - [링크](https://m.blog.naver.com/PostView.nhn?blogId=sarang2594&logNo=221246170677&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
    - brew 패키지 관리하기 : [링크](https://rkjun.wordpress.com/2013/07/14/os-x-missing-package-manager-home-brew/)

### Homebrew 설치
* <b>터미널(Terminal)</b> 에서 작업
* 설치 명령어 : `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
* macOS Catalina 이후 버전 사용으로 인한 zsh 사용시에 brew를 통해 설치된 패키지 명령 사용을 위한 path 추가 : `echo 'export PATH="/usr/local/sbin:$PATH"' >> ~/.zshrc`

### brew(CLI 패키지 관리) 사용법
* 패키지(응용프로그램) 찾기 : `brew search [찾을 패키지명]`
* 패키지 설치 : `brew install [패키지 명]`
* 패키지 삭제(둘 중 아무거나 선택)
    - `brew remove [패키지명]`
    - `brew uninstall [패키지명]`
* 설치된 패키지 목록 보기 : `brew list`
* 패키지 저장소 추가 : `brew tap [저장소명]`

### brew cask(GUI 패키지 관리) 사용법
* 패키지(응용프로그램) 찾기 : `brew search [찾을 패키지명]`
* 패키지 설치 : `brew cask install [패키지명]`
* 패키지 삭제(둘 중 아무거나 선택)
    - `brew cask remove [패키지명]`
    - `brew cask uninstall [패키지명]`
* cask 저장소에서 패키지 찾기 : `brew cask audit [패키지명]`
* 패키지 전체 최신버전으로 업데이트 : `brew cask upgrade`

### OpenJDK 8
* 패키지 저장소 추가 : `brew tap AdoptOpenJDK/openjdk`
* opendjk 8 설치 : `brew cask install adoptopenjdk8`
* 참조 : [링크](https://github.com/AdoptOpenJDK/homebrew-openjdk)

### CMake
* 설치 : `brew install cmake`

### JetBrain Toolbox
* 설치 : `brew cask install jetbrains-toolbox`

### Visual Studio Code
* 설치 : `brew cask install visual-studio-code`

### Docker
* 설명 : 가상화 머신 
* 설치 : `brew cask install docker`

### ADB
* 설명 : android platform tools
* 설치 : `brew cask install android-platform-tools`
* 참조 : [링크](https://stackoverflow.com/questions/31374085/installing-adb-on-macos)

### netcat
* 설명 : 통신을 리스닝 서버를 netcat 명령어를 통해 간단하게 실행 가능
* 설치 : `brew install netcat`
* 사용법 
    1. 외부 서버에 특정 포트로 접속이 가능한지 확인 : `nc -z [외부 서버 주소] [포트]`
    2. Listening 서버를 로컬에 띄우기 : `nc -l [포트]`

### ngrok
* 설명 : 외부망에서 tcp 접속할 수 있도록 지정 포트를 ngrok에서 제공해주는 도메인 및 포트에 바인딩해준다.
* 주의 : 회원가입 필요(무료)
* 설치 : `brew install ngrok`
* 사용법 : `ngrok tcp [로컬 서버 프로세스의 포트]`

### Packet Sender
* 설치 : `brew cask install packetsender`

### Wireshark
* 설명 : 패킷 검사 툴
* 설치 : `brew cask install wireshark`

### tomcat
* 설치 : `brew install tomcat`
* [macOS Apache Tomcat 설치, 아파치 톰캣 실행 방법](https://whitepaek.tistory.com/12)

### mongoDB
> * 설명 : nosql database 중 하나
> * 패키지 저장소 추가 : `brew tap mongodb/brew`
> * Client 만 설치(Server 설치하면 Client 도 포함되어 있어서 Server 설치 시 필요없는 ): `brew install mongodb-community-shell`  
> * Server 설치 : `brew install mongodb-community@4.2`
> * mongoDB 서비스(background) 실행 : `brew services start mongodb-community@4.2`

### nosqlbooster for MongoDB 
* 설명 : mongoDB GUI 관리툴 
* 설치 : `brew cask install nosqlbooster-for-mongodb`
* 참고 사이트 : [Install MongoDB Community Edition on macOS](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)  

### MySQL
* 설명 : RDBMS(관계형 데이터베이스)
* 설치 : `brew cask install mysql`
* [macOS MySQL 설치 및 설정 사용법](https://whitepaek.tistory.com/16)
* [MySQL 설치 후 터미널에서 환경변수 설정하기](https://jason719.tistory.com/45)  
* [macOS 터미널에서 mysql 서버 켜기 / 끄기](https://macinjune.com/all-posts/web-developing/problem-solving/macos-%ED%84%B0%EB%AF%B8%EB%84%90-mysql-%EC%84%9C%EB%B2%84-%EC%BC%9C%EA%B8%B0-%EB%81%84%EA%B8%B0/)  
* [[MySQL] 사용자 관리(추가, 삭제, 권한 부여)](https://blog.opid.kr/237)
* [Mysql 사용자 추가, 제거 및 권한 부여](https://cjh5414.github.io/mysql-create-user/)  
* [mysql 적용 macOS에서 my.cnf 파일의 위치](https://code-examples.net/ko/q/a42431)  
* [OSX의 MySQL my.cnf 생성 및 UTF-8 기본값 설정](https://psage.tistory.com/entry/OSX%EC%9D%98-MySQL-mycnf-%EC%83%9D%EC%84%B1-%EB%B0%8F-UTF-8-%EA%B8%B0%EB%B3%B8%EA%B0%92-%EC%84%A4%EC%A0%95)  
* [[DB/MariaDB/Mysql] Mysql 언어설정으로 인한 문제, 그리고 해결](https://postitforhooney.tistory.com/entry/DBMariaDBMysql-Mysql-%EC%96%B8%EC%96%B4%EC%84%A4%EC%A0%95%EC%9C%BC%EB%A1%9C-%EC%9D%B8%ED%95%9C-%EB%AC%B8%EC%A0%9C-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%ED%95%B4%EA%B2%B0)  
* mysql mysql.sock 오류시 해결방법 : [macOS에서 homebrew로 mysql 을 깨끗하게 재설치](https://walkingfox.tistory.com/89)

### Git
* 설명 : 버전 관리
* 설치 : `brew install git`

### Github Desktop
* 설명 : 앱으로 Github 관리 
* 설치 : `brew cask install github`

### htop
* 설명 : top 명령어를 좀 더 시각적으로 나타냄
* 설치 : `brew install htop`

### Mackup
* 설명 : macOS 환경설정 백업
* 설치 : `brew install Mackup`
* 설정파일 생성 - `vi ~/.mackup.cfg`
```bash
### icloud 에 백업 생성
[storage]
engine = icloud
```
* 백업 실행 - `mackup backup`
* mackup이 백업하는 프로그램 리스트 - `mackup list`
* 참조사이트 : [맥 설치와 환경 설정을 최대한 자동화하기](https://nolboo.kim/blog/2015/05/07/mac-setup/)

### m-cli
* 설명 : 맥 시스템 제어
* `brew install m-cli`
* 참조사이트 : [맥을 터미널로 제어할 수 있는 세 가지 명령어 유틸리티 m, mac, mas](https://nolboo.kim/blog/2016/09/03/control-mac-with-terminal/)
* OS 시스템 정보 얻기 - `m info`
* 네트워크 인터페이스 보기 - `m network ls`
* 확인 없이 재시작 - `m restart -f`
* 확인 없이 종료 - `m shutdown -f`
* 스크린세이버 실행 - `m screensaver`
* 휴지통 비우기 - `m trash clean`
* 와이파이 비밀번호 - `m wifi showpassword`

### mac
* 설명 : 개발 관련 명령
* 실제 실행 파일이라기 보다는 명령어를 입력하면 다른 명령어로 변환해서 시스템에 입력시켜줌.
* 주의) 다른 명령어들을 모두 설치해야함으로 설치가 오래걸리고 설치하지 않을 경우 해당 명령은 실행 불가
* `sh -c "$(wget https://raw.githubusercontent.com/guarinogabriel/mac-cli/master/mac-cli/tools/install -O -)"`
* 사용중인 포트 보기 - `mac ports`
* 로컬 IP 주소 보기 - `mac ip:local`
* 퍼블릭 IP 주소 보기 - `mac ip:public`
* 인터넷 스피드 테스트 - `mac speedtest`
* 볼륨 모두 추출 - `mac eject-all`
* 기타 SSH 관련 - 
```text
SSH Utilities: 
mac ssh:download-file X: Download file from remote server through SSH - X = Path of the remote file to download 
mac ssh:download-folder X: Download entire folder from remote server through SSH - X = Path of the remote folder to download 
mac ssh:download-database X: Download database from remote server through SSH - X = Name of the database to download 
mac ssh:sync:local X: Sync local folder with remote folder using rsync through SSH (download remote folder to local folder)
mac ssh:sync:remote X: Path of the remote folder to sync from local folder (upload local folder to remote folder) 
mac ssh:upload X: Upload file to remote server through SSH - X = Path of the file to upload to the remote server 
mac ssh:public-key X: Get public SSH key for local machine 
```

### Chrome
* 설명 : Google의 인터넷 브라우저 <b>(업데이트 문제 때문에 브라우저로 직접 다운로드 받는걸 추천)</b>
* 설치 : `brew cask install google-chrome`

### TimeMachine Editor
* 설명 : 맥 자동백업 프로그램인 타임머신 설정 프로그램
* 설치 : `brew cask install timemachineeditor`

### Intel Power Gadget
* 설명 : Intel CPU 온도 및 주파수 조회
* 설치 : `brew cask install intel-power-gadget`

### Macs Fan Control
* 설명 : 맥에서 도는 팬의 회전수 / 온도 체크
* 설치 : `brew cask install macs-fan-control`

### Go2Shell
* 설명 : 파인더 상위 메뉴 버튼을 통해서 현재 파인더의 디렉터리 위치를 터미널로 열기
* 설치 : `brew cask install go2shell`

### App Cleaner
* 설명 : 프로그램 삭제툴
* 설치 : `brew cask install appcleaner`

### 1password
* 설치 : `brew cask install 1password`

### Team Viewer
* 설치 : `brew cask install teamviewer`

### Slack
* 설명 : 사내 메신져
* 설치 : `brew cask install slack`

### Mounty
* 설명 : NTFS 쓰기 for macOS
* 설치 : `brew cask install mounty`

### 에러 1
* 로그 : `/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems/core_ext/kernel_require.rb:54:in 'require': cannot load such file -- active_support/core_ext/object/blank (LoadError)`
* 대처 : `brew update-reset`

## 링크를 통한 다운로드가 필요한 패키지 항목
### eGPU
* 설명 : <b>Thunderbolt 3</b>를 지원하지 않는 맥에서 외장 그래픽(External GPU)를 연결할 수 있도록 해주는 스크립트
* 설치 : `curl -qLs $(curl -qLs https://bit.ly/2WtIESm | grep '"browser_download_url":' | cut -d'"' -f4) > purge-wrangler.sh; bash purge-wrangler.sh; rm purge-wrangler.sh`
* 공식 사이트 : [링크](https://github.com/mayankk2308/purge-wrangler)

### logitech options
* 설명 : logitech 키보드 및 마우스 설정
* 다운로드 : [링크](https://www.logitech.com/ko-kr/product/options)

### LG 모니터 소프트웨어
* 다운로드 : [링크](https://www.lgservice.co.kr/search/driverSoftTotalSearchPage.do?type=3&searchKeyWord=34WK95U)

### Folx
* 다운로드 : [링크](https://mac.eltima.com/ko/download-manager.html)

### beats update
* 다운로드 : [링크](https://www.beatsbydre.com/kr/support/how-to/firmware-updates-beats-updater)

### VMWare Fusion
* 다운로드 : [링크](https://www.vmware.com/kr/products/fusion/fusion-evaluation.html)

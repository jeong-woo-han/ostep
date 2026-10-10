# 목차

사지방에서 코딩할 수 있는 여러 방법을 소개합니다.

[1. Github VS Code 웹 뷰어와 웹 컴파일러 조합 이용](#1-github-vs-code-웹-뷰어와-웹-컴파일러-조합-이용) $\color{green}{\text{가능}}$

[2. 하모니카OS VS Code 이용](#2-하모니카os-vs-code-이용) $\color{red}{\text{불가능}}$

[3. Google Cloud Platform에서 무료로 서버 구축하여 VS Code 설치하고 원격 접속](#3-google-cloud-platform에서-서버-구축하여-vs-code-설치하고-원격-접속) $\color{green}{\text{가능}}$

$\quad$  [3.1. Google Cloud Platform(GCP)에서 서버 구축](#google-cloud-platformgcp에서-서버-구축)

$\quad$  [3.2. 서버에 VS Code 설치(code-server)](#서버에-vs-code-설치code-server)

$\quad$  [3.3. 향후 Git을 활용하기 위한 설치와 설정](#향후-git을-활용하기-위한-설치와-설정)

$\quad$  [3.4. 데스크탑 파일을 서버에 복사](#데스크탑-파일을-서버에-복사)



# 사지방에서 코딩하는 법

## 1. Github VS Code 웹 뷰어와 웹 컴파일러 조합 이용

1. github repository를 만듭니다(README.md 포함).
2. 키보드의 '.' 키를 누르면 VS Code 웹 뷰어 창이 열립니다. (끄는 법 `Ctrl + Shift + .` 또는 좌상단 메뉴 아이콘 클릭 > 리포지토리로 이동)
3. VS Code 웹 뷰어에서 코딩을 합니다.
4. VS Code 웹 뷰어는 실행 및 디버깅 불가하므로 OneCompiler(onecompiler.com) 등 웹 컴파일러에서 컴파일, 실행합니다.
5. 코딩 성공..!
6. 파일은 commit을 해야 클라우드에 올라갑니다. 사지방 컴퓨터는 재부팅 시 파일 삭제되므로 꼭 commit을 해야 합니다. commit은 좌측 메뉴 창의 세 번째 버튼(소스 제어)을 누르면 할 수 있으며, 기록을 위해 메모를 입력할 수 있습니다.

## 2. 하모니카OS VS Code 이용

하모니카OS는 리눅스 환경이지만 사지방에서는 terminal과 VS Code 실행이 불가능합니다.
   
## 3. Google Cloud Platform에서 서버 구축하여 VS Code 설치하고 원격 접속

### Google Cloud Platform(GCP)에서 서버 구축

* **서버 구축 비용: 0원**
* **사지방에서도 아래와 같이 8080 포트 적용 가능합니다.**
    
1. GCP에 구글 로그인하고, 프로젝트 생성 후 [Computer Engine] > [VM 인스턴스] 에서 인스턴스를 만듭니다.

2. 다음과 같은 조건으로 설정해야 합니다. 이 조건 모두 설정 시 $6.11/mo 등 작은 청구 비용이 표시됩니다(차후 할인되어 무료 사용 가능).
   * 리전: 오리건
   * 영역: `us-west1-a`
   * 머신 유형: `e2-micro` (2 vCPU, 1GB 메모리)
   * 운영체제: Ubuntu
   * 선택버전: Ubuntu 22.04 LTS (최신 버전은 불안정할 수 있습니다.)
   * 선택부팅 디스크 유형: 표준 영구 디스크
   * 선택크기(GB): `30`
   * 스냅샷 일정: 일정 없음 (스냅샷, 백업 설정 시 별도 디스크 비용 청구)
   * (추가) 차후 [결제]에서 예산 알림 설정으로 1원 설정하여 비용 발생 시 알리도록 함.
  
3. [VM 인스턴스]에서 생성한 인스턴스로 들어가 상단 [SSH] 누르면 검은 쉘 창이 뜹니다. 이 경우 서버 구축 성공입니다.

4. 쉘에서 기본적인 툴을 설치할 수 있습니다. Ubuntu의 최신 프로그램 목록을 새로고침하고, `gcc`(컴파일러)와 `gdb`(디버거)를 아래와 같이 한 번에 설치한 후 설치를 확인합니다.
 
   ```
   sudo apt update && sudo apt install build-essential gdb -y
   gcc --version
   ```

### 서버에 VS Code 설치(code-server)

1. `code-server`를 설치합니다.

   ```
   curl -fsSL https://code-server.dev | sh
   ```

2. 서버 밖에서도 접속할 수 있도록 설정값을 바꿉니다. 설정값은 `~/.config/code-server/config.yaml`에 있습니다. 따라서 텍스트 편집을 위해 `nano`를 설치하고, `nano`를 통해 편집합니다.

   ```
   sudo apt install nano -y
   nano ~/.config/code-server/config.yaml
   ```

   여기서 텍스트 내용을 아래와 같이 바꾸고 `Ctrl + O` -> `Ctrl + X`를 눌러 저장 후 `nano` 창을 나갑니다.

   `bind-addr: 127.0.0.1:8080` -> `bind-addr: 0.0.0.0:8080`
   `password: xxxxxx` -> 비밀번호 변경

3. 서버가 켜질 때마다 `code-server`가 켜지도록 설정합니다. 또한 백그라운드에서 실행되도록 설정했기 때문에 `SSH`에서 `Ctrl + C`로 `code-server`를 꺼도 VS Code가 항상 켜져있습니다.

   ```
   sudo systemctl enable --now code-server@$USER
   ```

4. GCP에서 8080 포트의 방화벽을 열어야 합니다. 메뉴의 [VPC 네트워크] > [방화벽] > [방화벽 규칙 만들기]로 들어가, 다음과 같이 설정하고 [만들기]를 클릭합니다.
   * 이름: `allow-code-server` (알아보기 쉬운 이름으로 입력)
   * 네트워크: `default`
   * 대응 방향: 인그레스 (들어오는 트래픽 허용)
   * 일치 시 작업: 허용
   * 대상: 클릭해서 [네트워크의 모든 인스턴스]를 선택합니다. (이 설정을 해야 내가 만든 서버에 방화벽이 바로 적용됩니다.)
   * 소스 필터: IPv4 범위
   * 선택소스 IPv4 범위: `0.0.0.0/0` (어디서든 내 서버의 이 포트로 들어올 수 있게 허용한다는 뜻입니다.)
   * 프로토콜 및 포트: 맨 아래에서 [지정된 프로토콜 및 포트] 체크박스를 켭니다.
   * `tcp` 체크박스를 체크하고, 옆의 입력창에 `8080`을 입력합니다.
   
5. GCP의 [VM 인스턴스]로 가면 리스트에서 해당 인스턴스의 외부 IP를 확인할 수 있는데, 외부에서 해당 서버로 접속할 때의 IP를 의미합니다. 따라서 내 컴퓨터의 웹 브라우저에 `X.X.X.X:8080` (`X.X.X.X`는 표시된 외부 IP)를 입력하여 접속합니다.

6. 로그인 창이 뜨면 비밀번호를 입력합니다. 정상적으로 `code-server`가 실행되었다면 VS Code 인터페이스가 뜹니다.

7. 이제 모든 준비가 되었으므로 `hello.c`와 같은 간단한 코드를 만들고 아래와 같이 VS Code 내 터미널 창에서 컴파일 후 실행해봅니다.

   ```
   gcc -o hello hello.c
   ./hello
   ```

8. 정상적으로 실행되었다면 다행이나, 만약 자꾸 꺼진다면 RAM 용량 부족이 원인일 가능성이 크므로 가상 메모리를 추가하는 아래 명령을 `SSH`에 입력해줍니다. 부족한 1GB 램 옆에 2GB짜리 강력한 가상 메모리를 붙여주고, 튕겨서 잠들어있던 웹 VS Code 엔진(`code-server`)을 다시 깨운 것입니다. 명령어 정상 실행 후에는 표가 뜹니다.
   
   ```
   sudo fallocate -l 2G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile && echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab && sudo systemctl restart code-server@$USER && free -h
   ```

9. 공식 VS Code와 달리 오픈소스 VS Code에는 Microsoft 공식 C/C++ Extension이 없음에 유의.. 어떤 걸 다운 받을지 개인적으로 연구 중입니다.

### 향후 Git을 활용하기 위한 설치와 설정

1. `git`을 설치합니다.

   ```
   sudo apt-get install git
   ```

2. 비밀번호를 입력하라는 메시지가 뜨면, `sudo passwd $USER`을 입력하여 비밀번호 설정한 뒤 설치를 재실행합니다.

3. 원하는 디렉토리로 들어가서(`cd 디렉토리`), 클론을 만듭니다. 이렇게 되면 서버의 현재 디렉토리는 내 git 레포지토리의 복사본이 됩니다.

   ```
   cd 디렉토리
   git clone 내 레포지토리 주소
   (예) git clone https://github.com/jeong-woo-han/ostep
   ```
   
4. 내 서버를 git에 알립니다. git 상의 작업이 commit될 때 내가 한 작업임이 표시됩니다.

   ```
   git config --global user.name "내 깃허브 이름"
   git config --global user.email "내 깃허브 이메일"
   ```

5. Github에서 토큰을 발급받습니다. 프로필 클릭 -> `Settings` -> `Developer settings` -> `Personal access tokens` -> `Tokens (classic)` -> `Generate new token (classic)` -> 체크박스 `repo` 체크(내 저장소 접근 권한) -> `Generate token` -> 토큰 생성 완료(주의: 페이지를 나가면 다시 볼 수 없으므로 복사해두세요.)

6. 향후 토큰 입력 후에는 서버가 저장해두도록 설정합니다. `--global` 옵션이 있으므로 서버의 사용자 전체에 적용됩니다.
   
   ```
   git config --global credential.helper store
   ```

7. 아래와 같이 hello.c를 만들고 내 레포지토리에 올리는 테스트를 해봅니다.

   ```
   git add hello.c
   git commit -m "첫 C프로그램 완성"
   git push origin main
   ```

8. `push`를 할 때 `Username`과 `Password`를 요구할 수 있는데, `Password`에는 토큰 복사해서 붙여넣으면 되겠습니다.

### 데스크탑 파일을 서버에 복사

1. `code-server`를 설치한 경우 VS Code의 디렉토리 창에 Drag-and-drop하거나, 우클릭 -> `Upload`를 클릭하여 파일 선택하면 업로드됩니다.

2. `SSH`에서 커맨드로 업로드하고자 하는 경우 아래와 같이 입력합니다.
   
```
scp -r "디렉토리" 사용자명@서버IP:/home/사용자명/디렉토리
(예) scp -r "C:\Users\user\Desktop\hello.c" handaniel11@0.0.0.0:/home/handaniel11/
```

### Git 저장소(로컬)와 레포지토리(GitHub) 간 연동

1. 아래 명령어가 자주 사용됩니다.

| 명령어 | 기능 |
| --- | :-- |
| `git clone URL` | GitHub 저장소를 로컬(서버)에 처음 복제 |
| `git status` | 변경된 파일 및 Git 상태 확인 |
| `git add .` | 현재 디렉터리의 변경 사항을 커밋 대상으로 등록 |
| `git commit -m "메시지"` | 로컬 Git 저장소에 변경 이력 기록 |
| `git push` | 로컬 커밋을 GitHub에 업로드 |
| `git pull` | GitHub의 변경 사항을 가져와 현재 브랜치에 반영 |
| `git log --oneline` | 커밋 기록을 한 줄씩 확인 |
| `git remote -v` | 연결된 원격 저장소 주소 확인 |

2. 로컬 -> 레포지토리: 변경 상태 확인 -> 커밋 대상에 추가 -> 커밋 -> 업로드

```bash
git status
git add .
git commit -m "Update code"
git push
```

3. GitHub 저장소 처음 가져오기

```bash
cd ~
git clone https://github.com/사용자명/저장소명.git
```

4. GitHub의 최신 코드 가져오기

```bash
cd ~/저장소명
git pull
```

**중요한 습관: 여러 컴퓨터에서 같은 프로젝트를 작업한다면 작업 시작 전에 git pull, 작업을 마친 뒤에는 git add → commit → push를 하는 게 좋습니다. 다만 수정 중인 파일이 있다면 pull 전에 먼저 git status로 확인하세요.**
# ☁️ AWS Cloud Infrastructure & Web Server 실습 정리

본 문서는 AWS VPC 환경 구축부터 EC2 인스턴스 생성, 그리고 Nginx 웹 서버 배포까지의 실습 과정을 이론과 함께 정리한 문서입니다.

---

## 1. VPC (Virtual Private Cloud) 🔒
**"나만의 가상 네트워크 공간 만들기"**
- **이론**: AWS 클라우드 내에 격리된 사설 네트워크를 구축하여 보안과 독립성을 확보합니다.
- **실습**: `10.0.0.0/16` 대역을 설정하여 VPC 울타리를 생성했습니다.



## 2. Subnet (서브넷) 🏘️
**"네트워크 구역 나누기"**
- **이론**: VPC 내부를 더 작은 단위로 나누어 자원을 효율적으로 배치하고 관리합니다.
- **실습**: `Subnet A`를 `10.0.1.0/24` 범위로 생성하여 웹 서버가 위치할 공간을 마련했습니다.

## 3. Internet Gateway (IGW) & Route Table 🚪
**"인터넷으로 나가는 관문과 이정표"**
- **이론**:
  - **IGW**: VPC 내부의 리소스가 외부 인터넷과 통신할 수 있게 하는 통로입니다.
  - **Route Table**: 데이터 패킷이 목적지를 찾아갈 수 있도록 길을 안내하는 표입니다.
- **실습**: `0.0.0.0/0`(모든 인터넷 경로)의 타겟을 방금 만든 IGW로 설정하여 외부 통신을 활성화했습니다.



## 4. EC2 (Elastic Compute Cloud) 🖥️
**"확장 가능한 가상 서버"**
- **이론**: 클라우드에서 실행되는 가상 머신으로, 필요에 따라 성능을 조절할 수 있습니다.
- **실습**: Amazon Linux 2023 OS를 사용하여 `Subnet A`에 인스턴스를 생성하고 키 페어(`.pem`)를 통해 접속했습니다.

## 5. Security Group (보안 그룹) 🛡️
**"인스턴스 단위 가상 방화벽"**
- **이론**: 서버로 들어오고(Inbound) 나가는(Outbound) 트래픽을 포트 번호 기반으로 제어합니다.
- **실습**:
  - **Port 22 (SSH)**: 내 PC에서 서버 제어를 위해 허용
  - **Port 80 (HTTP)**: 전 세계 사용자의 웹 접속을 위해 허용



## 6. Nginx Web Server 🌐
**"웹 서비스를 제공하는 소프트웨어"**
- **이론**: 클라이언트의 HTTP 요청을 받아 정적 콘텐츠를 제공하는 고성능 웹 서버입니다.
- **실습**: `dnf` 패키지 매니저로 설치 후 `systemctl`을 통해 활성화하여 "Welcome to nginx!" 페이지를 띄우는 데 성공했습니다.

<img width="1832" height="1790" alt="image" src="https://github.com/user-attachments/assets/2b810fd0-3936-48ab-abed-f2a5361e5b1d" />


---

## 💡 트러블슈팅 (Troubleshooting)

### ❓ SSH 접속 시 `Permission denied (publickey)`
- **원인**: 키 페어(`.pem`) 파일의 권한이 너무 개방되어 있어 보안상 거부됨.
- **해결**: 터미널에서 `chmod 400 hasungjun-key.pem` 명령어로 소유자만 읽을 수 있게 수정 후 해결.

### ❓ 접속 시 `Operation timed out`
- **원인**: 라우팅 테이블에 인터넷 게이트웨이(IGW) 경로가 없거나, 보안 그룹에서 22번/80번 포트가 닫혀 있음.
- **해결**: 라우팅 테이블에 `0.0.0.0/0 -> IGW` 규칙을 추가하고 보안 그룹 인바운드 규칙을 수정하여 해결.

### ❓ `Unit nginx.service could not be found`
- **원인**: OS 버전 차이로 인해 설치 명령어가 정상 실행되지 않음.
- **해결**: `yum` 대신 `dnf`를 사용하여 `sudo dnf install -y nginx`로 재설치하여 해결.

<img width="1786" height="1702" alt="image" src="https://github.com/user-attachments/assets/63fb088b-1967-45da-aa5e-a1fd1f7cd133" />

---
**Last Updated**: 2026-04-04
**Author**: hasungjun

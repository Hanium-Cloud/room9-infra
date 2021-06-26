# 쿠버네티스 클러스터 설치

## 1. vm 생성

AWS EC2 서버(마스터 노드 1개, 워커 노드 2개)

### 1-1. vm 서버 생성

총 3개를 생성합니다.

- EC2 > 인스턴스 시작
- 단계 1: Amazon Machine Image(AMI) 선택
  - `Ubuntu Server 20.04 LTS (HVM), SSD Volume Type` 선택
- 다음
- 단계 2: 인스턴스 유형 선택
  - 마스터 노드 `t3.large` 선택
  - 워커 노드 `t2.medium` 선택
- 다음 ...
- 단계 4: 스토리지 추가
   - 볼륨 유형 : 루트
   - 디바이스 : /dev/sda1
   - 크기 : 30GB
   - 볼륨 유형 : 범용 SSD(gp2)
- 다음 ...
- 단계 6: 보안 그룹 구성
   - 새 보안 그룹 생성
   - 보안 그룹 이름 : launch-wizard-1
   - 설명 : ssh, tcp, k8s, calico 등 필수 포트 허용(보안 그룹 설명 참조)
- 검토 및 시작
   - 새 키 페어 생성
   - 키 페어 이름 : `room9.pem`
   - 키 페어 다운로드
   - `인스턴스 시작` 버튼 클릭
   - 인스턴스 보기 버튼 클릭

## 2. 쿠버네티스 구축

- ssh로 인스턴스 접속
- 루트 권한으로 변경
- docker 설치
   - version : `20.10.2`
- vim 에디터 설치 (설정 파일 변경용)
- kubeadm kubelet kubectl 설치
   - version : `v1.21.1`
- kubectl 구성 (마스터 노드만)
- 토큰, 해쉬값 발급 (마스터 노드만)
- 마스터, 워커노드 조인
   - 명령어 : `kubeadm join \ --token {토큰값} \ {마스터노드DNS}:6443 \ --discovery-token-ca-cert-hash \ sha256:{해쉬값}`

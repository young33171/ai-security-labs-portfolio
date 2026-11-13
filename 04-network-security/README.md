# 04. 네트워크 보안 실습

PCAP 기반 패킷 캡처와 프로토콜 헤더 분석을 정리했습니다.

## 학습 내용

- 네트워크 인터페이스 선택과 캡처 시작
- Ethernet, IPv4, TCP/UDP 헤더 파싱
- 네트워크 바이트 순서 변환
- BPF 필터 적용
- Wireshark 결과와 직접 파싱 결과 비교

기존 공개 저장소의 PCAP 프로그래밍 실습은 아래에서 확인할 수 있습니다.

- [pcap-programming](https://github.com/young33171/pcap-programming)

## 보안 주의

PCAP에는 IP 주소, 세션 정보, 평문 데이터가 포함될 수 있으므로 실제 트래픽 캡처 파일은 공개하지 않습니다.


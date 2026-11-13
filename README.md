# AI & Cybersecurity Lab Portfolio

서울여자대학교 정보보호학과 전공 수업과 보안 교육에서 수행한 실습을 주제별로 정리한 저장소입니다.

> 모든 실습은 교육·방어 목적의 격리된 환경에서 수행했습니다. 개인정보, 인증정보, 실제 공격 대상 정보 및 수업 원문 자료는 포함하지 않습니다.

## 실습 구성

| 영역 | 핵심 실습 | 사용 기술 |
|---|---|---|
| [AI·머신러닝](./01-ai-ml/) | PyTorch 텐서, KNN 악성코드 분류, 피싱 탐지, K-Means, PCA | Python, PyTorch, scikit-learn, pandas |
| [시스템 보안](./02-system-security/) | SetUID, 쉘코드, 버퍼 오버플로 분석 | Linux, C, NASM, GDB |
| [리버스 엔지니어링](./03-reverse-engineering/) | PE 구조, DLL 인젝션, 바이너리 패치 | Windows, Visual Studio, PEView, HxD, Process Explorer |
| [네트워크 보안](./04-network-security/) | PCAP 패킷 캡처·분석 | C/C++, libpcap, Wireshark |

## 주요 결과

- 이미지 군집화에서 K-Means와 PCA를 적용하고 군집 중심·설명 분산을 시각화했습니다.
- KNN, 로지스틱 회귀, 결정트리를 이용해 악성코드·피싱 사이트 분류 과정을 비교했습니다.
- PyTorch 텐서 생성·인덱싱·구조 변경·연산을 실행 결과와 함께 정리했습니다.
- Linux/Windows 보안 실습은 명령어 암기보다 동작 원리, 검증 방법, 방어 관점을 중심으로 기록했습니다.

## 실행 환경

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

노트북은 Google Colab에서도 열 수 있습니다. 데이터셋은 각 노트북의 다운로드 셀 또는 해당 폴더 README를 참고하세요.

## 저장소 원칙

1. 공개 가능한 코드와 직접 작성한 설명만 게시합니다.
2. 수업 교재 PDF와 제3자 저작물은 저장하지 않습니다.
3. 공격 기법은 로컬 가상환경과 교육용 파일에서만 검증합니다.
4. 실습마다 목표 → 환경 → 절차 → 결과 → 배운 점 순서로 기록합니다.

## 학습 키워드

`AI Security` · `Machine Learning` · `Malware Classification` · `Phishing Detection` · `PCA` · `K-Means` · `System Security` · `Reverse Engineering` · `Network Security`


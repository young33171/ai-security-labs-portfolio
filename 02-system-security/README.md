# 02. 시스템 보안 실습

Linux 프로세스 권한과 메모리 취약점이 실제로 어떻게 동작하는지 격리된 가상환경에서 확인했습니다.

## 실습 목록

- [SetUID 권한 실습](./setuid-lab.md)
- [쉘코드 제작·실행](./shellcode-lab.md)
- [버퍼 오버플로와 GDB 분석](./buffer-overflow-lab.md)

## 공통 방어 관점

- 최소 권한 원칙과 불필요한 SetUID 제거
- 스택 실행 방지(NX), ASLR, Stack Canary, PIE 적용
- 경계 검사 함수 사용과 컴파일러 경고 활성화
- 입력 길이 검증 및 안전한 API 사용


# 버퍼 오버플로와 GDB 분석

## 목표

고정 크기 버퍼에 과도한 입력이 들어갈 때 스택 프레임과 반환 주소가 어떻게 영향을 받는지 이해합니다.

## 분석 순서

```bash
gcc -g -fno-stack-protector -z execstack vulnerable.c -o vulnerable
gdb ./vulnerable
```

GDB에서 중단점을 설정하고 레지스터, 스택 메모리, 함수 호출 전후 상태를 확인했습니다.

## 방어 방법

- `strcpy`, `gets`와 같은 위험 함수 사용 금지
- 길이 제한과 입력 검증
- Stack Canary, NX, ASLR, PIE 활성화
- 정적 분석과 퍼징을 통한 경계값 검사


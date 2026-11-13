# Linux x86 쉘코드 실습

## 흐름

1. NASM 어셈블리로 교육용 시스템 호출 코드를 작성합니다.
2. `nasm`으로 오브젝트를 만들고 `objdump`/`ndisasm`으로 바이트를 확인합니다.
3. 바이트열을 C 배열로 변환합니다.
4. 실행 가능한 스택 옵션을 적용한 교육용 프로그램에서만 동작을 확인합니다.

```bash
nasm -f elf32 sample.asm -o sample.o
objdump -d sample.o
ndisasm -b 32 sample.bin
gcc -m32 -z execstack -fno-stack-protector runner.c -o runner
```

## 확장 실습

`whoami`, 로컬 테스트 파일 읽기, 프로세스 목록 확인 등 안전한 명령으로 시스템 호출 흐름을 비교했습니다.

## 방어 관점

실무 환경에서는 실행 가능한 스택을 허용하지 않고 NX/DEP, ASLR, Canary를 활성화해야 합니다.


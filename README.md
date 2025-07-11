
# Crypto4: Cryptographic Analysis Library

`Crypto4`는 GF(2) 위의 효율적인 행렬 연산을 위해 M4RI 라이브러리를 활용하는, 재사용 가능한 C 기반 암호 분석 및 LFSR 스트림 암호 구현 라이브러리입니다.

---

## 📂 프로젝트 구조

```

crypto4/
├── include/               # 헤더 파일
│   ├── crypto\_lib.h
│   └── encrypt.h
├── source/                # 라이브러리 소스
│   ├── crypto\_lib.c
│   ├── encrypt.c
│   ├── debug\_init.c
│   └── verify\_r4\_clock\_pattern\_bin.c
├── tools/                 # 유틸리티 소스
│   ├── gen\_zS\_bin.c
│   ├── gen\_s\_gt\_bin.c
│   └── gen\_s\_gt\_bin\_no\_header.c
├── examples/              # 사용 예시 코드
├── data/                  # 데이터 파일 (Git 미추적)
├── bin/                   # 빌드 산출물
├── Makefile
└── README.md

````

---

## ⚙️ 개발 환경 설정

Windows + MSYS2(MinGW64) 환경에서 Makefile을 바로 실행하려면 다음 단계를 따라주세요:

1. **MSYS2 설치 및 업데이트**  
   - https://www.msys2.org/ 에서 인스톨러를 다운로드하여 설치  
   - 설치 후 “MSYS2 MSYS” 콘솔 열기 →  
     ```bash
     pacman -Syu
     ```  
   - 창을 닫고, “MSYS2 MinGW 64-bit” 콘솔 열기 →  
     ```bash
     pacman -Su
     ```

2. **필수 패키지 설치**  
   “MSYS2 MinGW 64-bit” 쉘에서:
   ```bash
   pacman -S --needed mingw-w64-x86_64-toolchain mingw-w64-x86_64-m4ri
````

* `toolchain`에는 `gcc`, `g++`, `make` 등이 포함됩니다.
* `m4ri`는 GF(2) 행렬 연산용 라이브러리입니다.

3. **환경 변수 등록**
   Windows 시스템(또는 사용자) 환경 변수에 아래를 추가:

   ```
   C:\msys64\mingw64\bin
   ```

   이렇게 하면 PowerShell이나 CMD에서도 `gcc`, `make` 등이 인식됩니다.

---

## 💾 데이터 파일

대용량 바이너리/데이터는 Git으로 관리하지 않습니다.
아래 링크에서 다운로드 후 `data/` 폴더에 넣어주세요:

> **[데이터 파일 다운로드 (Google Drive)](https://drive.google.com/drive/folders/1liUMwRpyAcMHEVUwi5Ss53gn_1I4D6cg?usp=sharing)**

**필수 파일**

* `zS.bin`
* `r4_clock_patterns.bin`
* `ciphertext.bin`
* `Gt.bin`
* `s.bin`

---

## ⚡️ 빌드 방법

```bash
# 전체 빌드 (라이브러리 + 도구)
make

# 개별 빌드
make core_library       # 코어 라이브러리만
make tools              # tools 디렉터리의 툴만
make debug_init         # debug_init.exe 생성
make verify_r4          # R4 검증 툴 생성

# 빌드 아티팩트 정리
make clean
```

* MSYS2 MinGW64 쉘에서 `make` 명령을 실행하세요.
* 빌드 결과물은 `bin/` 디렉터리에 생성됩니다.

---

## 🚀 주요 기능

1. **GF(2) 행렬 연산**

   * M4RI 기반 빠른 연산
   * Companion matrix 전역 캐싱

2. **LFSR 스트림 암호**

   * 4개 레지스터 동시 운영
   * 패턴 기반 clock 제어
   * 선형화된 상태 확장(zS)으로 최적화

3. **성능 최적화**

   * 미리 계산된 zS 데이터 활용
   * M4RI 벡터화 연산
   * 메모리 및 연산 효율 고려

---

## 📖 사용 예시

### 행렬 연산

```c
#include "crypto_lib.h"

crypto_matrix_t* A = crypto_matrix_init(4, 4, "A");
crypto_matrix_t* B = crypto_matrix_init(4, 4, "B");

crypto_matrix_t* C = crypto_matrix_multiply(A, B);
crypto_matrix_t* D = crypto_matrix_add(A, B);
```

### LFSR 스트림 암호

```c
#include "encrypt.h"

// 초기화
lfsr_matrices_init();
lfsr_matrix_state_t state;
lfsr_matrix_initialization(&state);

// 키스트림 생성
mzd_t* keystream = mzd_init(1, CIPHERTEXT_SIZE);
keystream_generation_with_pattern_m4ri(&state, pattern, keystream);
```

---

## 🛠 확장 및 기여

1. **새 기능 추가**

   * `include/` 및 `source/`에 헤더/소스 추가
   * Makefile에 빌드 규칙 등록

2. **성능 개선**

   * Companion matrix, zS 데이터 캐싱
   * M4RI 벡터 연산 적극 활용

---

## 📜 라이선스

본 프로젝트는 교육 및 연구 목적으로 제공됩니다.

```text
MIT License
...
```

```


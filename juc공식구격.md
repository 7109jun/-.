# JUC Specification v1.0

> JUC (Juc Universal Compression)
> 이미지 압축 포맷 규격

---

## 1. 개요

JUC는 이미지 데이터를 효율적으로 저장하기 위한 압축 포맷이다.

목표:

- 이미지 용량 감소
- 색상 데이터 최적화
- 빠른 처리
- 동일한 규격 기반의 호환성 제공

---

## 2. 파일 확장자
.juc

---

## 3. 규격 공개 범위

공개:

- 파일 구조
- 데이터 저장 방식
- 압축 데이터 구조
- 복원 방식
- 메타데이터 형식

---

## 4. JUC 파일 구조
Header

Metadata

Image Data

Compression Data

End Marker

---

## 5. Header

| 이름 | 크기 | 설명 |
|---|---:|---|
| Magic | 4 Byte | JUC 식별 |
| Version | 2 Byte | 규격 버전 |
| Width | 4 Byte | 이미지 가로 |
| Height | 4 Byte | 이미지 세로 |

---

## 6. 이미지 데이터

JUC는 이미지 정보를 다음 데이터로 구성한다.

- Pixel Data
- Color Data
- Position Data

---

## 7. 압축 방식

JUC 압축은 다음 데이터를 기반으로 한다.

- 색상 중복 제거
- 데이터 그룹화
- 이미지 정보 최적화

---

## 8. 복원
JUC

↓

압축 해제

↓

Pixel 재구성

↓

이미지 출력

---

## 9. 규격 상태

현재 버전:


JUC v1.0


이 규격을 기준으로 JUC 파일을 처리할 수 있다.

---

# JUC Specification

Version: 1.0

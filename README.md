# 🎥 video_rdm_test

> **RDM(복제방지) 기반의 안전한 동영상 스트리밍 백엔드 서버**

---

## 📝 소개

**video_rdm_test**는 동영상 콘텐츠의 무단 복제를 방지하기 위한 **RDM(Replication Defense Mechanism)** 기술이 적용된 백엔드 시스템입니다.  
AWS를 기반으로 한 스토리지/전송 구조와 NestJS, PostgreSQL을 활용하여,  
**저작권 보호가 필요한 콘텐츠 제공 플랫폼의 기초 인프라**를 구성하는 것을 목적으로 합니다.

---

## ⚙️ 기술 스택

| 구분 | 사용 기술 |
|------|-----------|
| 언어 | TypeScript |
| 프레임워크 | [NestJS](https://nestjs.com/) |
| 데이터베이스 | PostgreSQL |
| 클라우드 | AWS S3 (영상 저장), CloudFront 또는 presigned URL |
| 인증/보안 | JWT, IP 검사, RDM 토큰 기반 접근 제한 |

---

## 📸 데모 / 스크린샷

> 개발 완료 후 아래에 영상 또는 흐름도 이미지 추가 예정

- 📘 Swagger API 문서 예시:
  - `/auth/login` – 사용자 인증 (JWT 발급)
  - `/video/upload` – 영상 업로드 (RDM 보호 설정 포함)
  - `/video/stream/:id` – presigned URL 또는 인증 기반 스트리밍
  - `/rdm/validate-token` – RDM 토큰 유효성 검사

---

## 🔒 RDM 복제방지 방식 설명

**1. 접근제한 토큰 기반 제어**
- 각 영상 요청 시 **일회성 RDM 토큰**을 함께 전송
- 토큰은 다음 요소를 포함하여 서명됨:
  - `userId`, `videoId`, `ipAddress`, `expiresAt`, `hash`
- 서버는 요청 시:
  - IP 주소 일치 여부 확인
  - 만료시간 확인
  - HMAC 해시 검증

**2. Pre-signed URL 활용**
- AWS S3에서 생성된 presigned URL은 유효시간과 IP 제약이 걸려 있음
- 다운로드 대신 스트리밍 전용으로 사용됨

**3. 재생 로그 기록**
- 각 영상 스트리밍 요청은 서버 로그에 저장
- 동일 토큰의 재사용, 과도한 재생 요청 등은 차단

---

## 🗂️ AWS 구성도

[Client]
│
│ 1. 로그인 (JWT 발급)
▼
[NestJS 서버]
│
├── /video/stream 요청 → RDM 토큰 확인
├── S3 presigned URL 생성
▼
[AWS S3] ←────── [CloudFront] (옵션)
│
└─ 영상 파일 응답 (제한 조건 포함)

- **CloudFront** 사용 시, Signed Cookie/URL 기반 제어 가능
- **S3** 단독 presigned URL 방식도 병행 가능

---

## 🙋‍♂️ 개발자 소개

| 역할 | 이름 | 설명 |
|------|------|------|
| 백엔드 개발 | 익명 | NestJS 기반 REST API 설계, AWS 연동, RDM 구조 설계 |

---

## 📄 라이선스

이 프로젝트는 [MIT 라이선스](LICENSE)를 따릅니다.

---

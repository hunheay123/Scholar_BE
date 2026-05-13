# 📚 Scholar — 자격증 스터디 플랫폼 백엔드

> **자격증 정보 조회 · 스터디 커뮤니티 · 일정 관리**를 제공하는 Spring Boot REST API 서버

<br>

[![Java](https://img.shields.io/badge/Java-17-007396?style=flat-square&logo=openjdk)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5.4-6DB33F?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![Spring Security](https://img.shields.io/badge/Spring_Security-JWT-6DB33F?style=flat-square&logo=springsecurity)](https://spring.io/projects/spring-security)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)](https://www.mysql.com/)

---

## 📌 프로젝트 소개

**Scholar**는 자격증을 준비하는 사람들을 위한 통합 플랫폼의 백엔드 서버입니다.

자격증 정보 탐색부터 시험 일정 관리, 스터디 커뮤니티까지 — 자격증 준비에 필요한 기능을 REST API로 제공합니다.  
JWT 기반 인증으로 보안을 갖추고, Spring Data JPA로 데이터를 관리합니다.

---

## ✨ 주요 기능

### 🔐 인증 (Auth)
| 기능 | 설명 |
|------|------|
| 회원가입 / 로그인 | 이메일·닉네임 중복 검사 포함 |
| JWT 토큰 발급 | Access Token + Refresh Token 이중 발급 |
| 토큰 재발급 | Refresh Token으로 Access Token 갱신 |
| 비밀번호 재설정 | 이메일 토큰 기반 비밀번호 변경 |
| 로그아웃 / 회원탈퇴 | 세션 무효화 및 계정 삭제 |

### 📋 자격증 (Certificate)
- 카테고리별 자격증 목록 조회
- 자격증 상세 정보 조회 (응시료, 시험 방법, 합격 기준, 응시 자격, 주관 기관 등)
- 관심 자격증 즐겨찾기(Favorite) 등록 및 관리

### 🗓️ 일정 (Schedule)
- 자격증별 시험 일정 조회
- 즐겨찾기한 자격증의 일정만 날짜 범위로 필터링 조회

### 📝 스터디 게시판 (Study Post)
- 카테고리별 스터디 게시글 목록 / 상세 조회
- 게시글 작성 · 수정 · 삭제 (본인 인증 필요)
- 게시글 댓글 CRUD

### 💬 정보 공유 게시판 (Share Post)
- 자격증 정보·후기 공유 게시글 CRUD
- 공유 게시글 댓글 CRUD

### 👤 사용자 (User)
- 내 정보 조회 및 수정
- 내가 작성한 게시글 / 댓글 조회

---

## 🛠️ 기술 스택

| 분류 | 기술 |
|------|------|
| **Language** | Java 17 |
| **Framework** | Spring Boot 3.5.4 |
| **Security** | Spring Security, JWT (JJWT 0.11.5) |
| **ORM** | Spring Data JPA (Hibernate) |
| **Database** | MySQL 8.0 (운영), H2 (개발/테스트) |
| **Validation** | Spring Validation |
| **Build** | Gradle |
| **Utility** | Lombok |

---

## 📂 프로젝트 구조

```
src/main/java/com/example/certif/
├── CertifApplication.java
├── config/
│   └── SecurityConfig.java          # Spring Security 설정
├── controller/                       # REST API 엔드포인트
│   ├── AuthController.java
│   ├── CertificateController.java
│   ├── CategoryController.java
│   ├── FavoriteController.java
│   ├── ScheduleController.java
│   ├── StudyPostController.java
│   ├── StudyCommentController.java
│   ├── SharePostController.java
│   └── UserController.java
├── service/                          # 비즈니스 로직
├── repository/                       # JPA 레포지토리
├── entity/                           # JPA 엔티티
│   ├── User.java
│   ├── Certificate.java
│   ├── Category.java
│   ├── Favorite.java
│   ├── Schedule.java
│   ├── StudyPost.java / StudyComment.java
│   └── SharePost.java / ShareComment.java
├── dto/                              # 요청/응답 DTO
├── security/                         # JWT 필터, 인증 핸들러
│   ├── JwtAuthenticationFilter.java
│   ├── CustomUserDetailsService.java
│   └── UserPrincipal.java
└── util/
    └── JwtUtil.java                  # JWT 생성 및 검증
```

---

## 🔗 API 명세

### Auth
| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/api/auth/signup` | 회원가입 |
| POST | `/api/auth/login` | 로그인 (토큰 발급) |
| POST | `/api/auth/logout` | 로그아웃 |
| POST | `/api/auth/refresh-token` | Access Token 재발급 |
| GET | `/api/auth/check-email` | 이메일 중복 확인 |
| GET | `/api/auth/check-nickname` | 닉네임 중복 확인 |
| POST | `/api/auth/password-reset/request` | 비밀번호 재설정 요청 |
| POST | `/api/auth/password-reset/confirm` | 비밀번호 재설정 완료 |
| DELETE | `/api/auth/delete-account` | 회원탈퇴 |

### Certificate
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/api/categories/{categoryId}/certificates` | 카테고리별 자격증 목록 |
| GET | `/api/certificates/{certificateId}` | 자격증 상세 조회 |

### Schedule
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/api/certificates/{certificateId}/schedules` | 자격증 시험 일정 |
| GET | `/api/schedules/favorites` | 즐겨찾기 자격증 일정 (날짜 필터) |

### Study Post
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/api/study/default` | 기본 카테고리 게시글 목록 |
| GET | `/api/study/category/{categoryId}` | 카테고리별 게시글 |
| GET | `/api/study/{postId}` | 게시글 상세 |
| POST | `/api/study` | 게시글 작성 🔒 |
| PATCH | `/api/study/{postId}` | 게시글 수정 🔒 |
| DELETE | `/api/study/{postId}` | 게시글 삭제 🔒 |

> 🔒 JWT 인증 필요

---

## 🚀 실행 방법

### 사전 요구사항
- Java 17+
- MySQL 8.0+

### 환경 변수 설정

`.env` 파일 또는 `application.properties`에 아래 항목을 설정합니다.

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/{DB명}
spring.datasource.username={MySQL 계정}
spring.datasource.password={MySQL 비밀번호}

jwt.secret={JWT 시크릿 키}
jwt.access-expiration=3600000
jwt.refresh-expiration=604800000
```

### 빌드 및 실행

```bash
# 저장소 클론
git clone https://github.com/hunheay123/Scholar_BE.git
cd Scholar_BE

# 빌드
./gradlew build

# 실행
./gradlew bootRun
```

서버는 기본적으로 `http://localhost:8080`에서 실행됩니다.

---

## 🏗️ 아키텍처

```
Client
  │
  ▼
[Controller Layer]  — REST API 요청 수신, DTO 변환
  │
  ▼
[Service Layer]     — 비즈니스 로직 처리
  │
  ▼
[Repository Layer]  — Spring Data JPA, DB 접근
  │
  ▼
[MySQL / H2]
```

**인증 흐름 (JWT)**
```
로그인 요청 → AuthController
  → 자격증명 검증 (CustomUserDetailsService)
  → Access Token + Refresh Token 발급
  → 이후 요청: JwtAuthenticationFilter에서 토큰 검증
```

---

## 📄 라이선스

This project is for academic and portfolio purposes.

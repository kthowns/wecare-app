# WeCare

**지속적인 관리와 케어가 필요한 피보호자를 위한 돌봄 서비스**

Goorm DeepDive 해커톤에서 개발한 Spring Boot + React Native 프로젝트입니다. 보호자와 피보호자가 초대 코드로 연결되어 복약, 식사, 활동 등의 일상 루틴을 관리하고 수행 기록과 메모를 공유합니다.

이 저장소는 [WeCare-DeepDive/wecare-app](https://github.com/WeCare-DeepDive/wecare-app)의 fork입니다. 팀 프로젝트의 주요 기능과 fork 소유자 [kthowns](https://github.com/kthowns)의 기여를 함께 정리했습니다.

## 주요 기능

| 영역 | 구현 내용 |
| --- | --- |
| 회원가입·인증 | 보호자·피보호자 역할, JWT 로그인·로그아웃·토큰 재발급, Redis 기반 토큰 관리 |
| 가족 연결 | 초대 코드 생성·수락, 연결 목록 및 상대 정보 조회, 관계 수정·연결 해제 |
| 루틴 관리 | 복약·영양제·식사·활동·사용자 정의 루틴 생성·수정·삭제, 반복 요일 및 알림 설정 저장 |
| 수행 기록 | 당일 루틴 및 수행 기록 조회, 완료·완료 취소, 보호자·피보호자 메모 관리 |
| 모바일 앱 | 로그인·회원가입, 가족 연결 화면 및 상태 관리, 홈·일정·루틴 화면 구성 |

해커톤 개발 당시의 구현 상태를 담고 있습니다. 리포트 화면은 placeholder이며 SOS 버튼은 로그 출력 단계입니다. 음성·사진 파일 처리와 서버 푸시 알림은 후속 구현 항목입니다. 일부 화면에는 목업 데이터가 포함되어 있습니다.

## 주요 기여 — kthowns

원본 저장소에 병합된 PR을 기준으로 정리했습니다. 인증과 루틴 등의 기능은 팀원들과 함께 개발했으며, 아래는 해당 계정이 수행한 작업입니다.

| 영역 | 작업 내용 | 관련 PR |
| --- | --- | --- |
| 초기 구성·API 문서 | 백엔드 프로젝트 초기화, Swagger·CORS 설정, API 상세 명세 및 토큰 입력 구성 | [#1](https://github.com/WeCare-DeepDive/wecare-app/pull/1), [#4](https://github.com/WeCare-DeepDive/wecare-app/pull/4), [#98](https://github.com/WeCare-DeepDive/wecare-app/pull/98) |
| CI/CD·배포 | GitHub Actions 테스트·배포 파이프라인, Docker 이미지 빌드·배포, EC2 및 Docker Compose 배포 구성과 오류 수정 | [#14](https://github.com/WeCare-DeepDive/wecare-app/pull/14), [#52](https://github.com/WeCare-DeepDive/wecare-app/pull/52), [#54](https://github.com/WeCare-DeepDive/wecare-app/pull/54) |
| 인증·인가 개선 | 인증 클래스 역할 분리, JWT 검증·로그아웃·재발급 로직 개선, 리소스 접근 권한 검사 및 Security 예외 처리 | [#49](https://github.com/WeCare-DeepDive/wecare-app/pull/49), [#96](https://github.com/WeCare-DeepDive/wecare-app/pull/96), [#113](https://github.com/WeCare-DeepDive/wecare-app/pull/113) |
| 도메인·API 정비 | 초대·연결 도메인 분리, 루틴·수행 기록 API 정비, 스키마 재구성, 공통 예외·응답 코드 적용 | [#96](https://github.com/WeCare-DeepDive/wecare-app/pull/96) |
| 버그 수정·기능 보완 | 날짜 타입 수정, 초대 코드 자기 연결 버그 수정, 루틴 엔드포인트 충돌 수정, 상대 이름을 포함한 연결 상세 조회 추가 | [#101](https://github.com/WeCare-DeepDive/wecare-app/pull/101), [#102](https://github.com/WeCare-DeepDive/wecare-app/pull/102), [#103](https://github.com/WeCare-DeepDive/wecare-app/pull/103), [#104](https://github.com/WeCare-DeepDive/wecare-app/pull/104) |

전체 개발 내역은 [원본 저장소 PR 목록](https://github.com/WeCare-DeepDive/wecare-app/pulls?q=is%3Apr)에서 확인할 수 있습니다.

## 기술 스택

| 구분 | 기술 |
| --- | --- |
| Backend | Java 17, Spring Boot 3.5.3, Spring Security, Spring Data JPA, Bean Validation, JWT |
| Database·Cache | MySQL, Redis |
| Frontend | React Native 0.76.9, React 18.3.1, Expo SDK 52, React Navigation, Zustand, Axios |
| API 문서·테스트 | springdoc-openapi / Swagger UI, JUnit 5, Spring Boot Test, Spring Security Test |
| 배포 | GitHub Actions, Docker, Docker Compose, AWS EC2 |

## 프로젝트 구조

```text
wecare-app/
├── wecare-backend/wecare/       # Spring Boot API 서버
│   └── src/
│       ├── main/java/com/example/wecare/
│       │   ├── auth/           # JWT 인증
│       │   ├── member/         # 사용자
│       │   ├── invitation/     # 초대 코드
│       │   ├── connection/     # 보호자·피보호자 연결
│       │   ├── routine/        # 루틴·수행 기록
│       │   └── common/         # 설정·접근 제어·예외 처리
│       └── test/               # 단위·통합 테스트
├── wecare-frontend/wecare-fe/   # React Native / Expo 앱
├── .github/workflows/          # 테스트·배포 자동화
└── docs/api.md                 # REST API 요약
```

## 로컬 실행

### 백엔드

JDK 17, MySQL, Redis가 필요합니다. Gradle Wrapper는 저장소에 포함되어 있습니다.

1. MySQL에 `wecare_test` 데이터베이스와 접근 가능한 계정을 준비합니다.
2. [개발 설정](wecare-backend/wecare/src/main/resources/application-dev.yml)의 DB 연결 정보, Redis 주소, JWT 설정을 로컬 환경에 맞춥니다. 기본 연결은 MySQL `localhost:3306`, Redis `localhost:6379`입니다.
3. 백엔드 디렉터리에서 개발 프로필로 실행합니다.

```bash
cd wecare-backend/wecare
./gradlew bootRun --args='--spring.profiles.active=dev'
```

Windows PowerShell에서는 `./gradlew` 대신 `.\gradlew.bat`를 사용합니다.

| 항목 | 로컬 주소 |
| --- | --- |
| API 서버 | `http://localhost:8080` |
| Swagger UI | `http://localhost:8080/api/my-docs` |
| OpenAPI JSON | `http://localhost:8080/api/api-spec` |

### 프론트엔드

Node.js와 npm, Android 에뮬레이터 또는 모바일 개발 환경이 필요합니다. iOS 빌드는 macOS와 Xcode가 필요합니다.

```bash
cd wecare-frontend/wecare-fe
npm install
```

해당 디렉터리에 `.env`를 생성하고 API 주소·타임아웃·로그 레벨을 설정합니다. 전체 환경변수 목록은 [프론트엔드 README](wecare-frontend/wecare-fe/README.md)를 참고하세요.

```dotenv
DEV_API_BASE_URL=http://10.0.2.2:8080
DEV_API_TIMEOUT=10000
DEV_LOG_LEVEL=debug
```

위 주소는 Android 기본 에뮬레이터 기준입니다. 실기기에서는 개발 PC의 LAN IP를, iOS 시뮬레이터에서는 `http://localhost:8080`을 사용합니다. 현재 코드는 `__DEV__`에 따라 development / production 설정을 선택합니다.

```bash
npm start
# Android 네이티브 개발 빌드
npm run android
# iOS 네이티브 개발 빌드 (macOS)
npm run ios
```

### 백엔드 테스트

테스트 프로필은 로컬 MySQL과 Redis를 사용합니다. [테스트 설정](wecare-backend/wecare/src/main/resources/application-test.yml)에 맞는 별도 테스트 DB를 준비한 뒤 백엔드 디렉터리에서 실행합니다. 테스트 실행 시 `schema.sql`이 적용됩니다.

```bash
./gradlew test -Dspring.profiles.active=test
```

## API 문서

엔드포인트 요약과 루틴 요청 예시는 [REST API 요약](docs/api.md)을 참고하세요. 상세 요청·응답 스키마는 서버 실행 후 Swagger UI에서 확인할 수 있습니다.

인증이 필요한 요청은 `Authorization: Bearer <accessToken>`을 사용합니다. 로그아웃·토큰 재발급에는 `Refresh-Token: Bearer <refreshToken>`도 필요합니다.

## CI/CD

- `dev` 및 `main` 대상 PR에서 MySQL·Redis 서비스와 함께 백엔드 테스트를 실행합니다.
- `main` push 시 JAR 빌드 → Docker 이미지 빌드·푸시 → SSH를 통한 EC2의 Docker Compose 배포를 수행합니다.
- fork에서 배포하려면 GitHub Actions의 `wecare-backend` 환경과 워크플로에서 참조하는 DB·JWT·Docker·EC2 Secrets를 별도로 설정해야 합니다.

워크플로는 [.github/workflows](.github/workflows)에 있습니다. 기존 배포 주소는 `https://wecare.kthowns.cloud`이며 현재 운영 여부는 확인하지 않았습니다.

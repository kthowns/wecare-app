# 🏥 We Care REST API Specification (v1.0.0)

We Care 서비스의 백엔드 REST API 명세서입니다. 모든 요청은 JSON 형식을 따르며, 인증이 필요한 API는 헤더에 토큰을 포함해야 합니다.

---

## 🌐 Server Information
* **Production:** `https://wecare.kthowns.cloud`
* **Local:** `http://localhost:8080`

## 🔑 Authentication
* **Access Token:** `Authorization` 헤더 (Bearer 없이 토큰 원본 전송)
* **Refresh Token:** `Refresh-Token` 헤더 (접두사 `"Bearer "` 포함 필수, 공백 주의)

---

## 📂 API Summary

### 1. 인증 서비스 (`인증 서비스`)
| Method | Path | Summary | Details |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/signup` | 회원가입 | 사용자 가입 |
| `POST` | `/api/auth/login` | 로그인 | 토큰 발급 |
| `POST` | `/api/auth/reissue` | 토큰 재발급 | Refresh-Token 헤더 필요 |
| `POST` | `/api/auth/logout` | 로그아웃 | 세션 종료 |

### 2. 사용자 서비스 (`사용자 서비스`)
| Method | Path | Summary | Details |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/members/me` | 내 정보 조회 | 상세 개인 정보 |
| `GET` | `/api/members/partner` | 연결된 상대 정보 조회 | 민감 정보 제외 |

### 3. 루틴 서비스 (`루틴 서비스`)
| Method | Path | Summary | Details |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/routines/{dependentId}` | 당일 전체 루틴 목록 조회 | 수행 기록 포함 |
| `POST` | `/api/routines/{dependentId}` | 루틴 생성 | 보호자만 가능 |
| `PATCH` | `/api/routines/{routineId}` | 루틴 업데이트 | 정보 수정 |
| `DELETE` | `/api/routines/{routineId}` | 루틴 삭제 | Hard-delete (기록은 보존) |
| `GET` | `/api/routines/{routineId}/details` | 루틴 세부 정보 조회 | 상세 설정 포함 |
| `PUT` | `/api/routines/{routineId}/repeats` | 반복 요일 업데이트 | 덮어쓰기 방식 |
| `PATCH` | `/api/routines/{routineId}/memo` | 메모 업데이트 | 역할별 메모 수정 |
| `POST` | `/api/routines/{routineId}/complete` | 루틴 수행 체크 | 시간 내 체크 시 성공 |
| `PATCH` | `/api/routines/{historyId}/undo` | 수행 체크 철회 | 기록 삭제 |

### 4. 초대 및 연결 서비스 (`초대/연결 서비스`)
| Method | Path | Summary | Details |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/invitations/generate` | 초대 코드 생성 | 10분간 유효 |
| `POST` | `/api/invitations/accept` | 초대 코드 수락 | 관계 수립 |
| `GET` | `/api/connections` | 내 연결 목록 조회 | |
| `GET` | `/api/connections/details` | 내 연결 세부 정보 조회 | 상대방 이름 포함 |
| `PATCH` | `/api/connections/{connectionId}` | 연결 관계 수정 | 관계(부모 등) 재설정 |
| `PATCH` | `/api/connections/{connectionId}/deactivate` | 연결 해제하기 | Soft-delete |

---

## 📦 Request DTO Examples

### 📝 RoutineRequest (루틴 생성/수정)
```json
{
  "title": "영양제 섭취",
  "startTime": "14:30:00",
  "endTime": "15:00:00",
  "routineType": "SUPPLEMENT", // ACTIVITY, CUSTOM, MEDICATION, SUPPLEMENT, MEAL
  "isAlertActive": true,
  "notificationType": "ON_START_TIME", // NONE, ON_START_TIME, ON_END_TIME, EVERY_10_MINUTES, EVERY_30_MINUTES, EVERY_HOUR
  "soundType": "DEFAULT_SOUND", // DEFAULT_SOUND, SILENT, VIBRATION, VOICE_MESSAGE
  "repeatDays": ["MON", "WED", "FRI"],
  "guardianMemo": "보호자 메모",
  "dependentMemo": "피보호자 메모"
}

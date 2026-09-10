# RunQuest API 명세서

기본 URL은 `/api/`입니다. 인증이 필요한 API는 `Authorization: Bearer <access_token>` 헤더를 사용합니다.

## 인증

| Method | Endpoint | 설명 |
| --- | --- | --- |
| POST | `/auth/signup/` | 회원가입과 JWT 발급 |
| POST | `/auth/login/` | 로그인과 JWT 발급 |
| POST | `/auth/token/refresh/` | access token 갱신 |
| POST | `/auth/logout/` | refresh token 무효화 |
| GET | `/auth/me/` | 현재 사용자 정보 조회 |
| GET/PATCH | `/auth/settings/` | 사용자 설정 조회와 수정 |
| GET | `/auth/nickname-availability/` | 닉네임 중복 확인 |
| POST | `/auth/password/change/` | 비밀번호 변경 |

## 홈, 설문, 날씨

| Method | Endpoint | 설명 |
| --- | --- | --- |
| GET | `/home/` | 홈 대시보드 데이터 |
| GET | `/survey/questions/` | 설문 질문 목록 |
| GET/POST | `/survey/response/` | 설문 결과 조회와 저장 |
| GET | `/weather/current/` | 현재 위치 기준 날씨와 미세먼지 |

## 코스

| Method | Endpoint | 설명 |
| --- | --- | --- |
| GET | `/courses/` | 기본 코스 목록 |
| GET | `/courses/{slug}/` | 기본 코스 상세 |
| POST/DELETE | `/courses/{slug}/favorite/` | 코스 북마크 추가와 해제 |
| GET | `/courses/favorites/` | 북마크한 코스 목록 |
| GET/POST | `/custom-courses/` | 사용자 코스 목록과 생성 |
| GET/PATCH/DELETE | `/custom-courses/{id}/` | 사용자 코스 상세, 수정, 삭제 |

## 추천

| Method | Endpoint | 설명 |
| --- | --- | --- |
| POST | `/recommendations/generate/` | 설문, 기록, 위치 기반 추천 생성 |
| GET | `/recommendations/current/` | 현재 추천 조회 |
| POST | `/recommendations/next/` | 다음 추천으로 이동 |

## 러닝

| Method | Endpoint | 설명 |
| --- | --- | --- |
| POST | `/runs/start/` | GPS 또는 시뮬레이션 러닝 시작 |
| GET | `/runs/active/` | 진행 중 러닝 조회 |
| PATCH | `/runs/{id}/progress/` | 좌표, 시간, 일시정지 상태 저장 |
| POST | `/runs/{id}/finish/` | 러닝 종료와 기록 확정 |
| GET | `/runs/{id}/` | 러닝 기록 상세 |
| POST | `/runs/{id}/save-course/` | 자유 러닝 기록을 사용자 코스로 저장 |
| GET | `/profile/records/` | 내 러닝 기록 목록 |
| GET | `/profile/summary/` | 내 러닝 통계 요약 |
| GET | `/profile/attendance/` | 출석 기록 |
| GET | `/profile/badges/` | 배지 목록 |

## 커뮤니티

| Method | Endpoint | 설명 |
| --- | --- | --- |
| GET/POST | `/community/posts/` | 게시글 목록과 작성 |
| GET/PATCH/DELETE | `/community/posts/{id}/` | 게시글 상세, 수정, 삭제 |
| POST | `/community/posts/{id}/like/` | 게시글 좋아요 토글 |
| POST | `/community/posts/{id}/save/` | 게시글 저장 토글 |
| POST | `/community/posts/{id}/comments/` | 댓글 작성 |
| PATCH/DELETE | `/community/comments/{id}/` | 댓글 수정과 삭제 |
| POST | `/community/comments/{id}/like/` | 댓글 좋아요 토글 |

## 챗봇과 알림

| Method | Endpoint | 설명 |
| --- | --- | --- |
| GET/POST/DELETE | `/chat/conversation/` | 챗봇 대화 조회, 전송, 초기화 |
| GET | `/notifications/` | 알림 목록 |
| POST | `/notifications/read-all/` | 전체 알림 읽음 처리 |
| POST | `/notifications/{id}/read/` | 개별 알림 읽음 처리 |

## 응답 정책

| 상황 | Status Code |
| --- | --- |
| 조회 성공 | `200 OK` |
| 생성 성공 | `201 Created` |
| 삭제 성공 | `204 No Content` |
| 입력값 오류 | `400 Bad Request` |
| 인증 필요 | `401 Unauthorized` |
| 권한 없음 | `403 Forbidden` |
| 리소스 없음 | `404 Not Found` |


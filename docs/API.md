# RunQuest API 가이드

이 문서는 API 사용 흐름을 설명합니다. 전체 OpenAPI 문서는 로컬 서버 실행 후 `/api/docs/` 또는 `/api/redoc/`에서 확인할 수 있습니다.

## 기본 정보

| 항목 | 값 |
| --- | --- |
| 기본 prefix | `/api/` |
| 인증 방식 | JWT Bearer Token |
| 문서 | `/api/docs/`, `/api/redoc/`, `/api/schema/` |

## 인증 흐름

1. 사용자는 `/api/auth/signup/` 또는 `/api/auth/login/`으로 access token과 refresh token을 받습니다.
2. 프론트엔드는 access token을 `Authorization: Bearer <token>` 헤더에 담아 API를 호출합니다.
3. access token이 만료되면 `/api/auth/token/refresh/`로 갱신합니다.
4. 로그아웃 시 `/api/auth/logout/`으로 refresh token을 무효화합니다.

## 추천 API 흐름

1. `/api/survey/response/`에 설문을 저장합니다.
2. `/api/recommendations/generate/`에 현재 위치를 선택적으로 전달합니다.
3. 서버는 설문, 기록, 위치, 복귀 여부를 기반으로 추천 세션을 생성합니다.
4. `/api/recommendations/current/`로 현재 추천을 조회합니다.
5. `/api/recommendations/next/`로 다음 추천을 확인합니다.

추천 생성 요청 예시:

```json
{
  "latitude": 36.350411,
  "longitude": 127.384548
}
```

## 러닝 API 흐름

1. `/api/runs/start/`로 러닝을 시작합니다.
2. GPS 모드에서는 브라우저 좌표를 주기적으로 `/api/runs/{id}/progress/`에 저장합니다.
3. 시뮬레이션 모드에서는 선택한 코스 좌표를 시간에 따라 보간해 progress를 갱신합니다.
4. `/api/runs/{id}/finish/`로 러닝을 종료합니다.
5. 자유 러닝이고 track point가 충분하면 `/api/runs/{id}/save-course/`로 사용자 코스로 저장합니다.

러닝 시작 요청 예시:

```json
{
  "tracking_mode": "gps",
  "course_slug": "gapcheon-river"
}
```

러닝 종료 응답에는 기록, 출석, 새 배지, AI 피드백 정보가 포함됩니다. 단, 이동 거리가 100m 미만이면 저장하지 않고 취소 응답을 반환합니다.

## 커뮤니티 API 흐름

커뮤니티는 게시글, 댓글, 좋아요, 저장, 관련 코스/러닝 기록 연결을 지원합니다.

목록 API는 빠른 렌더링을 위해 이미지 원본과 상세 nested 데이터를 제외합니다. 게시글 상세 API에서 댓글과 관련 데이터를 함께 조회합니다.

게시글 작성 요청 예시:

```json
{
  "category": "course",
  "title": "갑천 저녁 러닝 추천",
  "content": "바람이 시원하고 조명이 좋아요.",
  "related_course": 1
}
```

## 챗봇 API 흐름

1. `/api/chat/conversation/` GET으로 기존 대화를 조회합니다.
2. 같은 endpoint에 POST로 사용자 메시지를 보냅니다.
3. 서버는 메시지를 분석하고 필요한 경우 추천 코스를 함께 반환합니다.
4. 외부 AI가 비활성화되어도 로컬 fallback 응답으로 동작합니다.

## 알림 API 흐름

알림은 댓글, 좋아요, 배지, 추천, 시스템 이벤트를 담습니다. 프론트엔드는 알림의 `link` 또는 `metadata`를 기반으로 관련 화면으로 이동합니다.


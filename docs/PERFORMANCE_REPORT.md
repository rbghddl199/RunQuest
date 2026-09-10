# 성능 측정 및 개선 보고서

## 측정 방식

RunQuest는 요청 단위와 함수 단위로 실행 시간을 측정할 수 있도록 구성했습니다.

| 위치 | 역할 |
| --- | --- |
| `backend/core/middleware.py` | API 요청별 실행 시간과 DB 쿼리 수 기록 |
| `backend/core/performance.py` | 특정 로직 블록의 실행 시간 측정 |
| `backend/scripts/analyze_performance.py` | 커뮤니티와 챗봇 주요 API 성능 측정 |
| `backend/runquest-performance.log` | 성능 로그 저장 파일 |

성능 로그는 `.env`에 아래 값을 설정하면 활성화됩니다.

```env
RUNQUEST_PERFORMANCE_LOGS=True
RUNQUEST_SLOW_REQUEST_MS=350
```

## 최근 측정 결과

로컬 SQLite DB 기준 측정 결과입니다.

| 항목 | 결과 |
| --- | ---: |
| 커뮤니티 최신 목록 | 약 202ms / 1 query |
| 커뮤니티 인기 목록 | 약 15ms / 1 query |
| 커뮤니티 상세 | 약 11ms / 2 queries |
| 챗봇 로컬 응답 | 약 31ms / 16 queries |

## 적용한 개선

1. 커뮤니티 목록 API에서 base64 이미지 원본을 제외했습니다.
2. 목록 API에서 상세 코스, 러닝 기록, 댓글 nested 데이터를 제외했습니다.
3. 좋아요 수와 댓글 수는 serializer 반복 조회가 아니라 DB annotation으로 계산했습니다.
4. 게시글, 댓글, 좋아요, 저장 테이블에 조회용 index를 추가했습니다.
5. `select_related`, `prefetch_related`, `defer`를 적용해 불필요한 쿼리와 응답 크기를 줄였습니다.
6. 챗봇 응답 과정은 단계별 로그를 남겨 병목을 확인할 수 있게 했습니다.

## 추가 개선 후보

| 후보 | 이유 |
| --- | --- |
| 이미지 파일 저장소 전환 | base64 JSON보다 media URL 또는 S3 URL 방식이 운영에 적합 |
| 커뮤니티 인기 목록 캐싱 | 좋아요/댓글 수 기반 정렬은 데이터가 커질수록 비용 증가 |
| PostgreSQL 전환 | 운영 규모에서는 SQLite보다 인덱스와 동시성 처리에 유리 |
| 프론트 이미지 lazy loading | 초기 렌더링 비용 감소 |
| API pagination 표준화 | 목록 데이터 증가에 대비 |


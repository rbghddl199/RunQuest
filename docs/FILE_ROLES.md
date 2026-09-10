# 파일 역할 정리

## 백엔드

| 경로 | 역할 |
| --- | --- |
| `backend/manage.py` | Django 관리 명령 실행 진입점 |
| `backend/config/settings.py` | DB, 인증, CORS, 로깅, 성능 측정 설정 |
| `backend/config/urls.py` | 전체 URL 라우팅과 API 문서 연결 |
| `backend/core/models.py` | 사용자, 코스, 러닝 기록, 커뮤니티, 챗봇, 추천 DB 모델 |
| `backend/core/serializers.py` | 모델 데이터를 API JSON으로 변환하고 입력값 검증 |
| `backend/core/views.py` | 인증, 코스, 추천, 러닝, 커뮤니티, 챗봇 API 구현 |
| `backend/core/urls.py` | core 앱 API URL 라우팅 |
| `backend/core/recommendations.py` | 설문, 기록, 위치 기반 코스 추천 로직 |
| `backend/core/running.py` | 거리, 페이스, 칼로리, 좌표 계산 |
| `backend/core/gpx.py` | GPX 파일 파싱과 코스 데이터 변환 |
| `backend/core/chatbot.py` | 챗봇 응답과 코스 추천 메시지 생성 |
| `backend/core/weather.py` | 날씨와 미세먼지 정보 정규화 |
| `backend/core/notifications.py` | 알림 생성과 이동 링크 구성 |
| `backend/core/performance.py` | 함수 단위 실행 시간 측정 유틸리티 |
| `backend/core/middleware.py` | 요청 단위 성능 로그 미들웨어 |
| `backend/core/migrations/` | DB 스키마 변경 이력 |
| `backend/fixtures/runquest/` | 시연용 사용자, 코스, 기록, 커뮤니티 fixture |
| `backend/scripts/` | GPX 반영, 데이터 검증, 성능 분석 보조 스크립트 |

## 프론트엔드

| 경로 | 역할 |
| --- | --- |
| `frontend/src/main.js` | Vue, Pinia, Router 초기화 |
| `frontend/src/App.vue` | 최상위 앱 레이아웃 |
| `frontend/src/router/index.js` | 화면 URL 라우팅과 인증 가드 |
| `frontend/src/services/api.js` | Axios API 클라이언트와 토큰 처리 |
| `frontend/src/services/kakaoMap.js` | Kakao Map SDK 로딩 |
| `frontend/src/stores/` | 인증, 코스, 러닝, 추천, 커뮤니티, 챗봇 상태 관리 |
| `frontend/src/views/HomeView.vue` | 홈 대시보드 |
| `frontend/src/views/RecommendationView.vue` | 추천 코스 화면 |
| `frontend/src/views/RunStartView.vue` | 러닝 시작, GPX 적용, GPS/시뮬레이션 시작 |
| `frontend/src/views/RunActiveView.vue` | 러닝 중 실시간 기록 화면 |
| `frontend/src/views/RunCompletedView.vue` | 러닝 완료, 피드백, 코스 저장 화면 |
| `frontend/src/views/MyCoursesView.vue` | 북마크, 직접 생성, GPX 코스 관리 |
| `frontend/src/views/CommunityView.vue` | 커뮤니티 목록, 검색, 필터 |
| `frontend/src/views/CommunityDetailView.vue` | 게시글 상세, 댓글, 좋아요, 저장 |
| `frontend/src/views/ChatView.vue` | 챗봇 대화와 추천 코스 표시 |
| `frontend/src/components/maps/` | 코스 지도와 실시간 러닝 지도 컴포넌트 |
| `frontend/src/utils/` | 단위 변환, 코스 이미지 매핑, 프로필 이미지 유틸리티 |
| `frontend/public/` | 정적 이미지, 아이콘, 프로필 프레임 |

## 문서

| 경로 | 역할 |
| --- | --- |
| `README.md` | 설치, 실행, 데이터 준비, 테스트, 마무리 절차 |
| `docs/API_SPEC.md` | 주요 API 명세 |
| `docs/API.md` | API 상세 가이드 |
| `docs/ERD.md` | 데이터베이스 ERD |
| `docs/ARCHITECTURE.md` | 시스템 구조와 데이터 흐름 |
| `docs/DEPLOYMENT.md` | 배포 체크리스트 |
| `docs/PERFORMANCE_REPORT.md` | 성능 측정 결과와 개선 내역 |
| `docs/EXPECTED_QUESTIONS.md` | 발표 예상 질문과 답변 |
| `docs/PRESENTATION.md` | 시연 흐름과 발표 요약 |
| `docs/ui-review/` | 최신 시연 화면 스크린샷 |


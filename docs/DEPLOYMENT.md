# RunQuest 배포 체크리스트

현재 프로젝트는 로컬 시연 기준으로 구성되어 있습니다. 실제 배포를 진행할 경우 아래 항목을 순서대로 확인합니다.

## 환경 변수

| 변수 | 설명 |
| --- | --- |
| `DJANGO_DEBUG=False` | 운영 환경에서는 디버그 모드를 끕니다. |
| `DJANGO_SECRET_KEY` | 충분히 긴 무작위 문자열로 설정합니다. |
| `DJANGO_ALLOWED_HOSTS` | 배포 도메인과 서버 호스트를 등록합니다. |
| `DJANGO_CORS_ALLOWED_ORIGINS` | 프론트엔드 배포 주소를 등록합니다. |
| `VITE_API_BASE_URL` | HTTPS 백엔드 API 주소를 설정합니다. |
| `VITE_KAKAO_MAP_APP_KEY` | Kakao Map JavaScript 키를 설정합니다. |
| `OPENAI_API_KEY` | 외부 AI 피드백을 사용할 경우 설정합니다. |

## 배포 전 작업

1. `.env`가 Git에 포함되지 않았는지 확인합니다.
2. `python manage.py check`를 실행합니다.
3. `python manage.py test core`를 실행합니다.
4. `npm run build`를 실행합니다.
5. 운영 DB를 사용할 경우 마이그레이션을 적용합니다.
6. `python manage.py collectstatic --noinput`을 실행합니다.
7. Kakao Developers에 프론트엔드 HTTPS 도메인을 등록합니다.
8. 브라우저 GPS 사용을 위해 HTTPS로 서비스합니다.

## 로컬 컨테이너 실행

```powershell
docker compose up --build
```

## fixture 데이터 로드

```powershell
cd backend
python manage.py load_runquest_fixtures --reset
```

## 배포 후 점검

| 점검 항목 | 확인 내용 |
| --- | --- |
| 로그인 | JWT 발급과 토큰 갱신이 정상인지 확인 |
| 지도 | Kakao Map이 배포 도메인에서 로드되는지 확인 |
| GPS | HTTPS 환경에서 위치 권한과 실시간 좌표 수집 확인 |
| 커뮤니티 | 목록, 상세, 작성, 댓글, 좋아요 동작 확인 |
| 챗봇 | 외부 AI 설정 여부와 fallback 응답 확인 |
| 로그 | 느린 요청과 에러 로그 확인 |


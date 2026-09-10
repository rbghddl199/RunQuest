# RunQuest ERD

이 문서는 `backend/core/models.py` 기준의 논리 ERD입니다. Django 기본 인증 테이블은 `USER`로 축약해 표현했습니다.

## 전체 ERD

```mermaid
erDiagram
    USER {
        int id PK
        string username
        string email
        string password
        bool is_active
        datetime date_joined
    }

    USER_PROFILE {
        int id PK
        int user_id FK
        string nickname UK
        string user_type
        bool survey_completed
        datetime created_at
        datetime updated_at
    }

    USER_SETTINGS {
        int id PK
        int user_id FK
        bool push_notifications
        bool recommendation_notifications
        bool community_notifications
        bool marketing_notifications
        string distance_unit
        string temperature_unit
        string profile_visibility
        bool show_running_records
        datetime updated_at
    }

    SURVEY_RESPONSE {
        int id PK
        int user_id FK
        string experience_level
        string distance_range
        string course_type
        string preferred_time
        string running_style
        string running_goal
        json score_profile
        datetime completed_at
    }

    COURSE {
        int id PK
        string slug UK
        string name
        string district
        string course_type
        string difficulty
        decimal distance_km
        int estimated_minutes
        int elevation_gain_m
        decimal start_latitude
        decimal start_longitude
        json route_points
        json tags
        bool is_active
    }

    CUSTOM_COURSE {
        int id PK
        int owner_id FK
        string name
        string description
        decimal distance_km
        int elevation_gain_m
        decimal start_latitude
        decimal start_longitude
        json route_points
        string source
        string visibility
        datetime created_at
    }

    FAVORITE_COURSE {
        int id PK
        int user_id FK
        int course_id FK
        datetime created_at
    }

    RUNNING_RECORD {
        int id PK
        int user_id FK
        int course_id FK
        int custom_course_id FK
        string status
        string tracking_mode
        decimal distance_km
        int duration_seconds
        int paused_seconds
        int average_pace_seconds
        int calories
        json track_points
        json ai_feedback
        datetime started_at
        datetime completed_at
    }

    ATTENDANCE {
        int id PK
        int user_id FK
        int running_record_id FK
        date date
        datetime created_at
    }

    BADGE {
        int id PK
        string code UK
        string name
        string description
        string icon
        string color
        string condition_type
        int condition_value
        int sort_order
    }

    USER_BADGE {
        int id PK
        int user_id FK
        int badge_id FK
        datetime earned_at
    }

    NOTIFICATION {
        int id PK
        int user_id FK
        string notification_type
        string title
        string message
        string link
        bool is_read
        json metadata
        datetime created_at
    }

    COMMUNITY_POST {
        int id PK
        int author_id FK
        int related_course_id FK
        int running_record_id FK
        string category
        string title
        text content
        json images
        datetime created_at
        datetime updated_at
    }

    COMMUNITY_COMMENT {
        int id PK
        int post_id FK
        int author_id FK
        string content
        datetime created_at
        datetime updated_at
    }

    COMMUNITY_POST_LIKE {
        int id PK
        int user_id FK
        int post_id FK
        datetime created_at
    }

    COMMUNITY_COMMENT_LIKE {
        int id PK
        int user_id FK
        int comment_id FK
        datetime created_at
    }

    SAVED_COMMUNITY_POST {
        int id PK
        int user_id FK
        int post_id FK
        datetime created_at
    }

    CHAT_SESSION {
        int id PK
        int user_id FK
        datetime created_at
        datetime updated_at
    }

    CHAT_MESSAGE {
        int id PK
        int session_id FK
        int recommended_course_id FK
        string role
        text content
        json metadata
        datetime created_at
    }

    RECOMMENDATION_SESSION {
        int id PK
        int user_id FK
        decimal latitude
        decimal longitude
        json weights
        int current_index
        datetime created_at
    }

    RECOMMENDATION_ITEM {
        int id PK
        int session_id FK
        int course_id FK
        decimal score
        json reasons
        int rank
    }

    USER ||--|| USER_PROFILE : has
    USER ||--|| USER_SETTINGS : has
    USER ||--|| SURVEY_RESPONSE : answers
    USER ||--o{ FAVORITE_COURSE : saves
    COURSE ||--o{ FAVORITE_COURSE : saved_by
    USER ||--o{ CUSTOM_COURSE : creates
    USER ||--o{ RUNNING_RECORD : records
    COURSE ||--o{ RUNNING_RECORD : base_course
    CUSTOM_COURSE ||--o{ RUNNING_RECORD : custom_course
    USER ||--o{ ATTENDANCE : attends
    RUNNING_RECORD ||--o{ ATTENDANCE : creates
    USER ||--o{ USER_BADGE : earns
    BADGE ||--o{ USER_BADGE : awarded
    USER ||--o{ NOTIFICATION : receives
    USER ||--o{ COMMUNITY_POST : writes
    COURSE ||--o{ COMMUNITY_POST : related
    RUNNING_RECORD ||--o{ COMMUNITY_POST : shared
    COMMUNITY_POST ||--o{ COMMUNITY_COMMENT : has
    USER ||--o{ COMMUNITY_COMMENT : writes
    COMMUNITY_POST ||--o{ COMMUNITY_POST_LIKE : liked
    USER ||--o{ COMMUNITY_POST_LIKE : likes
    COMMUNITY_COMMENT ||--o{ COMMUNITY_COMMENT_LIKE : liked
    USER ||--o{ COMMUNITY_COMMENT_LIKE : likes
    COMMUNITY_POST ||--o{ SAVED_COMMUNITY_POST : saved
    USER ||--o{ SAVED_COMMUNITY_POST : bookmarks
    USER ||--o{ CHAT_SESSION : chats
    CHAT_SESSION ||--o{ CHAT_MESSAGE : contains
    COURSE ||--o{ CHAT_MESSAGE : recommends
    USER ||--o{ RECOMMENDATION_SESSION : requests
    RECOMMENDATION_SESSION ||--o{ RECOMMENDATION_ITEM : contains
    COURSE ||--o{ RECOMMENDATION_ITEM : ranked
```

## 설계 메모

| 항목 | 설명 |
| --- | --- |
| 기본 코스와 사용자 코스 분리 | DB fixture 기반 코스는 `Course`, 사용자가 만든 코스는 `CustomCourse`로 관리합니다. |
| 코스 삭제 보호 | 러닝 기록과 게시글의 코스 FK는 `SET_NULL`을 사용해 기록 자체를 보호합니다. |
| 러닝 기록 | GPS/시뮬레이션 모드를 `tracking_mode`로 구분하고 실제 이동 좌표는 `track_points`에 저장합니다. |
| 커뮤니티 최적화 | 게시글, 댓글, 좋아요, 저장 관계에 조회용 index를 추가했습니다. |
| 추천 세션 | 추천 결과를 `RecommendationSession`과 `RecommendationItem`에 저장해 다음 추천으로 넘기는 흐름을 지원합니다. |
| 챗봇 | 세션과 메시지를 분리하고, 메시지별 추천 코스를 선택적으로 연결합니다. |


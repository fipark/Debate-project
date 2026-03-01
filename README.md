# 🎙️ DebateLLM — AI 실시간 토론 플랫폼

> AI와 실시간으로 토론하고, 논리력·근거·전달력을 평가받는 웹 서비스

🔗 **Live Demo**: [https://llmdebate.vercel.app](https://llmdebate.vercel.app)

---

## 📌 프로젝트 소개

DebateLLM은 사용자가 AI 토론자와 **실시간으로 찬반 토론**을 진행하고, AI 심판이 **논리성·근거 제시·전달력(매너)** 3가지 항목으로 점수를 매기는 풀스택 웹 서비스입니다.

사회자 AI가 토론을 진행하고, 상대측 AI가 반론을 제시하며, 심판 AI가 실시간으로 점수를 평가합니다.

### 핵심 기능

- 🤖 **AI 실시간 토론** — LangGraph 기반 멀티 에이전트 (사회자 / 토론자 / 심판)
- 📊 **실시간 점수 평가** — 논리성, 근거 제시, 전달력 3항목 100점 만점
- 🔐 **소셜 로그인** — Google OAuth, Kakao OAuth
- 📧 **이메일 인증** — Gmail SMTP 인증코드 발송/검증
- 🏆 **리더보드** — 전체 유저 랭킹 시스템
- 🛡️ **관리자 기능** — 토론 주제 CRUD, 활성화/비활성화
- 🌙 **다크/라이트 모드** — 테마 전환 지원
- 📱 **반응형 UI** — 모바일 ~ 데스크탑 대응

---

## 🛠️ 기술 스택

### Frontend

| 기술 | 설명 |
|------|------|
| **Next.js 16** | App Router, Server Components |
| **TypeScript** | 타입 안정성 |
| **Tailwind CSS** | 유틸리티 기반 스타일링 |
| **shadcn/ui** | 재사용 가능한 UI 컴포넌트 |
| **Axios** | HTTP 클라이언트 |
| **WebSocket** | 실시간 토론 통신 |

### Backend

| 기술 | 설명 |
|------|------|
| **NestJS** | Node.js 프레임워크 |
| **TypeScript** | 타입 안정성 |
| **Prisma ORM** | 데이터베이스 ORM |
| **PostgreSQL** | 관계형 데이터베이스 (Supabase) |
| **JWT** | Access Token + Refresh Token 인증 |
| **Passport.js** | Google / Kakao OAuth 전략 |
| **Nodemailer** | Gmail SMTP 이메일 발송 |
| **WebSocket (ws)** | 실시간 양방향 통신 |

### AI Server(협업자)

| 기술 | 설명 |
|------|------|
| **LangGraph** | 멀티 에이전트 오케스트레이션 |
| **LLM** | 사회자 / 토론자 / 심판 역할 수행 |

### Infra / DevOps

| 기술 | 설명 |
|------|------|
| **Vercel** | 프론트엔드 배포 |
| **Render** | 백엔드 배포 |
| **Supabase** | PostgreSQL 호스팅 |
| **GitHub** | 소스 코드 관리 |

---

## 🏗️ 시스템 아키텍처

```
┌─────────────┐     HTTPS      ┌──────────────┐     WebSocket     ┌──────────────┐
│   Frontend   │ ◄────────────► │   Backend    │ ◄────────────────► │  AI Server   │
│  (Next.js)   │                │  (NestJS)    │                    │ (LangGraph)  │
│   Vercel     │                │   Render     │                    │              │
└─────────────┘                └──────┬───────┘                    └──────────────┘
                                      │
                                      │ Prisma ORM
                                      ▼
                               ┌──────────────┐
                               │  PostgreSQL   │
                               │  (Supabase)   │
                               └──────────────┘
```

---

## 🎮 주요 기능 상세

### 1. AI 실시간 토론

사용자가 주제를 선택하고 찬성/반대 입장을 고르면, AI 사회자가 토론을 진행합니다.

**토론 흐름:**
1. 사회자가 토론 시작 및 진행 멘트
2. 찬성 측 발언 (유저 또는 AI)
3. 사회자가 반대 측에게 발언권 부여
4. 반대 측 발언 (유저 또는 AI)
5. 여러 라운드 반복 후 토론 종료
6. AI 심판이 최종 평가 및 피드백 제공

### 2. 실시간 점수 시스템

AI 심판이 매 발언마다 3가지 항목으로 평가합니다.

| 항목 | 설명 |
|------|------|
| **논리성** | 주장의 논리적 구조와 일관성 |
| **근거 제시** | 구체적 사례, 데이터, 출처 활용 |
| **전달력(매너)** | 표현력, 설득력, 토론 매너 |

### 3. 인증 시스템

- **일반 회원가입**: 이메일 인증코드 → 비밀번호 설정 → 가입
- **소셜 로그인**: Google / Kakao OAuth 2.0
- **JWT 인증**: Access Token(15분) + Refresh Token(7일)

### 4. 보안

- **Rate Limiting**: Throttler로 API 요청 횟수 제한 (인증코드 발송 60초 3회)
- **Helmet**: HTTP 보안 헤더 자동 추가
- **bcrypt**: 비밀번호 해시 암호화
- **Refresh Token 해시 저장**: DB에 평문 토큰 미저장

---

## 📁 프로젝트 구조

### Frontend
```
debate-frontend/
├── app/
│   ├── (auth)/
│   │   ├── signin/          # 로그인 (소셜 로그인 포함)
│   │   ├── signin/callback/ # 소셜 로그인 콜백
│   │   └── signup/          # 회원가입 (이메일 인증)
│   ├── debate/
│   │   ├── [topicId]/       # 토론 채팅 화면
│   │   └── page.tsx         # 주제 선택
│   ├── leaderboard/         # 리더보드
│   ├── profile/             # 내 프로필
│   └── admin/topics/        # 관리자 주제 관리
├── components/
│   ├── ui/chat/             # 채팅 컴포넌트 (ChatRow, LiveScoreCard 등)
│   └── page-header.tsx      # 공통 반응형 헤더
├── lib/
│   ├── auth.ts              # 인증 유틸
│   ├── debate-ws/           # WebSocket 훅
│   └── chat/                # 채팅 헬퍼
└── hooks/                   # 커스텀 훅
```

### Backend
```
debate-backend/
├── src/
│   ├── auth/
│   │   ├── auth.controller.ts   # 인증 API (로그인, 소셜, 인증코드)
│   │   ├── auth.service.ts      # 인증 비즈니스 로직
│   │   └── dto/                 # 요청/응답 DTO
│   ├── common/
│   │   ├── guards/              # JWT Guard
│   │   └── strategies/          # Google, Kakao OAuth 전략
│   ├── debates/                 # 토론 세션 관리
│   ├── topics/                  # 토론 주제 API
│   ├── users/                   # 유저 / 리더보드
│   ├── admin/topics/            # 관리자 주제 CRUD
│   ├── mail/                    # Gmail SMTP 서비스
│   └── ws-proxy/                # WebSocket 프록시 (AI 서버 연결)
├── prisma/
│   └── schema.prisma            # DB 스키마
└── .env                         # 환경변수
```

---

## 🗄️ DB 스키마 (주요 모델)

```prisma
model User {
  id           Int       @id @default(autoincrement())
  email        String?   @unique
  name         String
  password     String?
  provider     String    @default("local")
  providerId   String?
  role         String    @default("user")
  hashedRT     String?
  sessions     Session[]

  @@unique([provider, providerId])
}

model Topic {
  id          Int       @id @default(autoincrement())
  title       String
  description String?
  isActive    Boolean   @default(true)
  sessions    Session[]
}

model Session {
  id              Int       @id @default(autoincrement())
  userId          Int
  topicId         Int
  side            String
  totalScore      Int?
  logicScore      Int?
  evidenceScore   Int?
  persuasionScore Int?
  feedback        String?
  messages        Message[]
}
```

---

## 🚀 로컬 실행 방법

### 사전 요구사항
- Node.js 18+
- PostgreSQL (또는 Supabase)
- Gmail 앱 비밀번호
- Google / Kakao OAuth 클라이언트 키

### 1. 백엔드

```bash
git clone https://github.com/fipark/debate-backend.git
cd debate-backend
npm install
```

`.env` 설정:
```env
DATABASE_URL=postgresql://...
AT_SECRET=your_access_token_secret
RT_SECRET=your_refresh_token_secret
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
GOOGLE_CALLBACK_URL=http://localhost:3001/auth/google/callback
KAKAO_CLIENT_ID=your_kakao_client_id
KAKAO_CALLBACK_URL=http://localhost:3001/auth/kakao/callback
FRONTEND_URL=http://localhost:3000
MAIL_USER=your_email@gmail.com
MAIL_PASS=your_app_password
```

```bash
npx prisma migrate dev
npm run start:dev
```

### 2. 프론트엔드

```bash
git clone https://github.com/fipark/debate-frontend.git
cd debate-frontend
npm install
```

`.env.local` 설정:
```env
NEXT_PUBLIC_API_URL=http://localhost:3001
```

```bash
npm run dev
```

---

## 📡 API 엔드포인트

### Auth
| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/auth/signup` | 회원가입 |
| POST | `/auth/signin` | 로그인 |
| POST | `/auth/send-code` | 이메일 인증코드 발송 |
| POST | `/auth/verify-code` | 인증코드 검증 |
| GET | `/auth/google` | Google 소셜 로그인 |
| GET | `/auth/kakao` | Kakao 소셜 로그인 |
| POST | `/auth/refresh` | 토큰 갱신 |
| POST | `/auth/logout` | 로그아웃 |
| GET | `/auth/me` | 내 정보 조회 |

### Topics
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/topics` | 주제 목록 조회 |
| GET | `/topics/:id` | 주제 상세 조회 |

### Debates
| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/debates` | 토론 세션 생성 |
| GET | `/debates/:id` | 세션 정보 조회 |
| GET | `/debates/:id/messages` | 메시지 목록 조회 |
| POST | `/debates/:id/messages` | 메시지 저장 |
| PATCH | `/debates/:id/end` | 토론 종료 |

### Users
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/users/me` | 내 정보 |
| GET | `/users/me/sessions` | 내 토론 기록 |
| GET | `/leaderboard` | 리더보드 |

### Admin
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/admin/topics` | 주제 관리 목록 |
| POST | `/admin/topics` | 주제 생성 |
| PATCH | `/admin/topics/:id` | 주제 수정 |
| PATCH | `/admin/topics/:id/toggle` | 활성화 토글 |

---

## 🔧 트러블슈팅

| 문제 | 원인 | 해결 |
|------|------|------|
| Render 서버 슬립 | 무료 플랜 15분 비활성 시 슬립 | 첫 요청 시 30초~1분 대기 |
| 카카오 이메일 미제공 | 개인 개발자 이메일 권한 없음 | providerId로 유저 식별 |
| passport-kakao 타입 에러 | @types 미제공 | require + 직접 타입 정의 |
| useSearchParams 빌드 에러 | Next.js SSR 제한 | Suspense 래핑 |
| AI 빈 응답 | tool 검색 시 빈 문자열 전송 | 빈 문자열 방어 코드 |

---

## 👨‍💻 개발자

**박준환**
- 연세대학교(신촌) 응용정보공학전공
- 풀스택 개발 (NestJS + Next.js) + postgresql(DB)

---

## 📄 라이선스

이 프로젝트는 개인 포트폴리오 목적으로 제작되었습니다.


## 🗺️ 향후 계획

- 🌍 글로벌 서비스 확장 (한중영 지원)
- 💳 Stripe 결제 시스템 (프리미엄 구독 + 토론 횟수 충전)
- ☁️ AWS 인프라 마이그레이션

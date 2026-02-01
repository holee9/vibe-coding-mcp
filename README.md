# vibe-coding-mcp

**MoAI-ADK (Multi-Agent Orchestrated AI Development Kit) 실행 환경**

Claude Code 기반의 다중 에이전트 코딩 자동화 시스템으로, Domain-Driven Design(DDD) 방법론과 SPEC-First 개발 워크플로우를 통해 고품질 소프트웨어 개발을 자동화합니다.

## 목차

- [핵심 기능](#핵심-기능)
- [빠른 시작](#빠른-시작)
- [프로젝트 구조](#프로젝트-구조)
- [개발 워크플로우](#개발-워크플로우)
- [에이전트 시스템](#에이전트-시스템)
- [기술 스택](#기술-스택)
- [문서](#문서)

---

## 핵심 기능

### 1. 다중 에이전트 시스템 (19개 에이전트)

- **Manager 에이전트 (7개)**: 워크플로우 조정, SPEC 관리, 품질 검증
- **Expert 에이전트 (8개)**: 백엔드, 프론트엔드, 보안, DevOps 등 전문 분야
- **Builder 에이전트 (4개)**: 에이전트, 명령어, 스킬, 플러그인 생성

### 2. SPEC-First DDD 워크플로우

1. **Plan** (`/moai:1-plan`): EARS 형식으로 요구사항 명세화
2. **Run** (`/moai:2-run`): DDD 방법론으로 구현 (ANALYZE-PRESERVE-IMPROVE)
3. **Sync** (`/moai:3-sync`): 자동 문서화 및 품질 검증

### 3. 점진적 공개 (Progressive Disclosure)

- 3단계 스킬 로딩으로 초기 토큰 로드 67% 감소
- 필요할 때만 전체 스킬 콘텐츠 로드
- 200K 토큰 예산 내에서 대규모 프로젝트 처리

### 4. TRUST 5 품질 프레임워크

- **Tested**: 85% 커버리지 목표
- **Readable**: 명확한 명명 규칙과 일관된 스타일
- **Unified**: 자동 포맷팅과 임포트 정렬
- **Secured**: OWASP 준수 및 보안 스캔
- **Trackable**: 구조화된 커밋 메시지와 변경 추적

---

## 빠른 시작

### 전제 조건

- Claude Code 설치
- Git
- Node.js (MCP 서버 실행용)

### 설치

1. 저장소 복제

```bash
git clone https://github.com/holee9/vibe-coding-mcp.git
cd vibe-coding-mcp
```

2. 프로젝트 초기화

```bash
/moai:0-project
```

3. 첫 번째 SPEC 작성

```bash
/moai:1-plan "사용자 인증 기능 구현"
```

4. DDD로 구현

```bash
/moai:2-run SPEC-001
```

5. 문서화

```bash
/moai:3-sync SPEC-001
```

---

## 프로젝트 구조

```
vibe-coding-mcp/
│
├── .claude/                          # Claude Code 설정
│   ├── agents/                       # 에이전트 정의 (19개)
│   │   └── moai/
│   │       ├── manager-*             # Manager 에이전트 (7개)
│   │       ├── expert-*              # Expert 에이전트 (8개)
│   │       └── builder-*             # Builder 에이전트 (4개)
│   │
│   ├── commands/                     # 슬래시 명령어
│   │   └── moai/
│   │       ├── 0-project.md          # 프로젝트 초기화
│   │       ├── 1-plan.md             # SPEC 명세 생성
│   │       ├── 2-run.md              # DDD 구현 실행
│   │       ├── 3-sync.md             # 문서 동기화
│   │       └── ...
│   │
│   ├── skills/                       # 재사용 가능 스킬 (45개)
│   │   ├── moai-foundation-*         # 파운데이션 스킬
│   │   ├── moai-domain-*            # 도메인 스킬
│   │   ├── moai-lang-*              # 언어 스킬
│   │   ├── moai-platform-*          # 플랫폼 스킬
│   │   └── moai-workflow-*          # 워크플로우 스킬
│   │
│   ├── hooks/                        # 훅 시스템
│   └── output-styles/                # 출력 스타일
│
├── .moai/                            # MoAI 프로젝트 설정
│   ├── config/                       # 설정 파일
│   │   ├── sections/
│   │   │   ├── user.yaml            # 사용자 정보
│   │   │   ├── language.yaml        # 언어 설정
│   │   │   ├── project.yaml         # 프로젝트 정보
│   │   │   ├── quality.yaml         # 품질 설정
│   │   │   └── llm.yaml             # LLM 라우팅
│   │
│   ├── specs/                        # SPEC 문서 저장소
│   │   └── SPEC-XXX/
│   │       └── spec.md
│   │
│   ├── project/                      # 프로젝트 메타데이터
│   │   ├── product.md               # 제품 개요
│   │   ├── structure.md             # 아키텍처 문서
│   │   └── tech.md                  # 기술 스택
│   │
│   └── reports/                      # 품질 리포트
│
├── .mcp.json                         # MCP 서버 설정
├── CLAUDE.md                         # Claude Code 지침
├── .gitignore                        # Git 무시 파일
└── README.md                         # 이 파일
```

---

## 개발 워크플로우

### 1단계: SPEC 명세 (`/moai:1-plan`)

```bash
/moai:1-plan "사용자 인증 기능 구현"
```

- manager-spec가 EARS 형식으로 요구사항 명세 생성
- Context7로 베스트 프랙티스 검색
- Sequential Thinking으로 복잡한 문제 분해
- `.moai/specs/SPEC-XXX/spec.md`에 명세 저장

### 2단계: 전략 수립 (manager-strategy)

- 시스템 아키텍처 설계
- 기술 스택 선택
- 트레이드오프 분석

### 3단계: 구현 실행 (`/moai:2-run`)

```bash
/moai:2-run SPEC-001
```

- **ANALYZE**: 요구사항 분석 및 설계
- **PRESERVE**: 기존 동작 보존을 위한 특성화 테스트
- **IMPROVE**: 점진적 기능 개선 및 구현
- expert-backend, expert-frontend 등 전문 에이전트가 병렬로 작업

### 4단계: 문서화 (`/moai:3-sync`)

```bash
/moai:3-sync SPEC-001
```

- manager-docs가 Nextra 기반 문서 자동 생성
- API 문서, 아키텍처 다이어그램, 사용자 가이드
- TRUST 5 품질 검증

---

## 에이전트 시스템

### Manager 에이전트 (7개)

| 에이전트 | 역할 | 주요 도구 |
|---------|------|-----------|
| `manager-spec` | EARS 형식 요구사항 명세 생성 | Task, Read, Write |
| `manager-ddd` | DDD 구현 (ANALYZE-PRESERVE-IMPROVE) | Task, Bash, Edit |
| `manager-docs` | Nextra 기반 문서 생성 | Task, Read, Write |
| `manager-quality` | TRUST 5 품질 검증 | Task, Bash, Grep |
| `manager-project` | 프로젝트 구성 및 초기화 | Task, Bash, Write |
| `manager-strategy` | 시스템 설계 및 아키텍처 | Task, Sequential Thinking |
| `manager-git` | Git 작업 및 브랜칭 전략 | Task, Bash, Git |

### Expert 에이전트 (8개)

| 에이전트 | 전문 분야 | 핵심 기능 |
|---------|----------|----------|
| `expert-backend` | 백엔드 개발 | API, 서버 로직, 데이터베이스 |
| `expert-frontend` | 프론트엔드 개발 | React, UI, 클라이언트 코드 |
| `expert-security` | 보안 | 취약점 평가, OWASP 준수 |
| `expert-devops` | DevOps | CI/CD, 인프라, 배포 |
| `expert-performance` | 성능 | 프로파일링, 병목 분석 |
| `expert-debug` | 디버깅 | 오류 분석, 문제 해결 |
| `expert-testing` | 테스트 | 테스트 생성, 커버리지 |
| `expert-refactoring` | 리팩토링 | 코드 정리, 아키텍처 개선 |

### Builder 에이전트 (4개)

| 에이전트 | 생성 대상 |
|---------|----------|
| `builder-agent` | 새로운 에이전트 정의 |
| `builder-command` | 슬래시 명령어 |
| `builder-skill` | 스킬 모듈 |
| `builder-plugin` | 플러그인 번들 |

---

## 기술 스택

### 코어

- **Claude Code**: 메인 오케스트레이션
- **MoAI-ADK**: 멀티-에이전트 프레임워크
- **MCP (Model Context Protocol)**: 외부 서비스 통합

### 언어 및 프레임워크

- **YAML**: 설정 파일
- **Python**: Hook 시스템
- **Markdown**: 문서화

### MCP 서버

- **Context7**: 공식 라이브러리 문서 검색
- **Sequential Thinking**: 복잡한 문제 단계별 분해

### 개발 도구

- **Git**: 버전 관리
- **AST-Grep**: 구조적 코드 분석
- **WebFetch/WebSearch**: 외부 리소스 접근

---

## 문서

자세한 문서는 `.moai/project/` 디렉토리를 참조하세요:

- **[product.md](.moai/project/product.md)**: 제품 개요, 타겟 사용자, 핵심 기능, 사용 사례
- **[structure.md](.moai/project/structure.md)**: 아키텍처, 디렉토리 구조, 모듈 조직
- **[tech.md](.moai/project/tech.md)**: 기술 스택, 개발 환경, 빌드 및 배포
- **[infrastructure-plan.md](.moai/project/infrastructure-plan.md)**: 인프라 구축 계획 (4-Node 클러스터, AI 엔진, CI/CD)

### 인프라 구축 현황

하드웨어 인프라 구축을 위한 상세 가이드가 포함되어 있습니다:

**현재 완료된 구성 요소:**
- ✅ Raspberry Pi 5: n8n, Cloudflare Tunnel, Issue Form, Email Notification (외부 도메인 접속 완료)
- ✅ Synology NAS: Gitea, Redmine, PostgreSQL

**진행 예정인 구성 요소:**
- ⏳ Raspberry Pi 5: Portainer, Nginx Proxy Manager
- ⏳ ASUS GX10: GLM-4.7 (SGLang), LangGraph 에이전트
- ⏳ Jetson Nano: CI/CD 파이프라인

**상세 계획:** [infrastructure-plan.md](.moai/project/infrastructure-plan.md) 참조

---

## 사용 사례

### 1. 신규 프로젝트 개발

```bash
# 프로젝트 초기화
/moai:0-project

# 핵심 기능 명세화
/moai:1-plan "사용자 인증 API 개발"

# 백엔드 API 구현
/moai:2-run SPEC-001

# 프론트엔드 UI 개발
/moai:2-run SPEC-002

# 문서 자동 생성
/moai:3-sync SPEC-001
```

### 2. 레거시 코드 현대화

```bash
# 현재 상태 분석 및 명세화
/moai:1-plan "마이크로서비스로 전환"

# 점진적 마이크로서비스 추출
/moai:2-run SPEC-001

# 품질 검증
/moai:3-sync SPEC-001
```

### 3. API 개발 및 문서화

```bash
# API 명세
/moai:1-plan "RESTful API 개발"

# FastAPI 엔드포인트 구현
/moai:2-run SPEC-001

# OpenAPI 문서 생성
/moai:3-sync SPEC-001
```

---

## 라이선스

이 프로젝트는 오픈 소스이며 커뮤니티 기여를 환영합니다.

---

**GitHub**: https://github.com/holee9/vibe-coding-mcp

*마지막 업데이트: 2026-01-22*

# vibe-coding-mcp 아키텍처 문서

## 시스템 개요

vibe-coding-mcp는 MoAI-ADK(Multi-Agent Orchestrated AI Development Kit) 프레임워크를 구현하는 다중 에이전트 시스템입니다. Claude Code의 오케스트레이션 능력과 전문화된 하위 에이전트들의 협력을 통해 자동화된 고품질 소프트웨어 개발을 가능하게 합니다.

### 핵심 아키텍처 원칙

1. **에이전트 분리**: 각 에이전트는 독립적인 200K 토큰 컨텍스트를 가지며 상호 간섭 없이 작업
2. **점진적 공개**: 3단계 스킬 로딩으로 토큰 효율성 극대화
3. **모듈화**: 재사용 가능한 스킬과 명령어로 확장성 확보
4. **품질 우선**: TRUST 5 프레임워크로 자동화된 품질 보장

---

## 디렉토리 구조

```
vibe-coding-mcp/
│
├── .claude/                          # Claude Code 설정
│   ├── agents/                       # 에이전트 정의 (19개)
│   │   └── moai/                     # MoAI 전용 에이전트
│   │       ├── manager-*             # Manager 에이전트 (7개)
│   │       ├── expert-*              # Expert 에이전트 (8개)
│   │       └── builder-*             # Builder 에이전트 (4개)
│   │
│   ├── commands/                     # 슬래시 명령어
│   │   └── moai/                     # MoAI 명령어
│   │       ├── 0-project.md          # 프로젝트 초기화
│   │       ├── 1-plan.md             # SPEC 명세 생성
│   │       ├── 2-run.md              # DDD 구현 실행
│   │       ├── 3-sync.md             # 문서 동기화
│   │       ├── 9-feedback.md         # 피드백 제출
│   │       ├── alfred.md             # Alfred 도움말
│   │       ├── fix.md                # 빠른 수정
│   │       └── loop.md               # 반복 작업
│   │
│   ├── skills/                       # 재사용 가능 스킬 (45개)
│   │   ├── moai-foundation-*         # 파운데이션 스킬
│   │   ├── moai-domain-*            # 도메인 스킬
│   │   ├── moai-lang-*              # 언어 스킬
│   │   ├── moai-library-*           # 라이브러리 스킬
│   │   ├── moai-platform-*          # 플랫폼 스킬
│   │   ├── moai-tool-*              # 도구 스킬
│   │   └── moai-workflow-*          # 워크플로우 스킬
│   │
│   ├── hooks/                        # 훅 시스템
│   │   └── moai/
│   │       └── lib/                  # 훅 라이브러리
│   │
│   └── output-styles/                # 출력 스타일
│       └── moai/
│           ├── alfred.md             # Alfred 스타일
│           ├── r2d2.md               # R2D2 스타일
│           └── yoda.md               # Yoda 스타일
│
├── .moai/                            # MoAI 프로젝트 설정
│   ├── config/                       # 설정 파일
│   │   ├── sections/                 # 섹션별 설정
│   │   │   ├── user.yaml            # 사용자 정보
│   │   │   ├── language.yaml        # 언어 설정
│   │   │   ├── project.yaml         # 프로젝트 정보
│   │   │   ├── quality.yaml         # 품질 설정
│   │   │   ├── llm.yaml             # LLM 라우팅
│   │   │   ├── git-strategy.yaml    # Git 전략
│   │   │   ├── pricing.yaml         # 가격 정책
│   │   │   └── system.yaml          # 시스템 설정
│   │   │
│   │   ├── config.yaml              # 메인 설정
│   │   └── multilingual-triggers.yaml # 다국어 트리거
│   │
│   ├── specs/                        # SPEC 문서 저장소
│   │   └── SPEC-XXX/                # 개별 SPEC
│   │       └── spec.md              # 명세 문서
│   │
│   ├── project/                      # 프로젝트 메타데이터
│   │   ├── product.md               # 제품 개요
│   │   ├── structure.md             # 아키텍처 (이 파일)
│   │   └── tech.md                  # 기술 스택
│   │
│   ├── reports/                      # 품질 리포트
│   ├── memory/                       # 프로젝트 메모리
│   │
│   └── llm-configs/                  # LLM 설정
│       └── glm.json                 # GLM 설정 템플릿
│
├── .mcp.json                         # MCP 서버 설정
├── CLAUDE.md                         # Claude Code 지침
├── .gitignore                        # Git 무시 파일
└── README.md                         # 프로젝트 README
```

---

## 주요 디렉토리 상세

### 1. `.claude/agents/` - 에이전트 정의

**목적**: 각 에이전트의 시스템 프롬프트, 도구 접근 권한, 모델 사양을 정의

**Manager 에이전트 (7개)**
| 에이전트 | 역할 | 주요 도구 |
|---------|------|-----------|
| `manager-spec` | EARS 형식 요구사항 명세 생성 | Task, AskUserQuestion, Read, Write |
| `manager-ddd` | DDD 구현 (ANALYZE-PRESERVE-IMPROVE) | Task, Bash, Read, Write, Edit |
| `manager-docs` | Nextra 기반 문서 생성 | Task, Read, Write, Glob, Grep |
| `manager-quality` | TRUST 5 품질 검증 | Task, Bash, Grep, Read |
| `manager-project` | 프로젝트 구성 및 초기화 | Task, Bash, Write |
| `manager-strategy` | 시스템 설계 및 아키텍처 | Task, Read, Sequential Thinking |
| `manager-git` | Git 작업 및 브랜칭 전략 | Task, Bash, Git |

**Expert 에이전트 (8개)**
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

**Builder 에이전트 (4개)**
| 에이전트 | 생성 대상 |
|---------|----------|
| `builder-agent` | 새로운 에이전트 정의 |
| `builder-command` | 슬래시 명령어 |
| `builder-skill` | 스킬 모듈 |
| `builder-plugin` | 플러그인 번들 |

### 2. `.claude/commands/` - 슬래시 명령어

**목적**: 사용자가 직접 호출하는 명령어를 정의

**Type A: 워크플로우 명령**
- `/moai:0-project`: 프로젝트 초기화 및 구성
- `/moai:1-plan`: SPEC 문서 생성 (manager-spec)
- `/moai:2-run`: DDD 구현 실행 (manager-ddd)
- `/moai:3-sync`: 문서 동기화 (manager-docs)

**Type B: 유틸리티 명령**
- `/moai:alfred`: Alfred 도움말 및 시스템 정보
- `/moai:fix`: 빠른 수정 및 자동화
- `/moai:loop`: 반복 작업 최적화

**Type C: 피드백 명령**
- `/moai:9-feedback`: GitHub 이슈 생성 (버그/제안/질문)

### 3. `.claude/skills/` - 재사용 가능 스킬

**목적**: 에이전트가 필요시 로드하는 전문 지식 모듈

**파운데이션 스킬**
- `moai-foundation-claude`: Claude Code 작성 키트
- `moai-foundation-core`: TRUST 5, SPEC-First DDD, 위임 패턴
- `moai-foundation-context`: 컨텍스트 관리

**도메인 스킬**
- `moai-domain-backend`: 백엔드 패턴 (FastAPI, Express)
- `moai-domain-database`: 데이터베이스 (PostgreSQL, MongoDB)
- `moai-domain-uiux`: UI/UX (Figma, React)

**언어 스킬**
- `moai-lang-python`: Python 패턴, 타이핑, 테스트
- `moai-lang-typescript`: TypeScript/JavaScript 패턴

**라이브러리 스킬**
- `moai-library-mermaid`: 다이어그램 생성
- `moai-library-nextra`: Nextra 문서 사이트

**워크플로우 스킬**
- `moai-workflow-docs`: 문서 생성 워크플로우
- `moai-workflow-project`: 프로젝트 관리

### 4. `.moai/config/` - 설정 시스템

**목적**: 모듈화된 설정으로 프로젝트 동작 제어

**주요 설정 파일**

`user.yaml`: 사용자 정보
```yaml
user:
  name: holee
```

`language.yaml`: 언어 설정
```yaml
language:
  conversation_language: ko          # 사용자 대화 언어
  agent_prompt_language: en          # 에이전트 프롬프트 언어
  git_commit_messages: ko            # Git 커밋 메시지 언어
  code_comments: en                  # 코드 주석 언어
  documentation: ko                  # 문서 언어
```

`project.yaml`: 프로젝트 정보
```yaml
project:
  name: vibe-coding-mcp
  description: ''
  type: ''
  locale: '{{LOCALE}}'
  created_at: '2026-01-22T09:51:49.331647Z'
  initialized: 'true'
```

`quality.yaml`: 품질 설정
```yaml
constitution:
  development_mode: ddd              # DDD 방법론
  enforce_quality: true              # 품질 강제
  test_coverage_target: 85           # 85% 커버리지 목표
```

`llm.yaml`: LLM 라우팅
```yaml
llm:
  mode: claude-only                  # claude-only / mashup / glm-only
  glm_env_var: GLM_API_KEY
  glm:
    base_url: "https://open.bigmodel.cn/api/anthropic"
    models:
      haiku: "glm-4.7-flashx"
      sonnet: "glm-4.7"
```

### 5. `.moai/specs/` - SPEC 저장소

**목적**: EARS 형식의 요구사항 명세 문서 저장

**구조**
```
specs/
└── SPEC-001/
    └── spec.md                     # EARS 형식 명세
```

**SPEC 문서 형식**
```markdown
# SPEC-XXX: 제목

## 배경
프로젝트 배경 및 동기

## 요구사항
### Ubiquitous (항상 활성)
- 시스템은 [기능]을 제공해야 한다

### Event-Driven (이벤트 기반)
- [조건] 시 [기능]을 실행해야 한다

### State-Driven (상태 기반)
- [조건] 동안 [기능]을 유지해야 한다

### Unwanted (금지)
- 시스템은 [행위]를 해서는 안 된다

### Optional (선택)
- 가능하다면 [기능]을 제공해야 한다
```

### 6. `.moai/project/` - 프로젝트 메타데이터

**목적**: 프로젝트 문서 및 메타데이터 저장

**파일**
- `product.md`: 제품 개요 (사용자, 혜택, 사용 사례)
- `structure.md`: 아키텍처 문서 (디렉토리 구조, 모듈 조직)
- `tech.md`: 기술 스택 (프레임워크, 라이브러리, 도구)

### 7. `.mcp.json` - MCP 서버 설정

**목적**: Model Context Protocol 서버 구성

```json
{
  "$schema": "https://raw.githubusercontent.com/anthropics/claude-code/main/.mcp.schema.json",
  "mcpServers": {
    "context7": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@upstash/context7-mcp@latest"]
    },
    "sequential-thinking": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  },
  "staggeredStartup": {
    "enabled": true,
    "delayMs": 500,
    "connectionTimeout": 15000
  }
}
```

**MCP 서버**
- **Context7**: 공식 라이브러리 문서 검색
- **Sequential Thinking**: 복잡한 문제 단계별 분해

---

## 모듈 조직

### 에이전트 시스템 구조

```
Alfred (오케스트레이터)
    │
    ├── 7 Manager Agents (워크플로우 조정)
    │   ├── manager-spec     → 요구사항 분석 및 명세
    │   ├── manager-strategy → 시스템 설계
    │   ├── manager-ddd      → DDD 구현
    │   ├── manager-quality  → 품질 검증
    │   ├── manager-docs     → 문서 생성
    │   ├── manager-project  → 프로젝트 관리
    │   └── manager-git      → Git 관리
    │
    ├── 8 Expert Agents (전문 작업 실행)
    │   ├── expert-backend    → 백엔드 개발
    │   ├── expert-frontend   → 프론트엔드 개발
    │   ├── expert-security   → 보안 분석
    │   ├── expert-devops     → DevOps
    │   ├── expert-performance → 성능 최적화
    │   ├── expert-debug      → 디버깅
    │   ├── expert-testing    → 테스트
    │   └── expert-refactoring → 리팩토링
    │
    └── 4 Builder Agents (시스템 확장)
        ├── builder-agent    → 에이전트 생성
        ├── builder-command  → 명령어 생성
        ├── builder-skill    → 스킬 생성
        └── builder-plugin   → 플러그인 생성
```

### 스킬 시스템 구조

```
45 재사용 가능 스킬
    │
    ├── Foundation (3) - 핵심 원칙
    │   ├── moai-foundation-claude  (Claude Code 패턴)
    │   ├── moai-foundation-core   (TRUST 5, DDD)
    │   └── moai-foundation-context (컨텍스트)
    │
    ├── Domain (3) - 도메인 전문성
    │   ├── moai-domain-backend    (백엔드)
    │   ├── moai-domain-database   (데이터베이스)
    │   └── moai-domain-uiux       (UI/UX)
    │
    ├── Language (2) - 언어별 패턴
    │   ├── moai-lang-python       (Python)
    │   └── moai-lang-typescript   (TypeScript)
    │
    ├── Library (6) - 라이브러리 전문성
    │   ├── moai-library-mermaid   (다이어그램)
    │   ├── moai-library-nextra    (문서 사이트)
    │   └── ... (4 more)
    │
    ├── Platform (8) - 플랫폼 통합
    │   ├── moai-platform-github   (GitHub)
    │   ├── moai-platform-vercel   (Vercel)
    │   └── ... (6 more)
    │
    ├── Tool (4) - 도구 사용법
    │   ├── moai-tool-ast-grep     (AST-Grep)
    │   └── ... (3 more)
    │
    └── Workflow (19) - 워크플로우 패턴
        ├── moai-workflow-docs     (문서화)
        ├── moai-workflow-project  (프로젝트)
        └── ... (17 more)
```

### 워크플로우 실행 흐름

```
사용자 요청
    ↓
Alfred (분석 및 라우팅)
    ↓
[1단계] SPEC 생성 (/moai:1-plan)
    ├── manager-spec (EARS 명세)
    ├── Context7 (베스트 프랙티스 검색)
    └── Sequential Thinking (복잡한 분해)
    ↓
[2단계] 전략 수립 (manager-strategy)
    ├── 시스템 아키텍처 설계
    ├── 기술 스택 선택
    └── 트레이드오프 분석
    ↓
[3단계] 구현 실행 (/moai:2-run)
    ├── Parallel (독립 작업)
    │   ├── expert-backend (API)
    │   └── expert-frontend (UI)
    │
    └── Sequential (의존 작업)
        ├── manager-ddd (DDD 구현)
        ├── expert-testing (테스트)
        └── manager-quality (품질 검증)
    ↓
[4단계] 문서화 (/moai:3-sync)
    ├── manager-docs (Nextra 문서)
    ├── moai-library-mermaid (다이어그램)
    └── 마크다운/Mermaid 통합
    ↓
결물 통합 및 보고
```

---

## 에이전트-스킬 통합

### 스킬 로딩 메커니즘

**점진적 공개 (3단계)**

1. **레벨 1: 메타데이터** (시작 시)
   - SKILL.md YAML frontmatter만 로드
   - 약 100 토큰/스킬
   - 모든 에이전트 프론트미스에 나열된 스킬 자동 로드

2. **레벨 2: 스킬 본문** (트리거 시)
   - SKILL.md 전체 내용 로드
   - 약 5K 토큰/스킬
   - 키워드, 단계, 에이전트, 언어로 트리거

3. **레벨 3**: 참조 파일** (필요 시)
   - reference.md, modules/, examples/ 로드
   - Claude가 필요시 결정
   - 무제한 확장 가능

**트리거 조건 예시**

`moai-foundation-core` 스킬:
- 키워드: "TRUST 5", "SPEC", "DDD", "quality"
- 단계: 1-plan, 2-run, 3-sync
- 에이전트: manager-*, expert-*
- 언어: en, ko

`moai-library-mermaid` 스킬:
- 키워드: "diagram", "flowchart", "mermaid"
- 단계: 3-sync (문서화)
- 에이전트: manager-docs, design-uiux
- 언어: en

---

## 데이터 흐름

### 1. 사용자 요청 처리

```
사용자 입력
    ↓
AskUserQuestion (필요시 명확화)
    ↓
Alfred 분석 (요청 유형, 복잡성, 도메인)
    ↓
에이전트 선택 및 위임
```

### 2. 병렬 작업 분해

```
복잡한 작업 감지
    ↓
도메인별 하위 작업 식별
    ↓
에이전트 매핑
    ↓
병렬 실행 (최대 10개 에이전트)
    ↓
결과 통합
```

### 3. 품질 검증

```
코드 생성
    ↓
TRUST 5 검증 (manager-quality)
    ├── Tested: 테스트 커버리지 확인
    ├── Readable: 린터 검사
    ├── Unified: 포매터 실행
    ├── Secured: 보안 스캔
    └── Trackable: 커밋 메시지 검증
    ↓
피드백 및 수정 루프
    ↓
품질 리포트 생성
```

---

## 확장 포인트

### 1. 새로운 에이전트 추가

```bash
# builder-agent 사용
Use the builder-agent subagent to create a new expert-ml agent
for machine learning tasks with scikit-learn and TensorFlow integration.
```

### 2. 새로운 스킬 추가

```bash
# builder-skill 사용
Use the builder-skill subagent to create a moai-lang-rust skill
for Rust development patterns with ownership and borrowing concepts.
```

### 3. 새로운 명령어 추가

```bash
# builder-command 사용
Use the builder-command subagent to create a /moai:4-deploy command
for automated deployment to Vercel with environment variable management.
```

### 4. 새로운 플러그인 추가

```bash
# builder-plugin 사용
Use the builder-plugin subagent to create a moai-aws-plugin
for AWS Lambda, API Gateway, and DynamoDB integration.
```

---

## 보안 아키텍처

### 1. 샌드박싱

**파일시스템 격리**
- 쓰기 작업은 cwd로 제한
- 명시적인 경로만 허용

**네트워크 제한**
- 도메인 화이트리스트
- 프록시 기반 트래픽 제어

### 2. 접근 제어

**IAM(IAM 스킬)**
- 역할 기반 접근 제어
- 최소 권한 원칙

**Hooks**
- PreToolUse: 작업 전 검증
- PostToolUse: 작업 후 로깅

### 3. 감사 및 추적

**Git 추적**
- 모든 변경 사항 커밋
- 구조화된 커밋 메시지

**리포트 생성**
- 품질 리포트 자동 생성
- 이슈 추적 및 관리

---

*마지막 업데이트: 2026-01-22*
*문서 버전: 1.0.0*

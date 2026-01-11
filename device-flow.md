flowchart LR
    %% Lanes (역할 그룹)
    subgraph DEV[Developer & IDE]
        DEV_USER(Developer)
        VSC(VS Code
+ Claude/Copilot)
    end

    subgraph ORCH[Orchestrator & AI Planner]
        UI(Dev Portal / Form
(명령 폼/프롬프트))
        N8N(n8n Orchestrator
(조정자))
        AI_PLAN(AI Agent
(계획·요약 생성))
    end

    subgraph COLLAB[Collaboration & Records]
        GIT(Gitea
(코드/리포트 이력))
        RM(Redmine
(요구사항/진행))
        NAS(NAS
(이미지/Bitstream/로그))
    end

    subgraph EXEC[Executors (Build & Compute)]
        CI(CI Server/Runner
(작업 분배자))
        YOCTO_PC(PC A
Yocto 빌드 Worker)
        FPGA_PC(PC B
FPGA/Questa Worker)
        GX10(ASUS GX10
AI/분석 Worker)
    end

    subgraph TARGET[Targets & Edge]
        IMX(i.MX 8M Plus EVK
(실제 타깃))
        FPGA(Xilinx FPGA Board
(실제 타깃))
        JETSON(Jetson Orin Nano
(레퍼런스/엣지))
    end

    %% 1. 요구사항 흐름 (누가 누구에게 요청?)
    DEV_USER -->|기능/작업 요청| UI
    DEV_USER -->|코드 작성/리뷰| VSC

    %% 2. 계획·분해 (AI가 계획 세우고 n8n이 조정)
    UI -->|프롬프트/폼 데이터| N8N
    N8N -->|요청 전달| AI_PLAN
    AI_PLAN -->|태스크/체크리스트/스크립트 초안| N8N

    %% 3. 협업 도구와의 관계 (기록/이슈)
    N8N -->|이슈/상태 생성·갱신| RM
    N8N -->|브랜치/파일 생성·업데이트| GIT
    VSC -->|코드/리포트 커밋| GIT

    %% 4. 실행자에게 일 시키기 (CI + Worker들)
    N8N -->|파이프라인 트리거| CI
    CI -->|Yocto 빌드 Job| YOCTO_PC
    CI -->|FPGA/Questa Job| FPGA_PC
    CI -->|AI 분석/리포트 Job| GX10

    %% 5. 실행 결과 공유 (NAS/협업 툴로)
    YOCTO_PC -->|이미지/로그 업로드| NAS
    FPGA_PC -->|Bitstream/로그 업로드| NAS
    GX10 -->|모델/분석 리포트| NAS

    CI -->|빌드/테스트 결과 요약| RM
    CI -->|상태/결과 메타데이터| N8N

    N8N -->|진행/완료 리포트 메일·알림| DEV_USER

    %% 6. 타깃과의 협업 (배포/검증)
    YOCTO_PC -->|SD 이미지/네트워크 배포| IMX
    FPGA_PC -->|Bitstream/JTAG 배포| FPGA
    CI -->|엣지 테스트 Job| JETSON

    IMX -->|실행 로그/테스트 결과| NAS
    FPGA -->|실행 로그/측정 데이터| NAS
    JETSON -->|런타임 데이터/로그| NAS

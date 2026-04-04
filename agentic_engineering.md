좋아요! Agentic Engineering은 Harness Engineering보다 한 단계 위의 개념으로, **에이전트가 실제로 자율적으로 계획하고 실행하는 시스템 전체를 설계하는 것**입니다. 전체 그림부터 잡아드릴게요.

---

## 🧠 Agentic Engineering의 핵심 철학

Anthropic이 수십 개 팀과 함께 일하면서 얻은 교훈은 명확합니다. 가장 성공한 구현들은 복잡한 프레임워크가 아니라 **단순하고 조합 가능한(composable) 패턴**을 사용했습니다. 핵심 원칙은: 단순성을 유지하고, 복잡도는 측정 가능하게 개선될 때만 추가하라는 것입니다.

---

## 🗺️ Agentic System의 전체 구조

```
┌─────────────────────────────────────────────────────────────┐
│                   AGENTIC SYSTEM                            │
│                                                             │
│   HUMAN                                                     │
│   (Goal 정의 + 최종 승인)                                    │
│      │                                                      │
│      ▼                                                      │
│   ORCHESTRATOR (메인 에이전트)                               │
│   ┌──────────────────────────────┐                          │
│   │  Plan → Delegate → Aggregate │                          │
│   └──────────┬───────────────────┘                          │
│              │                                              │
│    ┌─────────┼─────────┐                                   │
│    ▼         ▼         ▼                                    │
│  Planner  Worker   Reviewer   ← Subagents                   │
│  Agent    Agents   Agent                                    │
│    │         │         │                                    │
│    └─────────┴─────────┘                                    │
│              │                                              │
│         TOOLS / MCP                                         │
│   (DB, API, Browser, Files, Git)                            │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 5가지 Agent Primitive (기본 구성 요소)

Claude Code가 제공하는 에이전트 기본 요소들입니다:

**Subagents** — Claude가 독립적인 작업을 위한 자식 에이전트를 생성합니다. 메인 에이전트는 조율·위임·결과 수집만 합니다. 예를 들어 50개 파일 리팩토링을 하나의 에이전트가 순차 처리하는 대신, 10개의 서브에이전트가 각 모듈을 병렬로 처리합니다.

**MCP Servers** — 데이터베이스, API, 파일 시스템, 브라우저 등 외부 도구와 데이터 소스에 Claude를 연결합니다. 코드 작성을 넘어서 DB 쿼리, Jira 티켓 읽기, 배포 상태 확인까지 가능하게 합니다.

**Worktrees** — Git worktree로 여러 에이전트가 격리된 저장소 사본에서 동시에 작업합니다. 에이전트 간 병렬 작업을 위해 핵심적입니다.

**Hooks** — 도구 호출 전, 응답 후, 에러 발생 시 등 Claude 라이프사이클의 특정 시점에 실행되는 커스텀 스크립트입니다. Claude의 핵심 로직을 수정하지 않고 가드레일, 로깅, 커스텀 동작을 추가합니다.

**Agent Teams** — 현재 리서치 프리뷰 단계로, 공유 메모리를 통해 여러 Claude 인스턴스가 협업합니다.

---

## 🏗️ 실제 프로덕션 아키텍처 패턴

실제 프로덕션에서 사용하는 에이전트 시스템 구조입니다:

```
orchestrator (메인 Claude Code 인스턴스)
├── planner subagent     (spec 읽고 구현 계획 생성)
├── implementer subagents (모듈/기능별, worktree 사용)
│   ├── module-a (worktree branch: agent/module-a)
│   ├── module-b (worktree branch: agent/module-b)
│   └── module-c (worktree branch: agent/module-c)
├── reviewer subagent    (구현 후 품질 검토)
└── integrator subagent  (브랜치 병합, 테스트 실행)
```

오케스트레이터는 코드를 직접 쓰지 않습니다. 워크플로우만 관리합니다. 각 구현자 에이전트는 독립된 worktree, 계획의 해당 섹션, spec과 프로젝트 가드레일(CLAUDE.md), 자신만의 신선한 컨텍스트를 갖습니다.

---

## 📋 Workflow 패턴 4가지 (Anthropic 공식)

Anthropic이 정의한 핵심 워크플로우 패턴들입니다:

1. **Prompt Chaining** — 복잡한 태스크를 순차적인 단계로 분해, 이전 단계 출력이 다음 입력이 됨
2. **Routing** — 쉬운/일반적인 질문은 Claude Haiku처럼 작고 저렴한 모델로, 어렵거나 이례적인 질문은 더 강력한 Claude Sonnet으로 라우팅하여 최적 성능 실현
3. **Parallelization** — 독립적 서브태스크를 병렬로 실행하거나(섹션닝), 동일 태스크를 여러 번 실행해 다양한 출력을 얻음(보팅)
4. **Orchestrator-Subagents** — 오케스트레이터가 태스크를 분해하고 서브에이전트에 위임, 결과를 통합

---

## 🛡️ CLAUDE.md: 에이전트의 헌법

모든 프로젝트에 `CLAUDE.md` 파일이 있어야 합니다. 이것은 Claude Code의 프로젝트 메모리로, 매 세션 시작 시 읽힙니다. 이는 단순한 문서가 아니라 **AI 에이전트에 대한 행동 제어**입니다. 이 프로젝트에서 생성된 모든 서브에이전트가 이 규칙을 상속받습니다.

```markdown
# CLAUDE.md 예시

## Stack
- Flutter 3+, Dart, Riverpod

## Rules
- Strict null safety 적용
- 모든 새 위젯에 테스트 필수
- 비즈니스 로직은 ViewModel에만

## Architecture
Clean Architecture: data/domain/presentation

## Current Sprint
- 로그인 화면 구현 중 (spec/auth.md 참조)
```

---

## 🧹 Context 관리: 가장 많이 틀리는 부분

컨텍스트는 고갈되는 자원이며, 이를 관리하는 것이 엔지니어링 규율입니다. 핵심 접근법: **컨텍스트는 대화가 아닌 파일로 유지**하세요. spec, 계획, 가드레일은 파일에 저장하고, 에이전트가 매 세션 새로 읽게 합니다. 새 태스크를 시작할 때는 새 세션을 시작하고, 서브에이전트를 격리에 활용합니다. 실제 파일에서 읽어오는 신선한 컨텍스트가 누적된 대화보다 훨씬 효과적입니다.

```
❌ 나쁜 패턴: 하나의 긴 세션에서 모든 것을 처리
✅ 좋은 패턴: 태스크마다 새 세션 + 상태는 파일로 유지
```

---

## 🔌 실전 MCP 연결 예시 (코드)

MCP는 Slack, GitHub, Google Drive, Asana 같은 도구들에 커스텀 통합 코드나 OAuth 플로우 없이 연결할 수 있게 해줍니다. 에이전트가 이메일 규칙을 만들거나, Slack 메시지를 검색해 팀 컨텍스트를 이해하거나, 누군가에게 이미 할당된 작업이 있는지 Asana에서 확인할 수 있습니다.

```python
# Claude Agent SDK 예시
from anthropic import Anthropic

client = Anthropic()

response = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=4096,
    tools=[
        # Bash 도구 - 파일 시스템, 명령 실행
        {"type": "bash_20250124", "name": "bash"},
        # MCP 서버 - 외부 서비스 연결
    ],
    mcp_servers=[
        {"type": "url", "url": "https://github.mcp.server", "name": "github"},
        {"type": "url", "url": "https://slack.mcp.server", "name": "slack"},
    ],
    messages=[{"role": "user", "content": "GitHub 이슈 #42 구현하고 PR 만들어줘"}]
)
```

---

## 📅 학습 로드맵 (10주)

추천 학습 경로입니다:

- **Week 1-2**: 아키텍처 개요, 핵심 개념 (tools, permissions, context)
- **Week 3-4**: Tool 정의와 스키마, Permission 시스템, 커스텀 툴 빌드
- **Week 5-6**: Query Engine 심화, Tool-calling 루프, 상태 관리
- **Week 7-8**: 멀티에이전트 시스템, 컨텍스트 관리, MCP 통합
- **Week 9-10**: 실전 프로젝트로 직접 에이전트 시스템 빌드

---

Flutter 개발에 적용한다면, **Planner 에이전트**가 위젯 구조를 설계하고, **여러 Worker 에이전트**가 각 화면을 병렬로 구현하고, **Reviewer 에이전트**가 Dart 분석과 테스트를 돌리는 구조로 바로 써볼 수 있어요. 특정 패턴 더 파고싶은 게 있으면 알려주세요!
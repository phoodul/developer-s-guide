## Claude Code CLI 설치 + Remote Control 설정

2단계로 진행할게요!

---

### 1단계 — Claude Code 설치

현재 tmux `vibe` 세션 안에서 아래 순서대로 실행하세요:

```bash
# Node.js 설치 (nvm 방식)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

# 쉘 재로드
source ~/.bashrc

# Node.js 22 설치
nvm install 22

# 버전 확인
node --version
npm --version
```

그 다음 Claude Code 설치:

```bash
# Claude Code 설치 (네이티브 인스톨러 - 권장)
curl -fsSL https://claude.ai/install.sh | bash
```

```bash
# Claude Code 환경 변수 설정
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

설치 확인:
```bash
claude --version
```

---

### 2단계 — 로그인

```bash
claude
```

브라우저가 자동으로 열리면 claude.ai 계정으로 로그인하면 돼요. 자동으로 안 열리면 URL을 복사해서 브라우저에 붙여넣으면 돼요.

> ⚠️ API 키는 작동하지 않아요. 반드시 `/login`으로 claude.ai 계정 인증을 해야 해요.

---

### 3단계 — Remote Control 설정

Remote Control은 Claude Code v2.1.51 이상이 필요하고, Pro/Max/Team/Enterprise 플랜에서 사용 가능해요.

로그인 후 프로젝트 폴더에서:

```bash
# 현재 세션에 Remote Control 활성화
claude --remote-control
```

또는 서버 모드:

```bash
claude remote-control
```

세션 URL이 표시되고, 스페이스바를 누르면 QR코드가 나와요. 폰으로 스캔하면 Claude 앱에서 바로 연결돼요!

---

### Remote Control의 핵심 장점

파일은 절대 내 머신을 벗어나지 않아요. Anthropic 서버를 통해 전달되는 건 오직 메시지와 Claude가 생성한 결과뿐이에요.

포트 포워딩 설정이 전혀 필요 없고, 모든 연결은 Claude Code 프로세스에서 시작하는 아웃바운드 HTTPS예요.

---

Node.js 설치부터 시작해보세요! 결과 알려주시면 다음 단계 이어서 도와드릴게요 😊
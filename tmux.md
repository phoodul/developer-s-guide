## tmux 세션이란?

**tmux(Terminal Multiplexer)** 는 하나의 터미널 창에서 여러 개의 터미널 세션을 동시에 실행·관리할 수 있는 도구예요.

핵심 개념 3가지:
- **세션(Session)** — 독립적인 작업 공간. 터미널을 닫아도 백그라운드에서 계속 실행됨
- **윈도우(Window)** — 세션 안의 탭 개념
- **패널(Pane)** — 윈도우를 분할한 화면

---

## PowerShell 7에서 tmux 사용하기

tmux는 **Linux/macOS 전용**이라 Windows PowerShell에서 직접 실행되지 않아요.
PowerShell 7에서 쓰려면 **WSL(Windows Subsystem for Linux)** 을 통해야 해요.

### 1단계 — WSL 설치 (없다면)

```powershell
# PowerShell 7을 관리자 권한으로 실행 후
wsl --install
```

### 2단계 — WSL 안에서 tmux 설치

```powershell
# PowerShell에서 WSL 진입
wsl

# Ubuntu 기준
sudo apt update && sudo apt install tmux -y
```

### 3단계 — 기본 사용법

```bash
# 새 세션 시작
tmux new -s 작업이름

# 세션 목록 확인
tmux ls

# 세션 재연결 (detach 후 돌아오기)
tmux attach -t 작업이름
```

### 자주 쓰는 단축키

| 단축키 | 동작 |
|--------|------|
| `Ctrl+b` → `d` | 세션에서 나가기 (세션은 유지됨) |
| `Ctrl+b` → `c` | 새 윈도우 생성 |
| `Ctrl+b` → `%` | 패널 좌우 분할 |
| `Ctrl+b` → `"` | 패널 상하 분할 |
| `Ctrl+b` → 방향키 | 패널 이동 |

---

## PowerShell 자체 대안

tmux 없이 **PowerShell 7 네이티브**로 비슷하게 쓰고 싶다면:

```powershell
# Windows Terminal 탭/패널 분할로 대체 가능
# 또는 백그라운드 작업
Start-Job -ScriptBlock { python server.py }
Get-Job        # 작업 목록
Receive-Job 1  # 출력 확인
```

---

**어떤 용도로 tmux를 쓰려고 하시나요?** (예: 서버 작업, 개발 환경, 장시간 실행 등) 목적에 맞는 설정을 더 구체적으로 안내해 드릴 수 있어요!
# GitCoach 사용자 설치 가이드

## 1. 개요

GitCoach는 Git 입문 학생의 안전한 협업과 학습을 지원하는 CLI 프로그램입니다.

Git 명령어를 실행하기 전에 규칙 위반을 감지하고 문제의 원인과 올바른 해결 방법을 단계별로 안내합니다.

- 설치 방식: pipx + Python Wheel
- 설치 대상: 일반 사용자 및 학생
- 기준 환경: Windows 10/11, PowerShell
- AI 모델: Ollama (`gemma4:e2b`)

이 문서는 일반 사용자용 설치 가이드이며 개발 및 Docker Compose 환경 구축은 별도의 인프라 문서를 참고 바랍니다.

## 2. 사전 준비

### 2-1. Git 설치 방법

공식 홈페이지에서 Git을 다운로드하여 설치합니다.

- https://git-scm.com/downloads

설치 후 PowerShell에서 확인해 주세요.

```powershell
git --version
```

### 2-2. Python 설치 방법

Python 3.11 이상의 버전을 설치합니다.

- https://www.python.org/downloads/

설치 시 PATH 추가 옵션을 선택하는 것을 권장합니다.

```powershell
py --version
```

### 2-3. Ollama 설치 방법

공식 홈페이지에서 Windows용 Ollama를 다운로드 합니다.

- https://ollama.com/download/windows

설치 후 터미널을 재시작하고 확인합니다.

```powershell
ollama --version
```

GitCoach에서 사용하는 AI 모델을 다운로드합니다.

```powershell
ollama pull gemma4:e2b
```

모델 설치 상태를 확인합니다.

```powershell
ollama list
```

목록에 `gemma4:e2b`가 표시되면 설치가 완료된 것입니다.

## 3. GitCoach 설치

### 3-1. pipx 설치 방법

PowerShell에서 다음 명령어를 실행합니다.

```powershell
py -m pip install --user pipx
py -m pipx ensurepath
```

터미널을 종료하고 다시 실행합니다.

```powershell
py -m pipx --version
```

pipx는 Python 프로그램을 독립된 가상환경에 설치하고 전역 명령어로 사용할 수 있도록 지원합니다.

사용자는 별도로 가상환경을 활성화할 필요가 없습니다.

### 3-2. Wheel 다운로드 방법

프로젝트의 GitHub Releases 페이지에서 배포된 `.whl` 파일을 다운로드합니다.

- https://github.com/enryu0210/Github_Supervisor_Learning_Program/releases

정식 Release가 등록된 이후 다운로드할 수 있습니다.

### 3-3. Wheel 설치 방법

다운로드한 Wheel 파일의 경로를 지정하여 설치합니다.

```powershell
py -m pipx install "<다운로드한 Wheel 파일의 전체 경로>"
```

설치 예시:

```powershell
py -m pipx install ".\gitcoach-0.0.1-py3-none-any.whl"
```

위 파일명은 예시이며, 실제 배포된 버전에 맞게 변경합니다.

### 3-4. 설치 확인 방법

```powershell
py -m pipx list
gitcoach version
gitcoach --help
```

GitCoach 설치 정보, 버전 및 명령어 목록이 출력되면 정상적으로 설치된 것입니다.

## 4. GitCoach 초기 설정

### 4-1. 쉘 래퍼 설치 방법

```powershell
gitcoach init
```

초기 설정 과정에서 다음 작업이 수행됩니다.

- 쉘 래퍼 설치
- Ollama 연결 및 모델 확인
- 사용자 설정 파일 생성
- GitHub 사용자 ID 확인

GitHub 사용자 ID를 자동으로 확인하지 못하는 경우 본인의 GitHub ID를 직접 입력합니다.

초기 설정이 완료되면 PowerShell 터미널을 종료하고 다시 실행합니다.

### 4-2. 설치 환경 진단하기

```powershell
gitcoach doctor
```

다음 항목을 확인합니다.

- Git 설치 상태
- Ollama 연결 상태
- gemma4:e2b 모델 설치 상태
- GitCoach 전역 설치 상태
- PowerShell 쉘 래퍼 설치 상태

사용하지 않는 쉘의 래퍼가 미설치 상태인 것은 문제가 아닙니다.

### 4-3. 쉘 래퍼 적용 확인

```powershell
Get-Command git -All
```

출력 결과에 `Function`과 `Application`이 함께 표시되고 `Function`이 우선하면 PowerShell 쉘 래퍼가 적용된 상태입니다.

## 5. GitCoach 사용 방법

초기 설정 이후에는 기존과 동일하게 PowerShell 터미널에서 Git 명령어를 사용하면 됩니다.

GitCoach는 규칙 위반이 감지된 경우 경고와 학습 가이드를 제공합니다.

### 5-1. 주요 명령어

| 명령어 | 설명 |
|---|---|
| `gitcoach version` | 설치된 버전 확인 |
| `gitcoach doctor` | 설치 환경 진단 |
| `gitcoach explain R06` | 규칙 설명 확인 |
| `gitcoach log` | 현재 레포의 학습 기록 조회 |
| `gitcoach log --today` | 오늘의 학습 기록 조회 |
| `gitcoach guide` | 진행 중인 학습 가이드 확인 |
| `gitcoach guide hint` | 학습 가이드 힌트 확인 |
| `gitcoach sync` | 서버로 학습 기록 전송 |
| `gitcoach uninstall` | 쉘 래퍼 제거 |

### 5-2. 학습 가이드 동작 확인

실제 협업 레포에 영향을 주지 않도록 별도의 테스트 폴더에서 실행합니다.

```powershell
mkdir gitcoach-test
cd gitcoach-test
git init
"GitCoach Test" | Set-Content README.md
git add README.md
git commit -m "test: verify gitcoach wrapper"
```

커밋 메시지 규칙 위반(R06)이 감지되면 `[2] 어떻게 해야 하나요?`를 선택합니다.

안내에 따라 올바른 커밋 메시지를 입력합니다.

```powershell
git commit -m "test(cli): verify gitcoach wrapper"
```

가이드 완료 메시지가 출력되면 학습 기록을 확인합니다.

```powershell
gitcoach log
```

`guided`, `guide_completed` 기록을 확인할 수 있습니다.

GitCoach는 쉘 래퍼를 거치는 터미널 명령을 검사합니다. IDE의 Git 버튼이나 일부 스크립트 실행은 CLI 검사 대상에 포함되지 않습니다.

## 6. 문제 해결

### 6-1. gitcoach 명령어를 찾을 수 없는 경우

```powershell
py -m pipx ensurepath
```

터미널을 종료하고 다시 실행합니다.

```powershell
Get-Command gitcoach
```

### 6-2. PowerShell 실행 정책 오류

PowerShell 실행 정책으로 인해 쉘 래퍼가 설치되어 있어도 프로필 파일을 로드하지 못할 수 있습니다.

현재 실행 정책을 확인합니다.

```powershell
Get-ExecutionPolicy -List
```

조직에서 별도의 보안 정책을 적용하지 않고 있으며 사용자가 변경에 동의하는 경우 현재 사용자 범위에서 실행 정책을 설정할 수 있습니다.

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

터미널을 종료하고 다시 실행한 뒤 확인합니다.

```powershell
Get-Command git -All
```

현재 터미널에서만 임시로 허용하려면 다음 방법을 사용할 수 있습니다.

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
. $PROFILE
```

Process 범위의 설정은 터미널 종료 시 초기화됩니다.

실행 정책 변경은 GitCoach 외의 PowerShell 스크립트에도 영향을 줍니다.

### 6-3. Ollama 연결 실패

Ollama 프로그램이 실행 중인지 확인합니다.

```powershell
ollama list
gitcoach doctor
```

모델이 설치되지 않은 경우 다음 명령어를 실행합니다.

```powershell
ollama pull gemma4:e2b
```

### 6-4. AI 설명이 생성되지 않는 경우

```powershell
gitcoach explain R06
```

`llm_used=False`가 표시되는 경우 LLM 대신 기본 설명이 사용된 것입니다.

모델 초기 로딩, 응답 시간 초과 또는 연결 문제 등으로 발생할 수 있습니다.

LLM 호출에 실패하더라도 기본 설명과 학습 가이드를 사용할 수 있습니다.

### 6-5. 학습 기록이 표시되지 않는 경우

학습 기록은 사용자 전역 경로에 저장됩니다.

```text
~/.gitcoach/events.db
```

`gitcoach log`는 현재 레포를 기준으로 기록을 조회하므로 기존 학습을 진행한 레포에서 다시 실행합니다.

## 7. 서버 연동

GitCoach는 로컬 학습 기록을 서버로 전송하는 기능을 제공합니다.

서버 연동에 필요한 환경변수는 다음과 같습니다.

| 환경변수 | 설명 |
|---|---|
| `GITCOACH_SYNC_URL` | 이벤트 전송 엔드포인트 |
| `GITCOACH_SYNC_TOKEN` | 이벤트 전송 인증 토큰 |

실제 주소와 토큰은 팀 관리자에게 전달받아 사용합니다.

인증 토큰을 Git 저장소에 커밋하거나 외부에 공개하지 않습니다.

서버 설정 완료 후 다음 명령어로 학습 기록을 전송합니다.

```powershell
gitcoach sync
```

세부 환경변수 설정 절차는 서버 연동 검증 완료 후 추가합니다.

일반 사용자용 CLI에는 Supabase service key가 필요하지 않습니다.

## 8. 업데이트

GitHub Releases에서 새로운 버전의 Wheel 파일을 다운로드합니다.

```powershell
py -m pipx install --force "<새로운 Wheel 파일의 전체 경로>"
```

업데이트 후 설치 상태를 확인합니다.

```powershell
gitcoach version
gitcoach doctor
```

쉘 래퍼 변경이 포함된 버전이라면 해당 Release의 안내에 따라 초기 설정을 다시 진행합니다.

## 9. 제거

### 9-1. 쉘 래퍼 제거

GitCoach 패키지를 삭제하기 전에 쉘 래퍼를 먼저 제거합니다.

```powershell
gitcoach uninstall
```

터미널을 종료하고 다시 실행합니다.

```powershell
Get-Command git -All
```

GitCoach가 설치한 `Function`이 사라지고 기존 Git 실행 파일이 사용되는지 확인합니다.

### 9-2. 패키지 제거

```powershell
py -m pipx uninstall gitcoach
```

제거 상태를 확인합니다.

```powershell
py -m pipx list
Get-Command gitcoach -ErrorAction SilentlyContinue
```

GitCoach가 설치 목록에 없고 명령어가 검색되지 않으면 제거가 완료된 것입니다.

사용자 설정 및 학습 기록은 별도의 사용자 전역 경로에 저장되므로 패키지 제거와 구분됩니다.

Git, Python, Ollama 및 다운로드한 AI 모델은 자동으로 제거되지 않습니다.

## 10. 검증 환경

| 항목 | 검증 환경 |
|---|---|
| 운영체제 | Windows |
| 터미널 | VS Code PowerShell |
| Python | 3.13.7 |
| pipx | 1.18.16 |
| Ollama | 0.40.18 |
| AI 모델 | gemma4:e2b |
| 패키징 방식 | Python Wheel + pipx |

macOS 및 Linux 환경은 별도의 설치 검증이 필요합니다.

## 11. 관련 문서

- [프로젝트 README](../../README.md)
- [CLI 개발 가이드](01_cli.md)
- [인프라 구축 가이드](../infra-setup.md)
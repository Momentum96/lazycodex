# LazyCodex - SK하이닉스 AI 해커톤 2026 대회용 포크

[code-yeongyu/lazycodex](https://github.com/code-yeongyu/lazycodex)(OmO의 Codex 배포판)를 SK하이닉스 AI 해커톤 2026 2차 예선 규정에 맞게 줄인 포크입니다. Codex 플러그인 `omo` 하나만 남겼고, 대회에서 문제가 될 수 있는 기능을 빼거나 고쳤습니다.

> 대회 규정상 사용하는 공개 저장소는 **2차 예선 결과 통지 전까지 공개 상태로 유지**해야 합니다. 이 저장소를 삭제하거나 비공개로 돌리지 마세요.

## AI 에이전트용 세팅 순서

이 README를 읽고 환경을 세팅하는 코딩 에이전트(Codex 등)는 아래 순서를 그대로 따르세요. 각 단계는 다시 실행해도 안전합니다. **재시작이 필요한 지점에서는 멈추고 사용자에게 재시작을 요청하세요.** 에이전트는 자기 세션을 재시작할 수 없습니다.

1. **플러그인 설치** (셸에서 실행)

   ```bash
   codex plugin marketplace add Momentum96/lazycodex
   codex plugin add omo@sisyphuslabs
   codex plugin list   # omo@sisyphuslabs 가 installed, enabled 인지 확인
   ```

   `npx lazycodex-ai install`은 절대 실행하지 마세요. upstream이 설치됩니다.

2. **[사용자] 재시작 1**: Codex를 다시 실행하고, 시작 화면의 omo 훅 승인 요청을 승인합니다. `LazyCodex bootstrap running in background` 안내가 보이면 부트스트랩이 끝날 때까지 잠시 기다립니다.

3. **[사용자] 재시작 2**: 부트스트랩이 끝나면 Codex를 한 번 더 재시작합니다. 확인 방법: `~/.codex/config.toml` 맨 위에 `model = "gpt-6.1-sol"`이 있고, `~/.codex/agents/`에 `explorer.toml` 등 역할 파일이 생겼으면 완료입니다.

4. **권한·기능 설정**: 아래 [autonomous 설정 (수동)](#autonomous-설정-수동) 섹션대로 `~/.codex/config.toml`을 편집합니다.
   - 루트 키(`approval_policy`, `sandbox_mode`)는 반드시 첫 번째 `[섹션]`보다 위에 둡니다.
   - `[features]` 섹션이 이미 있으면 새로 만들지 말고 그 안에 키를 추가합니다. 섹션이나 키가 중복되면 Codex가 설정을 읽지 못합니다.
   - 그 밖의 키(모델, `[agents.*]`, `[hooks.state.*]`, `[marketplaces.*]`, `[plugins.*]`)는 건드리지 않습니다.
   - 편집한 뒤 `codex features list`를 실행해 오류가 없는지, `goals`·`multi_agent`·`unified_exec`가 `true`인지 확인합니다.

5. **[사용자] 재시작 3**: 설정을 반영하려고 Codex를 다시 실행합니다.

6. **최종 확인**: `/status`에서 모델이 `gpt-6.1-sol`인지, approval이 `never`, sandbox가 `danger-full-access`인지 확인합니다. 입력창에 `# LazyCodex - SK하이닉스 AI 해커톤 2026 대회용 포크

[code-yeongyu/lazycodex](https://github.com/code-yeongyu/lazycodex)(OmO의 Codex 배포판)를 SK하이닉스 AI 해커톤 2026 2차 예선 규정에 맞게 줄인 포크입니다. Codex 플러그인 `omo` 하나만 남겼고, 대회에서 문제가 될 수 있는 기능을 빼거나 고쳤습니다.

> 대회 규정상 사용하는 공개 저장소는 **2차 예선 결과 통지 전까지 공개 상태로 유지**해야 합니다. 이 저장소를 삭제하거나 비공개로 돌리지 마세요.

를 입력해 스킬 목록(`ultrawork`, `ulw-plan` 등)이 보이면 세팅이 끝난 것입니다.

설정 파일을 고쳤는데 `/status`에 반영되지 않으면, 플랫폼이 관리 설정으로 값을 강제하고 있을 수 있습니다. 이 경우 우회하려 하지 말고 그대로 두세요.

## upstream 대비 변경 사항

| 구분 | 변경 |
|---|---|
| 모델 | 기본 모델과 번들된 에이전트 역할 12개를 모두 `gpt-6.1-sol`(6.1 Sol)로 고정했습니다. upstream 기본값은 대회에서 제한된 `gpt-6-astra`입니다. 기존 Astra 설정은 관리 프로필로 인식되어 Sol로 교체됩니다. |
| 텔레메트리 | PostHog로 세션 정보를 보내는 `telemetry` 컴포넌트와 훅을 제거했습니다. |
| 자동 업데이트 | 세션 시작 때 `npx lazycodex-ai@latest install`을 실행하던 자동 업데이트 훅과 스크립트를 제거했습니다. Codex 버전 업데이트 금지 공지와 부딪히고, 포크 수정 사항을 upstream으로 덮어쓰기 때문입니다. |
| 브라우저 자동화 | `browser`(omowright), `ultimate-browsing`(WAF 우회·스텔스·쿠키 추출), `visual-qa`, `ulw-research` 스킬을 제거했습니다. 남은 스킬과 지시문의 검증 채널도 "curl + 플랫폼 내장 Browser 미리보기 수동 확인"으로 바꿨습니다. |
| Computer Use | 스킬과 지시문에 있던 computer-use(OS 수준 GUI 자동화) 사용 지시를 "대회 규정상 금지"로 바꿨습니다. |
| 외부 전송 | GitHub 이슈·PR을 만드는 `lcx-report-bug`, `lcx-contribute-bug-fix`를 제거했습니다. |
| 기타 제거 | `lcx-doctor`(Astra 기준 점검과 Codex 업그레이드 안내), `teammode`, `coding-agent-sessions` 스킬, 무제한 goal 예산 강제 훅을 제거했습니다. |
| 추가 스킬 | [Nutlope/hallmark](https://github.com/Nutlope/hallmark) 디자인 스킬(MIT)을 `skills/hallmark`에 포함했습니다. 외부 AI 모델 금지 규정 때문에 Together AI 이미지 생성 스크립트(`imagegen.mjs`)는 뺐고, 브라우저 렌더 검사는 Chrome이 없으면 정적 검사만 돌도록 안내를 넣었습니다. `$hallmark <프롬프트>`로 부릅니다. |
| 저장소 정리 | 랜딩 사이트(`packages/web`), 스크린샷·evidence(`.omo`), GitHub workflow, npm 설치기(`bin`, `package.json`), `src` 서브모듈을 제거했습니다. 플러그인은 번들된 `dist`로 동작하므로 서브모듈이 필요 없습니다. |

남아 있는 것: `hallmark`(추가), `ast-grep`, `comment-checker`, `debugging`, `frontend`, `git-master`, `init-deep`, `lsp`, `lsp-setup`, `programming`, `refactor`, `remove-ai-slops`, `review-work`, `rules`, `ultrawork`, `ulw-execute`, `ulw-loop`, `ulw-plan`, `data-scientist` 스킬과 rules·LSP·comment-checker·git-bash·ultrawork·ulw-loop 훅, 원격 MCP `context7`·`grep_app`, 로컬 MCP `git_bash`·`lsp`.

## 설치

upstream의 `npx lazycodex-ai install`은 **사용하지 마세요.** npm에 올라간 upstream 패키지가 설치되어 이 포크의 수정 사항(모델, 텔레메트리·자동 업데이트 제거 등)이 적용되지 않습니다. 반드시 아래처럼 마켓플레이스로 설치합니다.

```bash
codex plugin marketplace add Momentum96/lazycodex
codex plugin add omo@sisyphuslabs
```

1. 설치 후 Codex를 실행하면 시작 화면에서 omo 훅 승인을 요청합니다. 승인하세요. 승인 전에는 훅이 실행되지 않습니다.
2. 첫 세션에서 `LazyCodex bootstrap running in background — restart the session when it completes`가 표시되면, 백그라운드 부트스트랩이 에이전트 역할 등록, bin 링크, `ast_grep` MCP용 `sg` 바이너리 다운로드(GitHub)를 마칠 때까지 기다린 뒤 세션을 재시작하세요.
3. 재시작 후 `/status`에서 모델이 `gpt-6.1-sol`인지 확인하고, 아니면 `/model`로 선택하세요. 입력창에 `$`를 치면 설치된 스킬 목록이 보입니다.

부트스트랩은 권한 설정(approval/sandbox/network)을 건드리지 않습니다.

## autonomous 설정 (수동)

upstream의 `--codex-autonomous`는 npm 설치기 전용이라 이 포크에서는 `~/.codex/config.toml`을 직접 고칩니다. upstream이 하는 변경 중 경고 숨김(`[notice]`)은 제외했습니다.

1. 파일 맨 위, 첫 번째 `[섹션]`보다 앞에 아래 두 줄을 넣거나 기존 값을 바꿉니다.

   ```toml
   approval_policy = "never"
   sandbox_mode = "danger-full-access"
   ```

2. 같은 루트 영역에 `network_access = ...` 줄이 있으면 삭제합니다.
3. `[features]` 섹션에 아래 세 기능을 켭니다. 섹션이 없으면 새로 만듭니다.

   ```toml
   [features]
   multi_agent = true
   unified_exec = true
   goals = true
   ```

## 대회 중 유의 사항

- Codex 버전 업데이트는 금지입니다. 업데이트 권유가 떠도 무시하세요.
- 사용 모델은 공지된 권장 모델(6.1 Sol, 5.6 Terra, 6.0 Luna) 안에서만 고르세요.
- Computer Use와 감독·보안 기능 우회는 금지입니다. 웹 결과물은 서버를 `0.0.0.0`에 바인딩하고 플랫폼 내장 Browser 미리보기로 직접 확인하세요.
- 문제·파일·결과물을 외부로 보내지 마세요. Git Push는 불가합니다.

## 라이선스

MIT. 원저작권은 upstream [code-yeongyu/lazycodex](https://github.com/code-yeongyu/lazycodex)에 있으며 `LICENSE`를 그대로 유지합니다.

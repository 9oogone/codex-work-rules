# codex-work-rules

사용자가 Codex CLI에게 주는 작업 방식 규칙과 빈 기록 서식입니다. 과제 구현 코드·완성 답안은 없습니다.

| 파일 | 용도 |
|---|---|
| `AGENTS.md` | Codex가 시작할 때 읽는 작업 규칙(첫 착수 → 범위 → 작게 구현 → 실행 검증 → 보고) |
| `BRIEF.md` | 담당자 답을 들은 뒤 Codex에 주는 번호 매긴 지시 틀(1~3항목, 완료 기준, 수정 허용, 검증, 안 할 것) |
| `DECISIONS.md` | 담당자 Q&A, 결정·이유·버린 대안, 검증 결과를 적는 빈 서식 |
| `VERIFY.md` | 기능마다 완료 선언 전 돌리는 검증 체크리스트 |

원칙: 과제 README와 제공 지침이 이 저장소보다 우선합니다. 작업 중 만든 결과물·기록은 이 저장소에 올리지 않습니다.

## 사용

**1단계 가져오기.** 과제 작업 폴더(README.md가 있는 곳)에서. 실패하면 멈춥니다. 저장소에는 작업 지침과 빈 서식만 있고 과제 구현 코드·완성 답안은 없습니다.

```bash
kit=$(mktemp -d) && git clone --depth 1 https://github.com/9oogone/codex-work-rules.git "$kit" && ls "$kit"
```

**2단계 적용.** 같은 이름의 파일이 이미 있으면 건너뛰고, 기존 AGENTS.md는 덮지 않고 뒤에 한 번만 붙입니다. 이 저장소의 README는 복사하지 않습니다.

```bash
for f in BRIEF.md DECISIONS.md VERIFY.md; do if [ -e "$f" ]; then echo "이미 있음, 건너뜀: $f"; else cp "$kit/$f" .; fi; done
if [ -f AGENTS.md ]; then if grep -q codex-work-rules AGENTS.md; then echo "이미 붙어 있음"; else { printf '\n\n'; cat "$kit/AGENTS.md"; } >> AGENTS.md; fi; else cp "$kit/AGENTS.md" .; fi
```

기존 AGENTS.md나 AGENTS.override.md가 있으면 붙이기 전에 내용을 읽고 충돌이 없는지 봅니다. 충돌하면 붙이지 않고 과제 지침을 따릅니다.

**3단계 인식 확인.** 적용 뒤 새 Codex 세션을 그 폴더에서 열고 묻습니다.

```
현재 작업 디렉터리와 적용된 지침 파일 경로, 핵심 진행 규칙을 알려줘. 파일 수정·설치·구현은 하지 마.
```

답의 경로가 실제 AGENTS.md와 맞으면 "자동 로드 확인", 안 맞으면 파일을 직접 읽혀 쓰되 "직접 읽기"로 기록합니다.

## clone이 안 될 때 (직접 치는 5줄)

```
# 작업 규칙
1. 첫 착수 때 README를 읽고 목표·제출물·실행 명령·불확실한 점을 정리한다. 이 확인 중에는 수정·설치하지 않는다.
2. 내가 확인한 완료 기준에 필요한 것만, 핵심 흐름 하나부터 작게 만든다. 범위 밖 기능은 추가하지 않는다.
3. 완료라고 말하기 전에 실제로 실행해 기대 결과와 비교한다. 실행 안 한 것은 "미검증"이라고 쓴다.
4. 바꾼 파일·실행 명령·결과·남은 문제·결정 이유를 보고한다.
5. Codex 업데이트·개인 API 키·git push·감독 및 플랫폼 설정 변경·자동 조작 금지. 서버는 0.0.0.0에 바인딩.
```

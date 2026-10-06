# wotjr1649 marketplace

`wotjr1649` 제작자가 Claude Code와 Codex 플러그인을 함께 관리하는 공개 catalog다.
플러그인 코드는 각 플러그인의 저장소에 두고 이 저장소의 두 catalog가 검증된 릴리스 태그를 가리킨다.

| 플러그인 | 설치 식별자 | 릴리스 | 저장소 |
|---|---|---|---|
| devflow | `devflow@wotjr1649` | `v0.2.0` | [wotjr1649/devflow](https://github.com/wotjr1649/devflow) |

## 설치

Claude Code:

```bash
claude plugin marketplace add wotjr1649/marketplace
claude plugin install devflow@wotjr1649
```

Codex:

```bash
codex plugin marketplace add wotjr1649/marketplace
codex plugin add devflow@wotjr1649
```

새 세션에서 반영한다. Codex에서는 `/hooks`에서 설치한 플러그인의 훅을 검토하고 신뢰해야 실행된다.
이전 `devflow@devflow` 설치가 있으면 새 설치와 훅 신뢰를 확인한 뒤 이전 항목을 비활성화한다.
다른 플러그인을 함께 제거하지 않는다. devflow의 프로젝트 설정과 제한은 [README](https://github.com/wotjr1649/devflow#prerequisites-and-limits)를 따른다.

## 업데이트

```bash
claude plugin marketplace update wotjr1649
claude plugin update devflow@wotjr1649
codex plugin marketplace upgrade wotjr1649
codex plugin add devflow@wotjr1649
```

갱신 뒤 새 세션을 시작한다. 자동 업데이트 설정은 사용자 호스트에서 관리한다.

### Codex에 로컬 catalog checkout을 등록한 경우

`wotjr1649`가 미등록된 Codex에서 기존 같은 이름의 marketplace 캐시 폴더를 유지해야 하면,
이 저장소를 별도 폴더에 clone하고 그 checkout을 등록할 수 있다. 이미 등록돼 있으면 현재 소스를 확인하고 기존 등록을 유지한다.
`<marketplace-checkout>`은 이 저장소를 clone한 폴더로 바꾼다.

```bash
codex plugin marketplace add "<marketplace-checkout>"
codex plugin add devflow@wotjr1649
```

이 경우 catalog 등록은 `local`이고 devflow 코드는 catalog가 지정한 원격 Git 릴리스 태그에서 내려받는다.
개발용 `devflow-local` catalog와는 다른 경로다. 갱신은 다음 명령을 쓴다.

```bash
git -C "<marketplace-checkout>" pull --ff-only
codex plugin add devflow@wotjr1649
```

설치·갱신 뒤 새 세션을 시작하고 `/hooks`에서 새 식별자 또는 추가·변경된 훅의 신뢰를 확인한다.
`codex plugin marketplace upgrade`는 Git 소스로 등록한 catalog를 갱신하는 명령이므로 이 local 등록에는 쓰지 않는다.
checkout의 미커밋 변경이나 브랜치 분기로 pull이 거부되면 변경을 보존하고 원인을 확인한다. 강제 덮어쓰지 않는다.

## 유지보수

1. 플러그인 저장소의 버전을 올려 검사·리뷰·통합을 마친다. 태그와 GitHub Release를 게시한다.
2. `.claude-plugin/marketplace.json`과 `.agents/plugins/marketplace.json`의 해당 항목을 같은 새 태그로 바꾼다.
   플러그인 매니페스트의 버전도 바뀌어야 Claude Code 캐시가 갱신된다. 버전을 catalog에 중복하지 않는다.
3. `claude plugin validate .claude-plugin/marketplace.json`으로 검증하고 두 catalog의 플러그인 이름·저장소·태그 일치를 확인한다.
   새 태그가 실제 원격에 있으며 그 매니페스트 버전과 일치하는지 검사한다.
4. catalog를 commit·push하고 두 호스트에서 실제 원격 설치·업데이트와 설치 파일을 검증한다.

태그·GitHub Release 게시만으로 이 catalog가 바뀌지는 않는다. catalog의 `main`을 갱신하고 플러그인 소스는 릴리스 태그로 고정한다.
기존 태그는 재작성하지 않는다. devflow의 세부 [릴리스·롤백 계약](https://github.com/wotjr1649/devflow/blob/main/docs/specs/repository.md#릴리스와-롤백)을 따른다.

새 플러그인은 자신의 저장소·릴리스 태그를 가리키는 항목을 두 catalog에 추가하고 이 표를 갱신한다.
각 플러그인의 버전과 릴리스 시점은 독립적이다. 아직 없는 플러그인의 빈 항목은 만들지 않는다.

문제가 있으면 두 항목을 이전 검증 태그로 함께 되돌려 게시한다. 사용자는 catalog와 플러그인을 다시 갱신하고,
호스트가 이전 버전을 적용하지 않으면 해당 플러그인만 제거·재설치한다. 다른 플러그인과 기존 태그는 보존한다.

## 라이선스

이 catalog는 [Apache-2.0](LICENSE)이며, 각 플러그인의 라이선스는 해당 플러그인 저장소를 따른다.

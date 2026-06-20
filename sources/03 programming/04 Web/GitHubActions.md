# GitHub Actions

## 한 줄 요약

GitHub Actions는 GitHub repository에서 발생한 event를 기준으로 build, test, deploy, issue 처리 같은 작업을 자동 실행하는 workflow 실행 기능이다.

Repository 안의 `.github/workflows/*.yml` 파일은 사람이 직접 실행하는 script가 아니라 GitHub Actions가 읽는 실행 정의다. GitHub는 pull request 생성, comment 작성, branch push 같은 event가 발생하면 workflow file의 `on` 조건을 확인하고, 조건이 맞는 workflow run을 만든다.

## 먼저 알아야 할 용어

| 용어 | 의미 |
|---|---|
| workflow | 자동화 흐름 전체다. repository의 `.github/workflows/*.yml` 파일 하나가 보통 workflow 하나다. |
| event | workflow를 시작시키는 GitHub 활동이다. 예: `pull_request`, `push`, `issue_comment`. |
| workflow run | 특정 event 때문에 실제로 실행된 workflow 1회분이다. |
| job | workflow 안에서 runner 하나를 배정받아 실행되는 작업 묶음이다. |
| step | job 안에서 순서대로 실행되는 단일 작업이다. shell command를 실행하거나 action을 호출한다. |
| action | 재사용 가능한 작업 단위다. 예: `actions/checkout`, `actions/setup-python`, `actions/upload-artifact`. |
| runner | job을 실제로 실행하는 machine이다. GitHub-hosted runner와 self-hosted runner가 있다. |
| status check | commit 또는 pull request에 붙는 성공, 실패, 진행 중 같은 상태 표시다. branch protection이 merge 가능 여부를 판단할 때 사용할 수 있다. |
| artifact | workflow run이 만든 파일 결과물이다. test report, build result, verification output 같은 파일을 run에 첨부할 때 사용한다. |
| secret | workflow 실행 중에만 주입하는 민감한 설정값이다. private key, token, password 같은 값을 repository file에 저장하지 않기 위해 사용한다. |

## 가장 작은 실행 모델

GitHub Actions의 기본 흐름은 다음과 같다.

```text
GitHub event
  -> .github/workflows/*.yml의 on 조건 확인
  -> workflow run 생성
  -> job에 runner 할당
  -> step 순서대로 실행
  -> workflow run 결과와 log 저장
  -> pull request나 commit에 status check 표시
```

예를 들어 다음 workflow는 `main` branch에 push가 생기면 실행된다.

```yaml
name: example

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: python --version
```

이 파일이 `.github/workflows/example.yml`에 있으면, GitHub는 `main` branch push event가 생겼을 때 `example` workflow run을 만든다. `test` job은 `ubuntu-latest` runner에서 실행되고, 첫 step은 repository 코드를 checkout하며, 두 번째 step은 shell command를 실행한다.

## workflow file은 어디에 두는가

Workflow file은 repository의 다음 경로에 둔다.

```text
.github/workflows/<workflow-name>.yml
```

하나의 repository에는 여러 workflow file을 둘 수 있다. 각 workflow는 서로 다른 event와 책임을 가질 수 있다.

예:

```text
.github/workflows/test.yml
.github/workflows/deploy.yml
.github/workflows/comment-handler.yml
```

파일 이름은 사람이 구분하기 위한 이름이고, GitHub Actions 화면에서는 YAML 안의 `name` 값이 workflow 이름으로 표시된다.

```yaml
name: dev-gate
```

## event가 workflow를 실행한다

`on`은 workflow를 언제 실행할지 정하는 부분이다.

자주 쓰는 event는 다음과 같다.

| event | 실행 시점 | 주 사용처 |
|---|---|---|
| `push` | branch나 tag에 commit이 push될 때 | 배포, build, main branch test |
| `pull_request` | pull request가 열리거나 갱신될 때 | merge 전 test, lint, review gate |
| `issue_comment` | issue 또는 pull request에 일반 comment가 작성될 때 | comment command 처리 |
| `workflow_dispatch` | GitHub UI나 API에서 수동 실행할 때 | 수동 배포, 운영 작업 |
| `schedule` | cron schedule에 맞을 때 | 정기 점검, nightly job |

예:

```yaml
on:
  pull_request:
    branches:
      - Dev
    types:
      - opened
      - synchronize
      - reopened
```

이 설정은 base branch가 `Dev`인 pull request가 열리거나, 새 commit으로 갱신되거나, 다시 열릴 때 workflow를 실행한다.

`pull_request` event와 `issue_comment` event는 역할이 다르다.

- `pull_request`
  - pull request head commit을 검증하는 데 적합하다.
  - branch protection required status check로 쓰기 쉽다.
- `issue_comment`
  - pull request 일반 comment를 읽는 데 적합하다.
  - comment command를 감지할 수 있지만, comment 자체가 head commit 검증 결과는 아니다.

## job과 step

`jobs` 아래에는 하나 이상의 job을 둔다.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: python -m pytest
```

`test`는 job id다. `runs-on`은 어떤 runner에서 실행할지 정한다. `steps`는 job 안에서 순서대로 실행된다.

Step은 크게 두 종류다.

```yaml
- uses: actions/checkout@v4
```

`uses`는 이미 만들어진 action을 사용한다.

```yaml
- run: python -m pytest
```

`run`은 shell command를 실행한다.

같은 job 안의 step은 같은 runner filesystem을 공유한다. 그래서 앞 step에서 만든 파일을 뒤 step에서 읽을 수 있다. 하지만 다른 job은 기본적으로 서로 다른 runner에서 실행될 수 있으므로, 파일을 넘기려면 artifact나 cache 같은 별도 전달 방식을 사용해야 한다.

## runner

Runner는 job을 실제로 실행하는 machine이다.

```yaml
runs-on: ubuntu-latest
```

위 설정은 GitHub-hosted Ubuntu runner를 사용한다는 뜻이다. GitHub-hosted runner는 workflow run마다 새 virtual machine으로 준비된다. 로컬 PC나 운영 서버에서 실행되는 것이 아니다.

원격 production 서버에 배포하려면 runner가 직접 production 서버가 되는 것이 아니라, runner가 SSH로 production 서버에 접속해 명령을 실행하는 구조가 된다.

```text
GitHub-hosted runner
  -> SSH
  -> production server
  -> deploy command 실행
```

## checkout

Runner는 자동으로 repository 코드를 가지고 시작하지 않는다. 코드가 필요하면 보통 `actions/checkout`을 사용한다.

```yaml
- uses: actions/checkout@v4
```

Pull request의 특정 commit을 정확히 검증해야 하면 checkout 대상도 명확히 해야 한다.

```yaml
- uses: actions/checkout@v4
  with:
    ref: ${{ github.event.pull_request.head.sha }}
```

이렇게 하면 pull request head commit을 기준으로 test를 실행한다.

## secret과 variable

GitHub Actions에서 설정값은 보통 두 종류로 나눈다.

| 종류 | 용도 | 예 |
|---|---|---|
| variable | 민감하지 않은 설정값 | 허용 사용자 목록, mode 값 |
| secret | 외부에 노출하면 안 되는 값 | SSH private key, API token, password |

Secret은 repository file에 저장하지 않는다. GitHub repository settings에 등록하고, workflow에서 `${{ secrets.NAME }}` 형태로 읽는다.

예:

```yaml
env:
  SERVER_HOST: ${{ secrets.PRODUCTION_HOST }}
  SERVER_USER: ${{ secrets.PRODUCTION_DEPLOYER_USER }}
```

SSH private key를 사용할 때는 보통 runner의 임시 파일로 만든 뒤 `chmod 600`을 적용한다.

```yaml
- run: |
    install -m 700 -d ~/.ssh
    printf '%s\n' "$SSH_PRIVATE_KEY" > ~/.ssh/deploy_key
    chmod 600 ~/.ssh/deploy_key
  env:
    SSH_PRIVATE_KEY: ${{ secrets.PRODUCTION_DEPLOYER_SSH_PRIVATE_KEY }}
```

Secret은 GitHub 화면에서 일반 사용자가 값을 다시 열람하는 방식으로 쓰는 저장소가 아니다. 하지만 workflow file을 수정할 수 있는 사용자는 secret을 사용하는 악의적 workflow를 만들 수 있다. 그래서 secret을 사용하는 repository에서는 workflow file 변경에 code owner review나 branch protection을 적용해야 한다.

## permissions와 GITHUB_TOKEN

Workflow run에는 GitHub API를 호출할 수 있는 `GITHUB_TOKEN`이 제공된다. `permissions`는 이 token이 어떤 권한을 가질지 제한한다.

예:

```yaml
permissions:
  contents: read
  pull-requests: read
```

Comment를 읽고 workflow rerun API를 호출해야 하는 workflow는 더 많은 권한이 필요할 수 있다.

```yaml
permissions:
  actions: write
  contents: read
  issues: read
  pull-requests: read
```

권한은 필요한 job에 필요한 만큼만 부여하는 것이 원칙이다. 예를 들어 test만 하는 workflow에 `contents: write`를 줄 이유는 없다.

## status check와 branch protection

Status check는 commit이나 pull request에 붙는 상태다.

```text
pending
success
failure
skipped
neutral
```

Branch protection은 특정 branch에 merge하기 전에 필요한 조건을 강제하는 repository 설정이다. 예를 들어 `Dev` branch에 `dev-gate` status check를 required로 등록하면, pull request는 `dev-gate`가 통과하기 전까지 merge할 수 없다.

```text
pull request
  -> GitHub Actions workflow run
  -> dev-gate status check 생성
  -> branch protection이 status check 확인
  -> merge 허용 또는 차단
```

여기서 중요한 점은 GitHub Actions가 항상 merge를 직접 수행하는 것은 아니라는 점이다. 일반적인 merge gate 구조에서는 GitHub Actions가 test 결과를 status check로 제공하고, 실제 merge는 사용자가 GitHub UI에서 수행한다.

## artifact

Artifact는 workflow run에서 생성된 파일을 GitHub Actions run에 첨부하는 기능이다.

예:

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: verification-result
    path: storage/verification_runs/*/comparison/**
```

Artifact는 다음 상황에 유용하다.

- test report를 내려받아 확인해야 할 때
- build 결과물을 다음 사람이 받아야 할 때
- verification 실패 원인을 파일로 남겨야 할 때

Artifact는 log와 다르다. Log는 command 출력이고, artifact는 workflow가 업로드한 파일 묶음이다.

## workflow summary

Workflow summary는 GitHub Actions run 화면에 사람이 읽기 쉬운 요약을 남기는 기능이다.

```bash
echo "verification failed" >> "$GITHUB_STEP_SUMMARY"
```

긴 log를 다 읽지 않아도 결과 요약, 실패 이유, artifact 이름, rerun 대상 같은 정보를 볼 수 있게 할 때 사용한다.

## pull request gate 예시

Pull request gate는 merge 전에 반드시 통과해야 하는 검사 흐름이다.

예:

```text
1. ms -> Dev pull request 생성
2. pull_request event 발생
3. dev-gate workflow 실행
4. lint, unit test, integration test 실행
5. staging verification 실행
6. dev-gate status check 성공 또는 실패
7. branch protection이 merge 가능 여부 판단
```

이 구조에서 branch protection은 `dev-gate` status check 하나만 required로 둘 수 있다. 내부에서 여러 검사를 실행하더라도 GitHub branch protection 입장에서는 `dev-gate` 하나만 보면 된다.

## comment command 예시

Comment command는 pull request 일반 comment에 특정 명령어를 적어서 workflow가 반응하게 하는 방식이다.

예:

```text
/force-dev-merge 사유: verification diff가 운영 데이터 정리 지연 때문에 발생했고 코드 회귀가 아님
```

이 comment 자체가 required status check가 되는 것은 아니다. 보통은 다음처럼 별도 workflow가 comment를 감지한다.

```text
1. PR comment 작성
2. issue_comment event 발생
3. comment handler workflow 실행
4. comment 작성자와 사유 확인
5. 같은 pull request head commit의 기존 dev-gate run 찾기
6. dev-gate rerun 요청
7. rerun된 dev-gate가 comment를 읽고 예외 조건 적용
```

이렇게 하는 이유는 오래된 comment가 새 commit에 자동 적용되는 문제를 막기 위해서다. Comment command를 현재 head commit과 연결해서 해석해야 한다.

## production deploy 예시

Production deploy workflow는 merge gate와 분리하는 것이 좋다.

```text
1. pull request가 Dev에 merge됨
2. Dev branch에 push event 발생
3. production-deploy workflow 실행
4. GitHub-hosted runner가 production SSH key를 임시 파일로 준비
5. runner가 production server에 SSH 접속
6. production server에서 deploy command 실행
7. deploy command exit code가 workflow 성공 또는 실패가 됨
```

Merge gate와 production deploy를 분리하면 실패 책임이 명확해진다.

- merge gate 실패
  - 아직 `Dev`에 들어가면 안 되는 변경이다.
- production deploy 실패
  - 이미 `Dev`에는 들어갔지만 production activation에 실패한 상태다.

두 실패는 운영 대응이 다르므로 같은 required status check 안에 넣지 않는 편이 이해하기 쉽다.

## AutomationProject 예시

AutomationProject의 배포 흐름은 GitHub Actions를 다음 세 workflow로 나눈다.

| workflow file | event | 책임 |
|---|---|---|
| `.github/workflows/dev-gate.yml` | `pull_request` to `Dev` | `ms` 또는 `SJ` branch의 pull request를 merge해도 되는지 판단한다. |
| `.github/workflows/dev-gate-comment-rerun.yml` | `issue_comment` | `/force-dev-merge 사유: ...` comment를 검증하고 같은 head commit의 `dev-gate` run을 다시 실행한다. |
| `.github/workflows/production-deploy.yml` | `push` to `Dev` | `Dev`에 merge된 뒤 production server에서 deploy command를 실행한다. |

### dev-gate

`dev-gate`는 branch protection이 요구하는 단일 required status check다.

기본 흐름:

```text
1. pull request head commit checkout
2. Python dependency 설치
3. pull request 본문과 comment에서 /force-dev-merge 확인
4. force command가 유효하면 test와 verification skip
5. force command가 없으면 total test 실행
6. verification 실행
7. verification result artifact upload
8. dev-gate 성공 또는 실패 결정
```

실행하는 대표 command:

```bash
python -m scripts.test_manager --run-total-test
python -m scripts.verification.compare_latest_settlement_results_on_staging_between_code_versions_with_production_snapshot --force --candidate-branch ms
python -m scripts.verification.compare_latest_settlement_results_on_staging_between_code_versions_with_production_snapshot --force --candidate-branch SJ
```

`--candidate-branch` 값은 pull request head branch가 `ms`인지 `SJ`인지에 따라 달라진다.

### dev-gate-comment-rerun

`dev-gate-comment-rerun`은 required status check가 아니다. 이 workflow의 책임은 comment를 감지해 `dev-gate`를 다시 실행시키는 것이다.

```text
1. issue_comment event 수신
2. pull request comment인지 확인
3. /force-dev-merge 사유: ... 형식인지 확인
4. 작성자가 허용 목록에 있는지 확인
5. 현재 pull request head SHA 조회
6. 해당 head SHA의 최신 dev-gate run 조회
7. GitHub REST API로 dev-gate rerun 요청
```

Rerun된 `dev-gate`는 시작 시점에 pull request comment를 다시 읽고, 유효한 force command가 있으면 test와 verification을 skip한다.

### production-deploy

`production-deploy`는 `Dev` branch에 push가 생기면 실행된다. Pull request가 `Dev`에 merge되면 GitHub 관점에서는 `Dev` branch에 새 commit이 생긴 것이므로 `push` event가 발생한다.

```text
1. Dev push event 수신
2. production SSH private key를 runner 임시 파일로 준비
3. known_hosts로 서버 인증 설정
4. production server에 SSH 접속
5. sudo -n /usr/local/bin/deploy 실행
6. command exit code를 workflow 결과로 사용
```

이 workflow는 merge를 직접 수행하지 않는다. Merge는 GitHub pull request 화면에서 사용자가 수행하고, `production-deploy`는 merge 이후 production activation만 담당한다.

## 자주 헷갈리는 점

### `.yml` 파일은 서버에서 실행되는가

아니다. `.github/workflows/*.yml` 파일은 GitHub Actions가 읽는 실행 정의다. 실제 command는 GitHub Actions runner에서 실행된다. Runner가 SSH로 서버에 접속하면 그때 서버 command가 실행된다.

### workflow와 job은 같은가

다르다. Workflow는 자동화 흐름 전체이고, job은 workflow 안에서 runner 하나를 받아 실행되는 작업 묶음이다.

### action과 GitHub Actions는 같은가

다르다. GitHub Actions는 전체 자동화 기능 이름이다. action은 workflow step에서 `uses:`로 호출하는 재사용 가능한 작업 단위다.

### pull request comment workflow를 required check로 등록해도 되는가

일반적으로 적절하지 않다. Comment workflow는 pull request head commit을 검증하는 check가 아니라 comment event에 반응하는 side effect workflow다. Merge 가능 여부는 pull request head commit을 기준으로 실행된 workflow의 status check로 판단하는 편이 명확하다.

### secret을 GitHub에 넣으면 아무도 볼 수 없는가

GitHub UI에서 secret value를 일반 text처럼 다시 열람하는 구조는 아니다. 하지만 workflow file을 수정할 수 있는 사용자는 secret을 외부로 보내는 workflow를 만들 수 있다. 따라서 secret 사용은 workflow 변경 review, branch protection, 최소 권한 원칙과 함께 설계해야 한다.

## 관련 문서

- [SSH](./SSH.md)
  - GitHub Actions runner가 production server에 접속할 때 필요한 SSH key pair, `authorized_keys`, `known_hosts` 개념을 보충한다.
- [Web Deployment](./WebDeployment.md)
  - GitHub Actions가 실행하는 deploy command가 서버에서 어떤 배포 구조를 갱신하는지 이해할 때 필요하다.
- [Docker Network](./DockerNetwork.md)
  - Workflow test에서 Docker service나 Docker Compose database를 사용할 때 network 개념을 보충한다.

## 참고한 공식 문서

- GitHub Docs, Understanding GitHub Actions
  - https://docs.github.com/en/actions/get-started/understand-github-actions
- GitHub Docs, Workflows
  - https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows
- GitHub Docs, Events that trigger workflows
  - https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows
- GitHub Docs, Using secrets in GitHub Actions
  - https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets
- GitHub Docs, About protected branches
  - https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
- GitHub Docs, About code owners
  - https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
- GitHub Docs, Download workflow artifacts
  - https://docs.github.com/en/actions/how-tos/manage-workflow-runs/download-workflow-artifacts

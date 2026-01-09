# restack-cli

Git 커밋을 PR 단위로 재정렬하는 CLI 도구입니다.

## 개요

`restack-cli`는 여러 개의 Git 커밋을 PR(Pull Request) 단위로 재정렬하여 새로운 브랜치 체인을 생성하는 도구입니다. 각 PR의 마지막 커밋에 연결된 브랜치를 기준으로 커밋들을 그룹화하고, base 브랜치부터 순차적으로 cherry-pick하여 새로운 브랜치 체인을 만듭니다.

## 기능

- 🔍 **PR 범위 자동 분석**: 커밋 범위에서 브랜치가 연결된 커밋을 기준으로 PR 단위로 자동 그룹화
- 🍒 **Cherry-pick 기반 재정렬**: 각 PR의 커밋들을 순차적으로 cherry-pick하여 새로운 브랜치 체인 생성
- 🔍 **타입 검사**: 각 PR 단계마다 TypeScript 타입 검사를 실행하여 안정성 보장
- ⚠️ **충돌 처리**: cherry-pick 중 충돌 발생 시 대화형으로 해결 방법 선택 가능
- 🎨 **컬러 출력**: 직관적인 컬러 출력으로 진행 상황을 쉽게 파악

## 설치

```bash
npm install -g restack
```

또는

```bash
git clone https://github.com/uglyus-daniel/restack-cli.git
cd restack-cli
npm link
```

## 사용법

```bash
restack <start-commit> <end-commit> [base-branch]
```

### 매개변수

- `start-commit`: 재정렬할 커밋 범위의 시작 커밋 해시
- `end-commit`: 재정렬할 커밋 범위의 끝 커밋 해시
- `base-branch`: (선택사항) 기본 브랜치. 기본값은 `origin/feature/anytime`

### 예시

```bash
restack 938fa82e f80ce55b origin/feature/anytime
```

## 작동 방식

1. **커밋 범위 분석**: 지정된 시작 커밋부터 끝 커밋까지의 모든 커밋을 가져옵니다.

2. **PR 범위 식별**: 각 커밋에 연결된 브랜치를 확인하여 PR 경계를 식별합니다. 각 PR의 마지막 커밋에 로컬 브랜치가 있어야 합니다.

3. **재정렬 계획 표시**: 발견된 PR 범위와 재정렬 계획을 표시하고 사용자 확인을 받습니다.

4. **Cherry-pick 실행**: 
   - 첫 번째 PR은 base 브랜치에서 시작
   - 이후 PR들은 이전 PR의 임시 브랜치에서 시작
   - 각 PR의 모든 커밋을 순차적으로 cherry-pick

5. **타입 검사**: 각 PR 단계 완료 후 TypeScript 타입 검사를 실행합니다.

6. **브랜치 변환**: 모든 작업이 완료되면 임시 브랜치(`RESTACK-` prefix)를 최종 브랜치로 변환합니다.

## 요구사항

- Node.js >= 14.0.0
- Git
- TypeScript 프로젝트 (타입 검사 기능 사용 시)

## 주의사항

- 각 PR의 마지막 커밋에 로컬 브랜치가 있어야 합니다.
- 프로젝트 루트에 `uglyus-web` 디렉토리가 있어야 합니다 (타입 검사용).
- 충돌 발생 시 수동으로 해결해야 합니다.
- 원격 브랜치를 base로 사용하는 경우 자동으로 fetch합니다.

## 라이선스

MIT

## 기여

이슈와 Pull Request를 환영합니다!

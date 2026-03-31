# 🔧 문제 해결 가이드

## ❌ "Unable to connect to GitHub" 오류

### 원인 1: Token 권한 부족 (가장 흔함)

#### 해결 방법:
1. https://github.com/settings/tokens 접속
2. 기존 token 찾아서 **Edit** 또는 삭제 후 재생성
3. 권한 확인 - **반드시 다음을 모두 체크**:

```
✅ repo (전체 체크 필수!)
  ✅ repo:status
  ✅ repo_deployment
  ✅ public_repo
  ✅ repo:invite
  ✅ security_events
```

4. **Update token** 또는 **Generate token** 클릭
5. 새 token 복사 (ghp_XXXXX 형식)
6. Figma Tokens Studio에서 다시 설정

### 원인 2: Repository 정보 오타

Figma Tokens Studio에 **정확히 이렇게** 입력:

```
Personal Access Token: ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Repository: dev-teamremited/design-tokens
Default branch: main
File Path: tokens
Base Branch: main
```

**주의사항:**
- Repository 앞에 `https://github.com/` 붙이면 안 됨
- `dev-teamremited/design-tokens` 이렇게만 입력
- 대소문자 정확히 일치해야 함

### 원인 3: Figma 플러그인 캐시 문제

1. Figma 플러그인 완전히 종료
2. Figma 앱 재시작
3. 플러그인 다시 실행하여 연결 시도

---

## 🔄 대안: 수동 Export/Import 방식

GitHub 연동이 안 되면 수동으로도 사용 가능합니다.

### 1단계: Figma에서 토큰 정의

1. Tokens Studio 플러그인 실행
2. Settings → Sync providers → **Local** 선택
3. 토큰 정의 (색상, 간격 등)

### 2단계: Export

1. 플러그인에서 **Export** 버튼 클릭
2. JSON 파일들 다운로드

### 3단계: 로컬에 복사

```bash
# 다운로드한 JSON 파일들을 design-tokens/tokens/ 폴더에 복사
cp ~/Downloads/*.json /Users/kimhaneui/design-tokens/tokens/

# 빌드
cd /Users/kimhaneui/design-tokens
npm run build

# Git 커밋
git add tokens/
git commit -m "update: manual token update from Figma"
git push
```

### 4단계: Figma에서 Import (다른 팀원용)

1. 플러그인 실행 → Settings → Local
2. **Import** 버튼 클릭
3. GitHub에서 받은 JSON 파일 업로드

---

## 🧪 Token 권한 테스트

터미널에서 Token이 제대로 작동하는지 테스트:

```bash
# YOUR_TOKEN을 실제 token으로 교체
curl -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/dev-teamremited/design-tokens

# 성공하면 repository 정보가 JSON으로 출력됨
# 실패하면 "Bad credentials" 또는 "Not Found" 에러
```

---

## 📱 Figma Desktop vs Web 차이

**Figma Desktop App 사용 권장**
- Desktop: GitHub 연동 더 안정적
- Web: 브라우저 CORS 정책으로 인한 제한 있음

https://www.figma.com/downloads/ 에서 Desktop App 다운로드

---

## 🆘 여전히 안 될 때

### 플랜 A: GitHub Actions로 자동화

`.github/workflows/sync-tokens.yml` 생성:
```yaml
name: Sync Tokens
on:
  workflow_dispatch:

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm run build
      - uses: stefanzweifel/git-auto-commit-action@v4
```

Figma에서 수동 export → GitHub에서 Actions 실행 → 자동 빌드

### 플랜 B: Tokens Studio GitHub App 사용

1. https://github.com/apps/tokens-studio 접속
2. **Install** 클릭
3. `dev-teamremited/design-tokens` repository 선택
4. Figma 플러그인에서 GitHub App 방식으로 연결

### 플랜 C: VS Code Extension 사용

1. VS Code에 "Tokens Studio" extension 설치
2. 로컬에서 JSON 직접 편집
3. Git으로 커밋

---

## ✅ 정상 작동 확인 방법

GitHub 연동 성공 시:
1. Figma 플러그인 상단에 "GitHub: Design Tokens" 표시
2. **Pull** 버튼 클릭 → 토큰 로드됨
3. 토큰 수정 후 **Push** → GitHub commit 생성됨

---

## 💬 추가 도움이 필요하면

1. Token 생성 스크린샷 (token 값은 가리고)
2. Figma 플러그인 설정 화면 스크린샷
3. 정확한 에러 메시지 전문

이 정보를 제공하면 더 정확한 해결책을 드릴 수 있습니다.

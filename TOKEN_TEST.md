# Token 연결 테스트 가이드

## 🔍 Step 1: Token이 repository에 접근 가능한지 테스트

터미널에서 다음 명령어를 실행하세요 (YOUR_TOKEN 부분을 실제 token으로 교체):

```bash
curl -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/dev-teamremited/design-tokens
```

### ✅ 성공 시:
```json
{
  "id": 123456,
  "name": "design-tokens",
  "full_name": "dev-teamremited/design-tokens",
  ...
}
```
→ Token이 작동함. Figma 플러그인 설정 문제.

### ❌ 실패 시 (404 Not Found):
```json
{
  "message": "Not Found"
}
```
→ Organization 권한 문제 (아래 Step 2 참고)

### ❌ 실패 시 (401 Bad credentials):
```json
{
  "message": "Bad credentials"
}
```
→ Token이 잘못됨. 재생성 필요.

---

## 🏢 Step 2: Organization 권한 설정

**dev-teamremited가 Organization이므로 추가 설정이 필요합니다.**

### 2-1. Personal Access Token에 Organization 권한 추가

1. https://github.com/settings/tokens 접속
2. 생성한 Token 클릭 → **Edit**
3. 스크롤해서 **Resource owner** 섹션 찾기
4. **dev-teamremited** organization 옆의 드롭다운 클릭
5. **All repositories** 또는 **Only select repositories** 선택
   - All repositories: 더 간단 (권장)
   - Only select repositories: design-tokens만 선택
6. **Update token** 클릭
7. 새로 생성된 token 복사 (변경되었을 수 있음)

### 2-2. Organization에서 Third-party access 허용

1. https://github.com/orgs/dev-teamremited/settings/oauth_application_policy 접속
2. **Personal access tokens** 섹션 확인
3. 다음 중 하나:
   - **Allow all** (모든 PAT 허용) - 권장
   - 또는 특정 token 승인 필요

**Organization 관리자만 가능합니다.** 관리자가 아니면 관리자에게 요청하세요.

---

## 🎨 Step 3: Figma에서 정확한 정보 재입력

1. Figma Tokens Studio 플러그인 열기
2. Settings → Sync providers
3. 기존 GitHub provider **삭제**
4. **Add new** → **GitHub** 선택
5. **정확히** 다음과 같이 입력:

```
Name: Design Tokens

Personal Access Token:
ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
(복사-붙여넣기로 입력. 수동 타이핑 금지!)

Repository:
dev-teamremited/design-tokens
(주의: 앞에 https:// 없음!)

Default branch:
main
(소문자 m)

File Path:
tokens
(앞뒤 슬래시 없음!)

Base Branch:
main
```

6. **Save** 클릭
7. 에러 발생 시 → Step 4로

---

## 🖥️ Step 4: Figma Desktop App 사용

**Web 버전 Figma에서 GitHub 연결이 안 될 수 있습니다.**

1. Figma Desktop App 다운로드: https://www.figma.com/downloads/
2. Desktop App에서 파일 열기
3. Tokens Studio 플러그인 실행
4. Step 3 재시도

Desktop App이 더 안정적입니다.

---

## 🔧 Step 5: 다른 방법 - GitHub App 사용

Personal Access Token 대신 GitHub App으로 연결:

1. https://github.com/apps/tokens-studio 접속
2. **Install** 클릭
3. **dev-teamremited** organization 선택
4. **Only select repositories** → **design-tokens** 선택
5. **Install** 완료
6. Figma Tokens Studio에서:
   - Settings → Sync providers → Add new → **GitHub (via App)**
   - 자동으로 연결됨

이 방법이 더 안전하고 쉬울 수 있습니다.

---

## 📝 Step 6: 수동 방식 (최후의 수단)

GitHub 연동이 계속 안 되면:

1. Figma Tokens Studio → Settings → **Local** 선택
2. 토큰 정의
3. **Export** 버튼 → JSON 파일 다운로드
4. JSON 파일을 `/Users/kimhaneui/design-tokens/tokens/`에 복사
5. 터미널:
```bash
cd /Users/kimhaneui/design-tokens
npm run build
git add tokens/
git commit -m "update: tokens from Figma"
git push
```

---

## 🆘 지금 바로 테스트해보세요

```bash
# 1. Token 테스트 (YOUR_TOKEN 교체)
curl -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/dev-teamremited/design-tokens

# 2. 성공하면 → Figma 설정 재확인
# 3. 404 에러면 → Organization 권한 설정
# 4. 401 에러면 → Token 재생성
```

어떤 결과가 나오는지 알려주시면 정확한 해결책을 드리겠습니다!

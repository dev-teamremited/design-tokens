# 🎨 Figma 연동 완료 가이드

## ✅ 완료된 사항

- ✅ design-tokens 프로젝트 설정
- ✅ GitHub repository 생성: **https://github.com/dev-teamremited/design-tokens**
- ✅ 샘플 토큰 파일 생성 및 커밋

## 🚀 다음 단계: Figma에서 연동하기

### 1단계: GitHub Personal Access Token 생성 (2분)

1. 이 링크 클릭: https://github.com/settings/tokens/new
2. 설정:
   ```
   Note: Figma Tokens Studio
   Expiration: No expiration (또는 원하는 기간)
   Select scopes:
     ✅ repo (전체 체크 - public, private repositories 포함)
   ```
3. 아래로 스크롤 → **Generate token** 클릭
4. 🔐 **토큰 복사** (ghp_로 시작) - 다시 볼 수 없으니 메모장에 임시 저장!

### 2단계: Figma에서 Tokens Studio 플러그인 설치 (1분)

1. Figma 열기
2. 메뉴 → **Plugins** → **Browse plugins in Community**
3. 검색: **"Tokens Studio for Figma"**
4. **Install** 클릭

### 3단계: GitHub 연동 설정 (3분)

1. Figma 파일 열기 (또는 새 파일 생성)
2. 플러그인 실행:
   - **Plugins** → **Tokens Studio for Figma** 클릭
3. 플러그인 왼쪽 하단 **⚙️ (Settings)** 클릭
4. **Sync providers** 탭 선택
5. **Add new** 버튼 → **GitHub** 선택
6. 다음 정보 **정확하게** 입력:

```
Name: Design Tokens (원하는 이름)
Personal Access Token: [1단계에서 복사한 토큰]
Repository: dev-teamremited/design-tokens
Default branch: main
File Path: tokens
Base Branch: main
```

7. **Save** 클릭
8. 상단 드롭다운에서 방금 만든 "Design Tokens" provider 선택

### 4단계: 토큰 가져오기 (Pull) (1분)

1. 플러그인 오른쪽 상단 **Pull** 버튼 클릭
2. GitHub에서 샘플 토큰들이 자동으로 로드됨
3. 왼쪽 패널에서 토큰 확인:
   - global (기본 색상, 간격 등)
   - light (라이트 테마)
   - dark (다크 테마)

## 🎨 토큰 사용해보기

### Figma에서 토큰 적용
1. 프레임이나 도형 선택
2. Figma 오른쪽 패널에서 Fill 또는 다른 속성 선택
3. Tokens Studio 플러그인에서 토큰 클릭
4. 자동으로 해당 값이 적용됨

### 토큰 수정하기
1. 플러그인에서 토큰 클릭 → 값 수정
2. 오른쪽 상단 **Push** 버튼 클릭
3. Commit message 입력: `update: change primary color`
4. **Push changes** 클릭
5. GitHub에 자동으로 커밋됨!

## 💻 코드에서 사용하기

### 토큰 변경사항을 코드로 가져오기

```bash
cd /Users/kimhaneui/design-tokens
git pull origin main
npm run build
```

### harini (Next.js)에서 사용

```tsx
// app/layout.tsx
import '../../design-tokens/dist/web/tokens.css';

// 컴포넌트에서
<button style={{
  backgroundColor: 'var(--light-color-primary)',
  padding: 'var(--global-spacing-md)'
}}>
  버튼
</button>
```

### my-b2c-app (React Native)에서 사용

```tsx
import { LightColorPrimary, GlobalSpacingMd } from '../../design-tokens/dist/rn/tokens';

<View style={{
  backgroundColor: LightColorPrimary,
  padding: parseInt(GlobalSpacingMd)
}} />
```

## 🔄 일상적인 워크플로우

```
1. Figma에서 디자인 작업
2. Tokens Studio에서 토큰 수정
3. Push 버튼 클릭 → GitHub 자동 업데이트
4. 터미널에서:
   cd /Users/kimhaneui/design-tokens
   git pull && npm run build
5. harini/my-b2c-app에서 자동으로 새 토큰 사용됨!
```

## 📚 추가 리소스

- GitHub Repository: https://github.com/dev-teamremited/design-tokens
- 상세 가이드: `README.md`
- 빠른 시작: `QUICK_START.md`
- 코드 예시: `examples/` 폴더

## 🆘 문제 해결

### "Authentication failed" 에러
→ Personal Access Token 확인
  - ghp_로 시작하는지 확인
  - repo 권한이 체크되어 있는지 확인
  - 토큰이 만료되지 않았는지 확인

### Push 후 로컬에서 토큰이 안 보임
→ `git pull origin main` 실행 잊지 마세요!

### 빌드 후 dist 폴더가 비어있음
→ tokens/ 폴더에 JSON 파일이 있는지 확인
→ `npm run build` 다시 실행

## 💡 Pro Tips

1. **Semantic 토큰 사용**:
   - ❌ `{global.color.blue.500}` 직접 사용
   - ✅ `{light.color.primary}` semantic 토큰 사용

2. **테마 전환**:
   - `$themes.json`에서 light/dark 설정
   - 각 테마별로 다른 토큰 세트 사용 가능

3. **타입 지정**:
   - Color, Spacing, FontSize 등 타입 명확히 설정
   - Style Dictionary가 자동으로 적절한 형식으로 변환

4. **네이밍 컨벤션**:
   - global: 기본 값 (색상 팔레트, 기본 간격)
   - semantic: 용도별 (button.background, text.primary)
   - component: 컴포넌트별 (card.padding, modal.radius)

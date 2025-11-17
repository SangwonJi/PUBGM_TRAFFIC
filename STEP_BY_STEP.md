# 📝 단계별 실행 가이드 (A to Z)

## 🎯 목표
PUBG Mobile Revenue 시각화를 GitHub Pages에 배포하여 언제든지 접속 가능하게 만들기

---

## 📍 현재 상태 확인

### ✅ 이미 완료된 것들
- [x] `index.html` - 인터랙티브 시각화 페이지 생성됨
- [x] `_config.yml` - GitHub Pages 설정 파일 생성됨
- [x] `data/sample_data.csv` - 샘플 데이터 생성됨
- [x] `.github/workflows/update-visualization.yml` - 자동 업데이트 워크플로우 생성됨
- [x] 가이드 문서들 생성됨

### ⏳ 해야 할 것들
- [ ] Git 저장소 초기화 (아직 안 했다면)
- [ ] GitHub 저장소 생성/확인
- [ ] 파일 커밋 및 푸시
- [ ] GitHub Pages 활성화

---

## 🔤 A단계: GitHub 저장소 준비

### A-1. GitHub 저장소가 이미 있는 경우
1. 브라우저에서 GitHub.com 접속
2. 저장소 페이지로 이동
3. 저장소 URL 복사 (예: `https://github.com/sangwonji/my-repo.git`)

### A-2. 새 저장소를 만들어야 하는 경우
1. GitHub.com 로그인
2. 우측 상단 **+** → **New repository**
3. 저장소 이름 입력 (예: `pubgm-visualization`)
4. **Public** 선택 ⚠️ (무료 GitHub Pages는 Public 저장소만 가능)
5. **Initialize this repository with a README** 체크 해제
6. **Create repository** 클릭
7. 저장소 URL 복사

**저장소 URL 예시:**
```
https://github.com/sangwonji/pubgm-visualization.git
```

---

## 🔤 B단계: 로컬에서 Git 설정

### B-1. 터미널 열기
**방법 1: VS Code 사용 (권장)**
- VS Code에서 프로젝트 폴더 열기
- `Ctrl + `` (백틱) 키로 터미널 열기

**방법 2: Git Bash 사용**
- 시작 메뉴에서 "Git Bash" 검색
- 실행

**방법 3: CMD 사용**
- `Win + R` → `cmd` 입력 → Enter

### B-2. 프로젝트 폴더로 이동
```bash
cd "c:\Users\sangwon.ji\Desktop\자동화"
```

### B-3. Git 저장소 초기화 확인
```bash
git status
```

**결과에 따라:**

**케이스 1: "fatal: not a git repository" 오류**
→ Git 저장소가 아직 초기화되지 않음
```bash
git init
```

**케이스 2: 정상적으로 상태 표시**
→ 이미 Git 저장소임, 다음 단계로

### B-4. Git 사용자 정보 설정 (처음만)
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### B-5. 원격 저장소 연결 확인
```bash
git remote -v
```

**결과에 따라:**

**케이스 1: 아무것도 표시되지 않음**
→ 원격 저장소가 연결되지 않음
```bash
git remote add origin https://github.com/사용자명/저장소명.git
```
(A단계에서 복사한 URL 사용)

**케이스 2: 이미 연결되어 있음**
→ 다음 단계로

**케이스 3: 잘못된 URL이 연결되어 있음**
```bash
git remote remove origin
git remote add origin https://github.com/사용자명/저장소명.git
```

---

## 🔤 C단계: 파일 추가 및 커밋

### C-1. 현재 상태 확인
```bash
git status
```

### C-2. 시각화 파일들 추가
```bash
git add index.html _config.yml data/ .github/ README*.md DEPLOYMENT_GUIDE.md QUICK_START.md
```

**또는 개별적으로:**
```bash
git add index.html
git add _config.yml
git add data/
git add .github/
git add README.md
git add README_GITHUB_PAGES.md
git add DEPLOYMENT_GUIDE.md
git add QUICK_START.md
```

### C-3. 추가된 파일 확인
```bash
git status
```
→ 초록색으로 표시된 파일들이 추가됨

### C-4. 커밋
```bash
git commit -m "Add interactive visualization for GitHub Pages"
```

**성공 메시지 예시:**
```
[main (root-commit) abc1234] Add interactive visualization for GitHub Pages
 8 files changed, 500 insertions(+)
```

---

## 🔤 D단계: GitHub에 푸시

### D-1. 브랜치 확인
```bash
git branch
```

**결과:**
- `* main` 또는 `* master` 표시됨
- 별표(*)는 현재 브랜치

### D-2. 브랜치 이름 확인
현재 브랜치가 `main`인지 `master`인지 확인

### D-3. 첫 푸시 (처음만)
```bash
git push -u origin main
```

**또는 master 브랜치인 경우:**
```bash
git push -u origin master
```

### D-4. 인증
**팝업이 뜨면:**
- GitHub 사용자명 입력
- 비밀번호 또는 Personal Access Token 입력

**Personal Access Token 사용 방법:**
1. GitHub.com → 우측 상단 프로필 → **Settings**
2. 왼쪽 메뉴 하단 **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token (classic)** 클릭
5. **Note**: "Git Push" 입력
6. **Expiration**: 원하는 기간 선택
7. **repo** 체크박스 선택
8. 하단 **Generate token** 클릭
9. 생성된 토큰 복사 (한 번만 표시됨!)
10. 비밀번호 입력 시 이 토큰 붙여넣기

### D-5. 푸시 성공 확인
**성공 메시지 예시:**
```
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Writing objects: 100% (15/15), done.
To https://github.com/사용자명/저장소명.git
 * [new branch]      main -> main
Branch 'main' set up to track 'remote branch 'main' from 'origin'.
```

### D-6. 이후 푸시 (변경사항이 있을 때)
```bash
git push
```

---

## 🔤 E단계: GitHub Pages 활성화

### E-1. GitHub 저장소 페이지 열기
브라우저에서 저장소 URL 접속:
```
https://github.com/사용자명/저장소명
```

### E-2. Settings 메뉴 클릭
저장소 상단 탭에서 **Settings** 클릭

### E-3. Pages 메뉴 찾기
왼쪽 사이드바에서 스크롤하여 **Pages** 클릭

### E-4. Source 설정
1. **Source** 섹션에서:
   - 드롭다운 클릭
   - **Deploy from a branch** 선택 (기본값)
2. **Branch** 섹션에서:
   - 첫 번째 드롭다운: `main` 선택 (또는 `master`)
   - 두 번째 드롭다운: `/ (root)` 선택
3. **Save** 버튼 클릭

### E-5. 배포 확인
- 페이지 상단에 노란색 배너 표시:
  ```
  Your site is ready to be published at https://사용자명.github.io/저장소명/
  ```
- 몇 분 후 배너가 녹색으로 변경:
  ```
  Your site is published at https://사용자명.github.io/저장소명/
  ```

### E-6. Actions 탭에서 확인 (선택사항)
- 저장소 상단 **Actions** 탭 클릭
- "pages build and deployment" 워크플로우 확인
- 녹색 체크 표시가 나타나면 배포 완료

---

## 🔤 F단계: 사이트 접속 및 테스트

### F-1. 사이트 URL 확인
GitHub Pages 설정 페이지 또는 저장소 메인 페이지에서 URL 확인:
```
https://사용자명.github.io/저장소명/
```

### F-2. 브라우저에서 접속
1. 위 URL을 주소창에 입력
2. Enter 키 누르기
3. 시각화 페이지가 로드되는지 확인

### F-3. 기능 테스트

**✅ 기본 표시 확인:**
- [ ] 페이지가 정상적으로 로드되는가?
- [ ] 트리맵이 표시되는가?
- [ ] 통계 카드 4개가 표시되는가?
- [ ] 샘플 데이터가 자동으로 표시되는가?

**✅ 인터랙티브 기능 확인:**
- [ ] 트리맵에 마우스를 올리면 상세 정보가 표시되는가?
- [ ] 트리맵을 클릭/드래그할 수 있는가?

**✅ CSV 업로드 기능 확인:**
1. **"CSV 파일 로드"** 버튼 클릭
2. `data/sample_data.csv` 파일 선택
3. 트리맵이 업데이트되는지 확인
4. 상태 배지가 "CSV 데이터 사용 중"으로 변경되는지 확인

---

## 🔤 G단계: 문제 해결

### G-1. 페이지가 404 오류
**원인:** 배포가 아직 완료되지 않음
**해결:**
- 5-10분 기다린 후 다시 시도
- GitHub Actions 탭에서 배포 상태 확인

### G-2. CSS/JavaScript가 로드되지 않음
**원인:** 인터넷 연결 문제 또는 CDN 차단
**해결:**
- 인터넷 연결 확인
- 브라우저 개발자 도구(F12)에서 콘솔 오류 확인
- 다른 브라우저에서 시도

### G-3. Git 푸시 실패
**원인:** 인증 문제 또는 원격 저장소 URL 오류
**해결:**
```bash
# 원격 저장소 확인
git remote -v

# URL 수정
git remote set-url origin https://사용자명@github.com/사용자명/저장소명.git

# 다시 푸시
git push -u origin main
```

### G-4. GitHub Pages가 활성화되지 않음
**원인:** 저장소가 Private이거나 설정 오류
**해결:**
- 저장소를 Public으로 변경 (Settings → General → Danger Zone)
- Pages 설정 다시 확인 (main/master 브랜치, / (root))

---

## 🔤 H단계: 자동 업데이트 설정 (선택사항)

### H-1. GitHub Actions 확인
`.github/workflows/update-visualization.yml` 파일이 이미 생성되어 있음

### H-2. 자동 실행 확인
- 기본적으로 매일 자정(UTC)에 자동 실행
- 수동 실행: 저장소 → **Actions** → **Update Visualization** → **Run workflow**

### H-3. 데이터 업데이트 스크립트 추가 (고급)
`update_data.py` 파일을 생성하여 API에서 최신 데이터를 가져오도록 설정 가능

---

## ✅ 최종 체크리스트

배포 완료 확인:
- [ ] Git 저장소가 초기화되었는가?
- [ ] 원격 저장소가 연결되었는가?
- [ ] 모든 파일이 커밋되었는가?
- [ ] GitHub에 푸시되었는가?
- [ ] GitHub Pages가 활성화되었는가?
- [ ] 사이트가 정상적으로 접속되는가?
- [ ] 시각화가 정상적으로 표시되는가?
- [ ] CSV 업로드 기능이 작동하는가?

---

## 🎉 완료!

모든 단계를 완료하면 다음 URL에서 시각화를 확인할 수 있습니다:

```
https://사용자명.github.io/저장소명/
```

**축하합니다! 이제 언제든지 이 URL로 접속하여 시각화를 볼 수 있습니다!** 🎊

---

## 📚 추가 자료

- **상세 가이드**: `DEPLOYMENT_GUIDE.md` 참조
- **빠른 시작**: `QUICK_START.md` 참조
- **GitHub Pages 문서**: https://docs.github.com/en/pages

---

## 💡 팁

1. **빠른 배포**: `deploy.bat` 파일을 더블클릭하여 자동 배포
2. **업데이트**: 파일 수정 후 `git add`, `git commit`, `git push`만 하면 자동 반영
3. **커스터마이징**: `index.html` 파일을 수정하여 디자인 변경 가능

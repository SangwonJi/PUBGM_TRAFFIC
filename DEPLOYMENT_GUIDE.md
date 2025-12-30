# 🚀 GitHub Pages 배포 완전 가이드 (A to Z)

이 문서는 PUBG Mobile Revenue 시각화를 GitHub Pages에 배포하는 전체 과정을 단계별로 설명합니다.

---

## 📋 사전 준비사항

- ✅ GitHub 계정
- ✅ Git 설치 (https://git-scm.com/downloads)
- ✅ VS Code 또는 다른 텍스트 에디터

---

## A단계: GitHub 저장소 확인/생성

### A-1. 기존 저장소가 있는 경우
1. GitHub.com에 로그인
2. 저장소 페이지로 이동
3. 저장소 URL 복사 (예: `https://github.com/사용자명/저장소명.git`)

### A-2. 새 저장소를 만들어야 하는 경우
1. GitHub.com에 로그인
2. 우측 상단 **+** 버튼 → **New repository** 클릭
3. 저장소 이름 입력 (예: `pubgm-visualization`)
4. **Public** 선택 (GitHub Pages 무료 사용을 위해)
5. **Initialize this repository with a README** 체크 해제
6. **Create repository** 클릭
7. 저장소 URL 복사

---

## B단계: 로컬 Git 저장소 초기화

### B-1. 터미널 열기
- **VS Code**: `Ctrl + `` (백틱) 또는 터미널 메뉴
- **Git Bash**: 시작 메뉴에서 "Git Bash" 검색
- **CMD**: `Win + R` → `cmd` 입력

### B-2. 프로젝트 폴더로 이동
```bash
cd "c:\Users\sangwon.ji\Desktop\자동화"
```

### B-3. Git 저장소 초기화 (아직 안 했다면)
```bash
git init
```

### B-4. 원격 저장소 연결
```bash
# 기존 원격 저장소가 있다면 제거
git remote remove origin

# 새 원격 저장소 추가 (A단계에서 복사한 URL 사용)
git remote add origin https://github.com/사용자명/저장소명.git
```

**예시:**
```bash
git remote add origin https://github.com/sangwonji/pubgm-visualization.git
```

---

## C단계: 파일 추가 및 커밋

### C-1. 모든 파일 상태 확인
```bash
git status
```

### C-2. 시각화 관련 파일 추가
```bash
git add index.html
git add _config.yml
git add data/
git add .github/
git add README*.md
```

또는 한 번에:
```bash
git add index.html _config.yml data/ .github/ README*.md
```

### C-3. 변경사항 커밋
```bash
git commit -m "Add interactive visualization for GitHub Pages"
```

**참고:** 처음 커밋이라면 Git 사용자 정보 설정 필요:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

---

## D단계: GitHub에 푸시

### D-1. 브랜치 확인
```bash
git branch
```

### D-2. 메인 브랜치로 이동 (필요시)
```bash
# 현재 브랜치가 main이 아니면
git checkout -b main
```

또는:
```bash
git checkout -b master
```

### D-3. GitHub에 푸시

**첫 푸시인 경우:**
```bash
git push -u origin main
```

또는 (master 브랜치인 경우):
```bash
git push -u origin master
```

**이후 푸시:**
```bash
git push
```

### D-4. 인증
- GitHub 계정과 비밀번호 입력
- 또는 Personal Access Token 사용 (비밀번호 대신)

**Personal Access Token 생성 방법:**
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. **Generate new token** 클릭
3. **repo** 권한 체크
4. 토큰 생성 후 복사
5. 비밀번호 입력 시 토큰 붙여넣기

---

## E단계: GitHub Pages 활성화

### E-1. GitHub 저장소 페이지로 이동
브라우저에서 저장소 페이지 열기

### E-2. Settings 메뉴 클릭
저장소 상단 메뉴에서 **Settings** 클릭

### E-3. Pages 메뉴 선택
왼쪽 사이드바에서 **Pages** 클릭

### E-4. Source 설정
- **Source**: 드롭다운에서 **Deploy from a branch** 선택
- **Branch**: 
  - `main` 선택 (또는 `master`)
  - `/ (root)` 선택
- **Save** 버튼 클릭

### E-5. 배포 확인
- 몇 분 후 **"Your site is live at..."** 메시지 확인
- 또는 **Actions** 탭에서 배포 진행 상황 확인

---

## F단계: 사이트 접속 및 확인

### F-1. 사이트 URL 확인
GitHub Pages 설정 페이지에서 표시된 URL:
```
https://사용자명.github.io/저장소명/
```

**예시:**
```
https://sangwonji.github.io/pubgm-visualization/
```

### F-2. 브라우저에서 접속
1. 위 URL을 브라우저 주소창에 입력
2. 시각화 페이지가 표시되는지 확인
3. 샘플 데이터가 자동으로 표시되는지 확인

### F-3. 기능 테스트
- ✅ 트리맵이 정상적으로 표시되는가?
- ✅ 통계 카드가 표시되는가?
- ✅ CSV 파일 업로드 기능이 작동하는가?

---

## G단계: CSV 파일 업로드 테스트

### G-1. 샘플 CSV 파일 확인
`data/sample_data.csv` 파일이 올바른 형식인지 확인:
```csv
Country,Day1,Day2,Day3
USA,500000,520000,575000
China,400000,410000,434000
...
```

### G-2. 웹 페이지에서 테스트
1. GitHub Pages 사이트 접속
2. **"CSV 파일 로드"** 버튼 클릭
3. `data/sample_data.csv` 파일 선택
4. 트리맵이 업데이트되는지 확인

---

## H단계: 자동 업데이트 설정 (선택사항)

### H-1. GitHub Actions 확인
`.github/workflows/update-visualization.yml` 파일이 이미 생성되어 있음

### H-2. 자동 업데이트 활성화
- 기본적으로 매일 자정(UTC)에 실행되도록 설정됨
- 수동 실행: 저장소 → **Actions** 탭 → **Update Visualization** → **Run workflow**

### H-3. 데이터 업데이트 스크립트 추가 (선택사항)
`update_data.py` 파일을 생성하여 자동으로 최신 데이터를 가져오도록 설정 가능

---

## I단계: 문제 해결

### I-1. 페이지가 표시되지 않음
- **확인사항:**
  - GitHub Pages 설정이 올바른가? (main/master 브랜치, / (root))
  - 저장소가 Public인가? (무료 계정의 경우)
  - 배포가 완료되었는가? (Actions 탭 확인)
- **해결방법:**
  - Settings → Pages에서 설정 재확인
  - 몇 분 더 기다린 후 새로고침

### I-2. CSS/JavaScript가 로드되지 않음
- **확인사항:**
  - 인터넷 연결 확인
  - 브라우저 콘솔에서 오류 확인 (F12)
- **해결방법:**
  - CDN 링크가 올바른지 확인
  - 브라우저 캐시 삭제 후 재시도

### I-3. CSV 파일 업로드가 작동하지 않음
- **확인사항:**
  - CSV 파일 형식이 올바른가? (Country,Day1,Day2,Day3)
  - 브라우저 콘솔에서 오류 확인
- **해결방법:**
  - CSV 파일을 텍스트 에디터로 열어 형식 확인
  - 샘플 데이터와 비교

### I-4. Git 푸시 오류
- **확인사항:**
  - 원격 저장소 URL이 올바른가?
  - 인증 정보가 올바른가?
- **해결방법:**
  ```bash
  git remote -v  # 원격 저장소 확인
  git remote set-url origin https://사용자명@github.com/사용자명/저장소명.git
  ```

---

## J단계: 커스터마이징 (선택사항)

### J-1. 색상 변경
`index.html` 파일에서 색상 매핑 부분 수정

### J-2. 상위 국가 수 변경
`index.html`의 `calculateStats` 함수에서 `.slice(0, 15)` 부분 수정

### J-3. 제목/설명 변경
`index.html`의 `<title>` 및 헤더 부분 수정

---

## ✅ 체크리스트

배포 전 확인사항:
- [ ] 모든 파일이 로컬에 생성되었는가?
- [ ] Git 저장소가 초기화되었는가?
- [ ] 원격 저장소가 연결되었는가?
- [ ] 파일들이 커밋되었는가?
- [ ] GitHub에 푸시되었는가?
- [ ] GitHub Pages가 활성화되었는가?
- [ ] 사이트가 정상적으로 접속되는가?
- [ ] 시각화가 정상적으로 표시되는가?

---

## 📞 추가 도움말

- **GitHub Pages 문서**: https://docs.github.com/en/pages
- **Plotly.js 문서**: https://plotly.com/javascript/
- **Git 기본 명령어**: https://git-scm.com/docs

---

## 🎉 완료!

모든 단계를 완료하면 다음 URL에서 시각화를 확인할 수 있습니다:
```
https://사용자명.github.io/저장소명/
```

축하합니다! 🎊

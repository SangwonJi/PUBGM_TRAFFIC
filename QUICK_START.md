# ⚡ 빠른 시작 가이드 (5분 완성)

## 1️⃣ 터미널 열기
VS Code에서 `Ctrl + `` (백틱) 또는 Git Bash 실행

## 2️⃣ 폴더 이동
```bash
cd "c:\Users\sangwon.ji\Desktop\자동화"
```

## 3️⃣ Git 초기화 (처음만)
```bash
git init
git remote add origin https://github.com/사용자명/저장소명.git
```

## 4️⃣ 파일 추가 및 커밋
```bash
git add index.html _config.yml data/ .github/ README*.md
git commit -m "Add interactive visualization"
```

## 5️⃣ 푸시
```bash
git push -u origin main
```

## 6️⃣ GitHub Pages 활성화
1. GitHub 저장소 → **Settings** → **Pages**
2. Source: **main** 브랜치, **/ (root)** 선택
3. **Save** 클릭

## 7️⃣ 접속
몇 분 후: `https://사용자명.github.io/저장소명/`

---

**자세한 설명은 `DEPLOYMENT_GUIDE.md` 참조**

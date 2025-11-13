# PUBGM Review Log Automation (Python)
> **목표:** Outlook 폴더 **`검수적재`** 로 모아 둔 "검수 메일"을 읽고, 본문에서 **Google 스프레드시트 링크**를 추출해  
> KRAFTON Confluence 페이지(**ID=772382267**)의 특정 테이블(**ac:local-id = `df68e8ad-9a41-4c84-80d9-4949c78c6686`**)에 **한 줄씩 기록**합니다.  
> 기록 컬럼은 페이지 테이블과 동일한 순서로: **Date | 지역 | Summary | Review Feedback | Final Docs | Note | E-Mail**.  
> (Feedback/Note는 비워 두고, 나머지는 메일에서 파싱)

---

## 1) 이 리포지토리 구조
```
pubgm_review_automation/
├─ pubgm_review_automation.py   # 실행 스크립트
├─ index.html                   # PUBGM Revenue 시각화 (GitHub Pages)
├─ _config.yml                  # GitHub Pages 설정
├─ data/
│  └─ sample_data.csv          # 샘플 데이터
├─ .github/workflows/
│  └─ update-visualization.yml # 자동 업데이트 워크플로우
└─ README.md                    # 이 문서
```

---

## 2) 필요한 것
### A. Python 의존성
```bash
pip install requests msal beautifulsoup4 lxml python-dotenv
```

### B. Microsoft Graph (Outlook)
Azure AD에 **Public client** 앱 등록 (Device Code 로그인)
- Redirect URI: `https://login.microsoftonline.com/common/oauth2/nativeclient`
- **API 권한**(Delegated): `Mail.Read`, `Mail.ReadWrite`, `User.Read`
- 처음 실행 시 터미널에 **Device Code** 안내가 뜨고, 브라우저에서 로그인/승인합니다.

### C. Confluence Cloud API 토큰
- https://id.atlassian.com/manage-profile/security/api-tokens 에서 생성
- 이메일/토큰을 Basic Auth로 사용

### D. 환경변수(.env) 예시
프로젝트 루트에 `.env` 생성:
```
MS_TENANT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
MS_CLIENT_ID=yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy
MS_GRAPH_SCOPES=Mail.Read Mail.ReadWrite User.Read

# 메일 소스/사후처리 폴더
MS_SRC_FOLDER=검수적재
MS_DONE_FOLDER=검수적재/처리완료

# Confluence
CONF_BASE_URL=https://krafton.atlassian.net/wiki
CONF_EMAIL=sangwon.ji@krafton.com
CONF_API_TOKEN=zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz
CONF_PAGE_ID=772382267
CONF_TABLE_LOCAL_ID=df68e8ad-9a41-4c84-80d9-4949c78c6686
```

> **운영 팁**: “처리완료” 폴더를 만들어 두고, 성공 처리 후 메일을 이동하도록 설정하면 중복 처리를 자연스럽게 피할 수 있습니다.

---

## 3) 어떻게 동작하나
1. Graph API로 `MS_SRC_FOLDER` 의 메일 목록(`--top` 개) 조회
2. 각 메일 상세(제목/본문 HTML) 조회
3. **파싱 규칙**
   - **Date**: 수신일(UTC → `YYYY-MM-DD`)
   - **지역(Region)**: 제목의 대괄호 `[PUBGM/MKT/REGION]` 중 **마지막 토큰** (예: `.../NA]` → `NA`)
   - **Summary**: `]` 뒤 문자열에서 **`검수` 앞**까지 잘라 간단 요약
   - **Final Docs**: 본문에서 **`https://docs.google.com/spreadsheets/d/{ID}`** 를 첫 1개 추출 → `/edit` 로 교정
   - **E-Mail**: 발신자 이름
   - **Review Feedback / Note**: 빈 칸
4. Confluence 페이지 GET(`/rest/api/content/{id}?expand=body.storage,version,title`)
5. Storage HTML에서 **`<table ac:local-id="CONF_TABLE_LOCAL_ID">`**의 `<tbody>` 끝에 `<tr>` 추가
   - `<!-- MSGID:{messageId} -->` 주석을 같이 심어 **중복 방지**
6. 페이지 **버전+1**로 PUT 저장
7. (옵션) 성공 후 메일을 `MS_DONE_FOLDER` 로 이동

---

## 4) 실행법
```bash
# Dry-run (Confluence에 쓰지 않고 동작만 점검)
python pubgm_review_automation.py --top 20 --dry-run

# 실제 업데이트
python pubgm_review_automation.py --top 50
```

### 스케줄링(선택)
- macOS/Linux: crontab 예) 매시 10분
  ```
  10 * * * * /usr/bin/python3 /path/pubgm_review_automation/pubgm_review_automation.py --top 50 >> /path/logs/review.log 2>&1
  ```
- Windows: 작업 스케줄러에서 Python 실행 등록

---

## 5) 커스터마이즈 포인트
- **제목 규칙 변경**: `parse_region`, `parse_summary` 함수를 고치세요.
- **링크 포맷 추가**: 정규식을 확장하세요(Google Drive/Docs 등도).
- **중복 판정**: 현재는 `<!-- MSGID:... -->`. 필요하면 `날짜+제목 해시` 등으로 변경 가능.
- **테이블 위치**: ac:local-id가 바뀌면 `.env`의 `CONF_TABLE_LOCAL_ID` 업데이트.

---

## 6) 문제 해결(Quick Check)
- **401/403** (Confluence): 이메일/토큰/페이지 권한 확인
- **409** (PUT 충돌): 동시 실행 방지(한 프로세스만), 직전 버전+1인지 확인
- **테이블 찾기 실패**: 페이지 Storage의 실제 `ac:local-id` 재확인 (편집/복제 시 변경될 수 있음)
- **메일이 너무 많음**: `--top` 조절 또는 이미 처리한 메일은 “처리완료”로 이동

---

## 7) 이번 대화 기준 요구사항을 반영
- **입력**: Outlook `검수적재` 폴더의 "검수 요청" 메일
- **파싱**: 제목에서 Region/Summary, 본문에서 Google Sheet 링크
- **출력**: Confluence 페이지(772382267)의 **Review_Log 테이블**에 1줄씩 누적
- **중복 방지**: 메일 ID로 체크
- **사후처리**: (선택) 메일 이동(처리완료)

---

## 8) Power Automate vs Python — 추천
- 규칙이 자주 변하고, 향후 확장이 많다면 **Python 주력**을 권장합니다.
- 단, 팀에서 UI 기반 운영이 필요하면 PA를 보조로 유지하는 **하이브리드**도 좋습니다.

---

## 9) 📊 PUBGM Revenue 시각화 (GitHub Pages)

이 저장소에는 PUBG Mobile 매출 데이터를 시각화하는 인터랙티브 웹 페이지가 포함되어 있습니다.

### GitHub Pages로 배포하기

1. **저장소에 파일 푸시**
   ```bash
   git add index.html _config.yml data/
   git commit -m "Add visualization"
   git push
   ```

2. **GitHub Pages 활성화**
   - 저장소의 **Settings** → **Pages** 이동
   - Source: `main` 브랜치, `/ (root)` 폴더 선택
   - Save 클릭

3. **접속**
   - 몇 분 후 `https://[사용자명].github.io/[저장소명]/` 접속
   - 인터랙티브 트리맵 시각화 확인

### 기능
- ✅ **인터랙티브 트리맵**: Plotly.js 기반 동적 시각화
- ✅ **CSV 파일 업로드**: 브라우저에서 직접 데이터 업로드
- ✅ **실시간 통계**: 총 매출액, 평균/최대/최소 변화율 표시
- ✅ **반응형 디자인**: 모든 디바이스 지원

### 자동 업데이트 (선택사항)
GitHub Actions가 매일 자동으로 데이터를 업데이트합니다. 
자세한 내용은 `README_GITHUB_PAGES.md` 참조.

### CSV 파일 형식
```csv
Country,Day1,Day2,Day3
USA,500000,520000,575000
China,400000,410000,434000
...
```

더 자세한 내용은 `README_GITHUB_PAGES.md` 파일을 참조하세요.

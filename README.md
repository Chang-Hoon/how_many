# How many — 조건별 인구 추정기

성별·나이대·지역·종교·취미·키·몸무게·소득·혼인·안경·탈모 조건을 고르면, 대한민국에 해당하는 사람이 몇 명쯤인지 추정하는 웹앱입니다.

- 파일 하나(`index.html`)로 동작합니다. 서버나 빌드 과정이 필요 없습니다.
- 외부에서 불러오는 것은 Google Fonts 글꼴뿐이며, 인터넷이 없으면 기본 글꼴로 표시됩니다.
- 브라우저에서 `index.html`을 더블클릭해도 바로 실행됩니다.

## 폴더 구성

| 파일 | 설명 |
|---|---|
| `index.html` | 웹앱 본체 (데이터·계산·화면 모두 포함) |
| `README.md` | 이 문서 (GitHub 저장소 첫 화면에 표시됨) |

---

## GitHub Pages로 게시하기

GitHub Pages는 저장소의 HTML 파일을 무료로 웹사이트로 띄워 주는 기능입니다. 게시하면 주소는 다음 형태가 됩니다.

```
https://<GitHub 아이디>.github.io/<저장소 이름>/
```

> **공개 범위 주의**: 무료 계정에서 GitHub Pages를 쓰려면 저장소가 **Public(공개)** 이어야 합니다. 저장소의 코드와 게시된 페이지 모두 누구나 볼 수 있습니다. Private 저장소에서 Pages를 쓰려면 GitHub Pro 등 유료 요금제가 필요합니다.

### 방법 A — 웹 브라우저로만 올리기 (Git 설치 불필요)

1. https://github.com 에 로그인합니다.
2. 오른쪽 위 **+** → **New repository** 를 누릅니다.
   - Repository name: `How_many` (원하는 이름)
   - **Public** 선택
   - 나머지는 기본값 그대로 두고 **Create repository** 클릭
3. 새 저장소 화면에서 **uploading an existing file** 링크(또는 **Add file → Upload files**)를 누릅니다.
4. `D:\Z\How_many` 폴더의 `index.html`, `README.md` 를 끌어다 놓고 아래 **Commit changes** 를 누릅니다.
5. 저장소 상단 **Settings** → 왼쪽 메뉴 **Pages** 로 이동합니다.
6. **Build and deployment** 에서
   - Source: **Deploy from a branch**
   - Branch: **main**, 폴더: **/ (root)** → **Save**
7. 1~2분 기다린 뒤 같은 Pages 화면을 새로고침하면 상단에 `Your site is live at https://...` 주소가 표시됩니다. 그 주소로 접속하면 됩니다.

### 방법 B — Git 명령어로 올리기

Git이 설치되어 있어야 합니다 (`git --version` 으로 확인, 없으면 https://git-scm.com 에서 설치).
먼저 GitHub에서 위 2단계처럼 **빈 저장소**(README 추가 안 함)를 만든 뒤, PowerShell에서:

```powershell
cd D:\Z\How_many
git init
git add index.html README.md
git commit -m "조건별 인구 추정기 최초 게시"
git branch -M main
git remote add origin https://github.com/<GitHub 아이디>/How_many.git
git push -u origin main
```

푸시가 끝나면 방법 A의 5~7단계(Settings → Pages)로 게시를 켭니다.

### 수정 후 다시 게시하기

- 웹 방식: 저장소에서 `index.html` 을 열고 연필(Edit) 아이콘으로 고치거나, **Add file → Upload files** 로 새 파일을 다시 올리면 덮어씁니다.
- Git 방식:

```powershell
cd D:\Z\How_many
git add index.html
git commit -m "수정 내용 요약"
git push
```

푸시 후 1~2분 안에 사이트에 반영됩니다. 반영이 안 보이면 브라우저에서 `Ctrl + F5` 로 강력 새로고침하세요.

### 문제 해결

| 증상 | 확인할 것 |
|---|---|
| 404 페이지가 뜸 | 파일 이름이 정확히 `index.html`(소문자)인지, 저장소 최상위에 있는지, Pages 브랜치가 `main` / `(root)` 인지 |
| Settings에 Pages 메뉴가 안 보임 | 저장소가 Public인지 확인 |
| `git push` 때 로그인 창 | GitHub 계정으로 로그인하거나, 비밀번호 대신 Personal Access Token 사용 |
| 회사 PC에서 push 실패 | 사내 프록시/보안 정책으로 github.com이 막혔을 수 있음 → 방법 A(웹 업로드) 또는 개인 PC 사용 |

---

## 데이터와 계산 방식 요약

- **기본 인구**: 행정안전부 주민등록인구 2026년 10월 (17개 시도 × 성별 × 6개 나이대)
- **조건별 비율**: 성별·나이대별 확률을 칸마다 곱해서 합산 (조건끼리 서로 독립이라고 가정)
- **공식 통계 기반**: 종교(한국갤럽 2025), 취미(2025 국민생활체육조사), 키(사이즈코리아 8차), 소득(국세청·국가데이터처 2024), 혼인(2025 인구주택총조사), 탈모(한국갤럽)
- **추정값**: 악기 연주, 멧돼지 사냥·사육, 대머리 수준, 나이대별 안경 착용률, 종교의 지역 차이 등 (앱 화면에서 분홍색 **추정** 표시)

자세한 출처와 가정은 앱 하단 **데이터 출처와 계산 방식** 표에 있습니다.

### 데이터 갱신 방법

인구 수치는 `index.html` 안의 `const POP={...}` 에 있습니다. 시도별로 `M`(남성), `F`(여성) 배열이 있고, 순서는 `[10대 이하, 20대, 30대, 40대, 50대, 60대 이상]` 입니다. KOSIS(kosis.kr) 「행정구역(시군구)별/성별/연령별 주민등록인구」에서 새 값을 받아 이 숫자만 바꾸면 됩니다.

### 특수 표시

선택한 지역·성별·나이대에 세종 · 남성 · 50대가 포함되고(여러 개 선택하거나 비워 둔 경우도 포함), 취미로 '멧돼지 사냥' 또는 '멧돼지 사육'(둘 다, 또는 '수영'을 더한 조합 포함, 여러 개면 '모두 하는 사람' 기준)을 선택하면, 다른 조건(종교·키·안경 등)과 관계없이 추정값이 1명 미만일 때 `1명` 으로 표시됩니다. 조건 코드는 `index.html` 에서 `jkd` 로 검색하면 찾을 수 있습니다.

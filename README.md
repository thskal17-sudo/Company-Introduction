# 회사 소개서 자동 생성기

`data/company.yaml` 한 파일에 회사 정보와 강사 정보를 적으면
**HTML · PDF · Word(DOCX)** 형식의 회사 소개서가 자동으로 만들어집니다.

```
data/company.yaml  ──▶  python generate.py --all  ──▶  output/company-introduction.{html,pdf,docx}
```

## 1. 가장 쉬운 사용법 (GitHub 웹에서만 작업)

1. GitHub 에서 `data/company.yaml` 을 열어 연필 아이콘으로 수정하고 **Commit changes** 를 누릅니다.
2. 1~2분 뒤 **Actions** 탭의 "회사 소개서 자동 생성" 작업이 끝나면
   `output/` 폴더에 새 소개서가 커밋되어 있습니다. (Actions 실행 화면 하단 artifact 로도 받을 수 있습니다.)
3. 로고나 강사 사진은 `assets/` 폴더에 올린 뒤 YAML 에 경로만 적어 주세요.
   예) `logo: "assets/logo.png"`, `photo: "assets/instructors/hong.jpg"`

## 2. 내 컴퓨터에서 직접 생성

```bash
pip install -r requirements.txt
python generate.py            # HTML 만
python generate.py --all      # HTML + PDF + DOCX
python generate.py --pdf      # PDF 추가 (Chrome / Chromium 설치 필요)
python generate.py --docx     # Word 추가
python generate.py -d data/other.yaml -o dist -n brochure   # 다른 데이터 · 출력 폴더 · 파일명
```

- PDF 는 설치된 Chrome/Chromium 을 자동으로 찾아 사용합니다. 못 찾으면 `CHROME_PATH` 환경변수로 실행 파일 경로를 지정하거나,
  `output/company-introduction.html` 을 브라우저에서 열어 **인쇄 → PDF 로 저장** 하면 같은 결과를 얻습니다.
- 어떤 섹션이 A4 한 장을 넘기면 생성 시 경고가 출력됩니다. 그 섹션의 문장이나 항목 수를 줄여 주세요.

## 3. 입력 파일 구조 (`data/company.yaml`)

| 항목 | 내용 | 비고 |
|---|---|---|
| `company` | 회사명, 슬로건, 설립일, 대표, 연락처, 로고, 브랜드 색상 | `name` 만 필수 |
| `greeting` | 대표 인사말 (`text`, `signer`) | 회사 소개 페이지 상단에 강조 표시 |
| `about` | 회사 소개 문단 (빈 줄로 문단 구분) | |
| `mission`, `vision` | 미션 / 비전 한 줄 | |
| `values` | 핵심 가치 목록 (`title`, `en`, `description`) | 3개면 3열, 그 외 2열 배치 |
| `strategies` | 전략 방향 키워드 (`title`, `description`) | 원형 배지로 표시 |
| `stats` | 주요 숫자 (`label`, `value`) | 3~4개 권장 |
| `history` | 연혁 (`year`, `event`) | |
| `programs_heading` | 프로그램 섹션 제목 (`title`, `en`, `subtitle`) | 비우면 '교육 프로그램' |
| `programs` | 교육 프로그램 (`title`, `summary`, `target`, `duration`, `features`) | 3개 이상이면 차별화 포인트가 마지막 페이지로 이동 |
| `strengths` | 차별화 포인트 (`title`, `description`) | |
| `instructors` | 강사 (`name`, `title`, `photo`, `quote`, `bio`, `specialties`, `career`, `education`, `certifications`) | 한 페이지에 2명씩 배치 |
| `clients` | 주요 고객사 이름 목록 | |
| `testimonials` | 후기 (`quote`, `author`) | |
| `closing` | 마무리 문구 (`headline`, `message`) | |

비어 있거나 지운 항목은 소개서에서 자동으로 빠집니다.

## 4. 디자인 바꾸기

- 색상: `company.brand_color`, `company.accent_color` 만 바꾸면 전체 색이 바뀝니다.
- 레이아웃·문구: `templates/brochure.html.j2` (HTML/PDF), `generate.py` 의 `build_docx` (Word) 를 수정합니다.

## 폴더 구조

```
data/company.yaml          입력 데이터 (여기만 수정하면 됩니다)
assets/                    로고, 강사 사진
templates/brochure.html.j2 HTML/PDF 디자인 템플릿
generate.py                생성 스크립트
output/                    생성 결과물 (자동 커밋)
.github/workflows/build.yml GitHub Actions 자동 생성 설정
```

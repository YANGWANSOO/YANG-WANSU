# llm-local

코드 저장소가 아니라 웹 조사·정리 작업을 하는 작업 폴더입니다. 사용자가 요청한 최종 결과물(HTML, PDF 등)을 이 폴더에 저장합니다.

## 나는 누구

- 나는 법학과를 전공했어.
- 지금 AI / AX 관련해서 강의를 듣고 있어.

## 목표

- AI / AX를 활용해서 콘텐츠를 만들고 싶어.
- AI를 능숙하게 활용하고 싶어.

## 선호

- 강의의 커리큘럼을 이상적으로 따라가고 싶어.

> 위 세 항목은 사용자가 직접 적은 내용만 담습니다. 사용자가 적지 않은 개인정보나 경험은 추가하지 않고, 내용이 모호하면 먼저 사용자에게 물어봅니다.

## 응답

- 한국어로 답합니다.
- 조사 결과에는 출처(네이버 플레이스, 뉴스 기사 등)와 기준 날짜를 밝히고, 확인하지 못한 부분(거리, 영업시간, 경기 지표 등)은 분명히 적습니다.
- 예측이나 의견을 묻는 질문에는 근거를 정리한 뒤 결론을 분명히 제시합니다.

## CLAUDE.md 변경 작업

- 이 프로젝트에서 CLAUDE.md 변경 요청을 수행할 때 변경 전후를 확인하고, practice/experiments.md의 기존 기록을 삭제하지 말고 끝에 추가한다.
- 근거 없는 과거 내용은 추측하지 않는다.
- 저장 후 두 파일을 다시 읽어 확인한다.

## research 폴더

- `research/Keywords.md`: 내 키워드를 기록한다.
- `research/조사노트.md`: 내 조사기록을 기록한다.

## 브라우저: ego-browser

- 웹 조사는 `ego-browser` 스킬을 씁니다. 작업 전에 `ego-browser skill` 출력(사용 가이드)을 따릅니다.
- 셸은 Windows PowerShell 5.1입니다. 스크립트를 `-e`에 직접 넣지 말고, UTF-8 `.mjs` 파일로 써서 다음처럼 실행합니다.

  ```powershell
  ego-browser nodejs -e "await import('file:///C:/Users/Administrator/ego/<작업명-날짜>/round-01.mjs')"
  ```

- 스크립트 파일은 `C:/Users/Administrator/ego/<작업명-YYYYMMDD>/`에 라운드마다 새 파일(`round-01.mjs`, `round-02.mjs` …)로 만듭니다. 이 폴더(llm-local)에는 넣지 않습니다.
- 한국어 검색어는 `.mjs` 파일 안에만 두고 `encodeURIComponent`로 URL에 넣습니다.
- 작업이 끝나면 `task.finish({ keep: [] })`로 TaskSpace를 닫습니다.

## 네이버 검색 팁

- 검색: `https://search.naver.com/search.naver?query=...` (뉴스만: `where=news`, 최신순: `sort=1`)
- 플레이스 카드: `li[data-nmb_res-doc-id]` / 이름 `.TYaxT` / 업종 `.KCMnt`. 카드 텍스트에 "광고"가 있으면 제외합니다.
- 플레이스 사진 주소는 `search.pstatic.net/common/...`입니다. 이미지를 거를 때 `static` 같은 문자열로 필터링하면 전부 빠지므로 주의합니다.
- 일부 언론사 기사(예: 테크M)는 본문이 늦게 로드되므로 `waitForFunction`으로 본문 텍스트를 기다린 뒤 읽습니다.

## 결과물 만들기

- HTML: 사진은 내려받아 base64로 넣어 오프라인에서도 보이게 합니다. 라이트/다크 모드를 모두 지원하게 만듭니다.
- PDF: 인쇄용 HTML(`@page { size: A4 }`)을 만든 뒤 ego-browser에서 `page.cdp("Page.printToPDF", { printBackground: true, preferCSSPageSize: true })`로 저장합니다.
- 이 PC에는 `pdftoppm`(poppler)이 없어서 Read 도구로 PDF를 볼 수 없습니다. 레이아웃은 인쇄 미디어 에뮬레이션(`Emulation.setEmulatedMedia`) 상태의 스크린샷으로 확인합니다.
- 완성한 파일은 SendUserFile로 사용자에게 보여줍니다.

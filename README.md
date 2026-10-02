# DesignBook — 인터랙티브 반도체 설계 교과서

코드에서 마스크까지. 공대 학부생을 위한 한국어 반도체(칩) 설계 학습 사이트입니다.
자매편 [ProcessBook · 반도체 제조 공정 교과서](https://processbook.euiyun.com/)와 같은 형식으로, RTL → 합성 → P&R → 타이밍 → DRC → OPC → 마스크의 흐름을 14개 챕터와 45개 시뮬레이터로 다룹니다.
핵심은 4장입니다. 표준 셀 하나(INV·NAND2·NOR2)의 레이아웃을 레이어별로 뜯어 보고, 절단선을 그어 각 레이어가 ProcessBook 12장 공정 흐름의 어떤 마스크·공정 단계가 되는지 단면으로 확인합니다.

배포 주소: https://designbook.euiyun.com/

## 실행
빌드 과정이 없는 정적 사이트입니다.

```bash
python3 -m http.server 8000   # → http://localhost:8000
```
`index.html`을 브라우저로 바로 열어도 동작합니다. KaTeX와 폰트는 CDN에서 불러오므로 인터넷 연결이 필요합니다.

## 구성
| 장 | 파일 | 주제 |
|---|---|---|
| 01 | chapters/overview.html | 설계 흐름, 추상화 수준, PPA, PDK, 다이 원가·수율 |
| 02 | chapters/rtl.html | Verilog, 조합·순차 논리, blocking/non-blocking, FSM, 검증·커버리지 |
| 03 | chapters/synthesis.html | 카르노 맵·최소화, Liberty, 기술 매핑, 논리적 노력, SDC |
| 04 | chapters/stdcell.html | 표준 셀, 스틱 다이어그램, **레이어 ↔ 마스크 ↔ ProcessBook 공정 대응**, 절단선 단면, 추출·LVS, 셀 면적·밀도 |
| 05 | chapters/place.html | 플로어플랜, HPWL, 이차 배치·어닐링, 합법화 |
| 06 | chapters/cts.html | 클럭 스큐·지연, H-트리, 클럭 트리 합성, 메타안정성 |
| 07 | chapters/route.html | 금속 스택, Lee·A* 미로 배선, PathFinder, 혼잡도, 배선 규칙 |
| 08 | chapters/timing.html | 셋업·홀드·슬랙, 타이밍 그래프, NLDM 보간, 엘모어·리피터, PVT·OCV |
| 09 | chapters/power.html | 동적·누설 전력, 알파 파워·DVFS, 저전력 기법, IR 드롭, 전자 이동 |
| 10 | chapters/signoff.html | 설계 규칙의 공정 근거, DRC, LVS 디버깅, 안테나·밀도 규칙 |
| 11 | chapters/opc.html | 2D 결상·인쇄, 규칙·모델 기반 OPC, 피치별 CD, 리소 친화 설계 |
| 12 | chapters/mask.html | GDSII, 마스크 데이터 준비, 분할·전자빔 묘화, 마스크 수·비용·ECO |
| 13 | chapters/lab.html | 레이아웃 실험실: 그리면 DRC·LVS·단면이 실시간으로, 링크 공유 |
| 14 | chapters/glossary.html | 용어집, 종합 퀴즈(문제 은행에서 20문항) |

공통 코드
- `css/style.css`, `js/common.js` — ProcessBook과 같은 디자인 토큰·내비게이션·차트 헬퍼(전역 `PB`)
- `js/cell.js` — 교육용 가상 PDK **EDU45**의 레이어·설계 규칙·표준 셀, 레이아웃/단면 렌더러, DRC, 추출, LVS (전역 `CELL`)
- `js/optics.js` — 1D 부분 결맞음 결상 엔진 (ProcessBook에서 가져옴, 전역 `OPT`)
- `tools/head.py` — 챕터 `<head>`·사이트맵·JSON-LD 생성기

챕터 작성 규칙은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참고하세요.
시뮬레이터의 수치는 교육용 근사 모델이며, EDU45는 실제 공정이 아닌 가상 PDK입니다. 공정 세대별 수치는 공개 자료 기준의 대략값입니다.

## 배포 (GitHub Pages)
저장소 루트가 그대로 사이트입니다. `CNAME`에 `designbook.euiyun.com`이 들어 있고, `.nojekyll`로 Jekyll 처리를 끕니다.
1. GitHub 저장소 **Settings → Pages**에서 Source를 `Deploy from a branch`, 브랜치 `main` / 폴더 `/ (root)`로 지정합니다.
2. DNS에서 `designbook.euiyun.com`을 `geniuskey.github.io`로 가리키는 **CNAME 레코드**를 추가합니다.
3. Pages 설정에서 Custom domain이 잡히면 **Enforce HTTPS**를 켭니다.

## 라이선스

Copyright (c) 2026 geniuskey and DesignBook contributors

| 적용 대상 | 라이선스 | 재사용 조건 |
|---|---|---|
| JS·CSS·Python·HTML의 실행 코드 | [MIT](LICENSE-MIT) | 수정·재배포·상업적 이용 가능. 저작권 및 라이선스 고지 유지 |
| 교재 본문·그림·문제·해설 | [CC BY 4.0](LICENSE-CC-BY-4.0) | 수정·번역·재배포·상업적 이용 가능. 저작자·출처·라이선스 표시 및 변경 사실 명시 |

공통 스타일·스크립트와 `js/optics.js`는 같은 저자의 ProcessBook(MIT)에서 가져와 고쳐 썼습니다. 자세한 내용은 [라이선스 안내](LICENSE.md)를 참고하세요.

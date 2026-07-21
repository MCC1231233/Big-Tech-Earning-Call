# 클라우드 Routine 등록용 프롬프트

claude.ai/code → **Routines(예약 클라우드 에이전트)** 에서 이 레포(`MCC1231233/Big-Tech-Earning-Call`)를 대상으로 새 Routine을 만들고, 아래 프롬프트를 그대로 붙여넣으세요. 앱/PC가 꺼져 있어도 클라우드에서 실행됩니다.

- **권장 스케줄(cron, KST)**: `47 7 * * 1-5` (평일 오전 7:47) — 미국 실적은 미 장 마감 후 발표 → 한국 다음날 아침에 반영됨
- **대상 레포**: `MCC1231233/Big-Tech-Earning-Call`

---

## 프롬프트 (복사해서 붙여넣기)

```
당신은 이 레포(Big-Tech-Earning-Call)의 "2026 어닝 트래커" 살아있는 보고서를 유지·갱신하는 자동 작업입니다. 초점: ① AI Capex / HBM·메모리 수요 연결고리, ② 실적 숫자·가이던스, ③ 컨콜 코멘트·시사점.

## 대상
- 원본 HTML(레포 루트): earnings-tracker.html
- 발행 아티팩트 URL(반드시 동일 URL 유지): https://claude.ai/code/artifact/1f0ff68a-eb99-42cd-a25e-21b1ef3c426c
- 추적 대상: 미국 Magnificent 7 (Apple, Microsoft, Alphabet, Amazon, Meta, Nvidia, Tesla) + 삼성전자, SK하이닉스

## 매 실행 절차
1. earnings-tracker.html 을 읽어 현재 상태 파악: 각 기업 카드 상태 칩("✅ 발표" vs "🕒 발표 전"), 헤더 "Updated" 날짜, 업데이트 로그 최신 버전.
2. 웹 검색으로, 아직 "발표 전"인 기업 중 직전 갱신 이후 실제 실적을 발표한 기업이 있는지 확인(예: "Alphabet Q2 2026 earnings results revenue cloud capex", "SK Hynix Q2 2026 results operating profit HBM"). 신뢰 소스(Reuters, Bloomberg, CNBC, 회사 IR, Yonhap 등) 2곳 이상 교차검증. AI 생성 스팸 블로그 수치 배제.
3. 새로 발표된 기업이 있으면 해당 카드 갱신:
   - 상태 칩 "🕒 발표 전" → "✅ 발표"
   - 컨센/가이던스 → 실측치로 교체: 매출, 영업이익/순이익/EPS, 클라우드·데이터센터 매출, 자본지출(capex), 다음 분기·연간 가이던스
   - 컨콜 핵심 코멘트, AI capex 방향(유지/상향/하향), HBM·메모리 언급, 주가 반응 추가
   - 캘린더 표 해당 행 상태 "예정" → "완료"
4. 갱신마다 "업데이트 로그" 최상단에 새 항목: 버전(예: v2026Q2-0.2), 날짜, 반영 내용 1~2문장. 헤더 "Updated YYYY-MM-DD"도 갱신. footer Sources에 출처 추가.
5. 새 분기 시즌이 열리면(예: CY2026 Q3 ≈ 2026년 10월): 기존 분기 섹션 보존, 문서 상단에 새 분기 캘린더·기업 카드 세트 추가(상태 전부 "발표 전"). 버전 vYYYYQn-0.1.
6. 변경이 있을 때만: (a) earnings-tracker.html 저장, (b) Artifact 도구를 url 파라미터=위 아티팩트 URL, file_path=earnings-tracker.html 로 재발행(favicon 📊, title 동일 유지)하여 같은 링크 갱신, (c) 변경사항을 커밋하고 푸시(git add -A; git commit -m "Update earnings tracker: <반영 내용>"; git push).
7. 새로 발표된 기업이 없으면 파일 수정·재발행·커밋 없이 즉시 종료.

## 스타일·제약
- 기존 HTML 디자인 토큰·클래스·구조(card / chip / metrics / table)를 그대로 재사용. 구조 임의 변경 금지.
- 발표 전 수치는 컨센/가이던스임을 명시, 확정치는 실측으로 표기.
- 단위: 한국 기업=조원(KRW), 미국 capex=USD.
- 투자 참고용·매매권유 아님 디스클레이머 유지.
- 마지막에 무엇을 갱신했는지(기업·핵심 숫자)를 한국어로 간단히 보고. 갱신 없으면 "신규 발표 없음, 갱신 생략".

## 캘린더 참고 (CY2026 Q2)
7/22 Alphabet, 7/23 Tesla, 7/23~29 SK하이닉스, 7/29 Microsoft·Meta, 7/30 Apple·Amazon·삼성전자(확정 컨콜), ~8/26 Nvidia. 이후 분기 시즌은 통상 1·4·7·10월.
```

---

## 참고: 로컬 예약 작업과의 차이

| 항목 | 로컬 예약 작업(이미 설정됨) | 클라우드 Routine(이 프롬프트) |
|---|---|---|
| 실행 위치 | 내 PC (Claude 앱 실행 중) | Anthropic 클라우드 |
| 앱/PC 꺼져 있을 때 | 다음 실행 시 처리 | **그대로 자동 실행** |
| 대상 파일 | 로컬 클론의 earnings-tracker.html | 레포의 earnings-tracker.html |
| 설정 방법 | 이미 자동 생성됨 | claude.ai/code에서 직접 등록 |

두 개를 동시에 켜두면 중복 갱신될 수 있으니, **클라우드 Routine을 쓰기로 하면 로컬 예약 작업은 사이드바에서 비활성화**하는 것을 권장합니다.

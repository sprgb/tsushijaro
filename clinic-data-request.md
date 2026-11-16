# 제휴 병원 정보 요청서

계약 체결 후 병원에 아래 정보를 받아 `clinics.json`의 `clinics` 배열에 항목 1개로 추가합니다. 코드 수정은 필요 없습니다.

## 1. 앱에 표시·사용되는 항목 (필수)

| 키 | 요청 내용 | 형식 | 예시 |
|---|---|---|---|
| `name` | 병원명 | 문자열 | `도쿄 한인 비대면 클리닉 (신주쿠)` |
| `priceJpy` | 표시할 가격 (세금 포함) | 정수, 엔 (쉼표·단위 없음) | `26940` |
| `plan` | 가격 기준 | 문자열 | `2.5mg 3개월 기준` |
| `support.phone.langs` | 전화 응대 언어 (없으면 빈 배열) | `ko` `en` `ja` 배열 | `["ko","en"]` |
| `support.line.langs` | LINE 응대 언어 (없으면 빈 배열) | `ko` `en` `ja` 배열 | `["ko","en"]` |
| `lineUrl` | LINE 상담 링크 (LINE 응대 시 필수) | `https://line.me/` 로 시작 | `https://line.me/R/ti/p/@clinic` |
| `phone` | 상담 전화번호 (전화 응대 시 필수) | `+국가번호` 뒤 숫자만, 하이픈 없음 | `+81355550101` |
| `shipping.coldDelivery` | 냉장(쿨) 택배 발송 가능 | `true` 이어야 노출 | `true` |
| `shipping.yamatoBranchHold` | 야마토 영업소 留め(지점보관) 발송 가능 | `true` 이어야 노출 | `true` |

## 2. 당사가 입력하는 항목

| 키 | 내용 | 형식 |
|---|---|---|
| `id` | 병원 식별자 (중복 불가) | 영문 소문자·숫자·`_`·`-` |
| `region` | 위치·특징 한 줄 (선택) | 문자열 |
| `highlight` | 배지 문구 (선택) | 문자열 |
| `priceNote` | 가격 보충 (선택) | 문자열 |
| `ad.active` | 광고 노출 여부 | `true` / `false` |
| `ad.start`, `ad.end` | 계약(노출) 기간, 양끝 포함 | `YYYY-MM-DD` |

## 3. 병원에 함께 확인할 항목 (앱에는 아직 표시하지 않음)

- 상담 가능 시간 (JST 기준 요일·시간)
- 접수 후 최단 발송 소요일, 당일 발송 마감 시각
- 가격 유효기간, 변경 시 통보 방법
- 도착일 안내를 받을 담당자 연락처
- 영업소 보관 기한 초과 시 반송·재발송 규정

## 4. 입력 예시

```json
{
  "id": "clinic_example",
  "name": "예시 클리닉",
  "priceJpy": 29800,
  "plan": "2.5mg 3개월 기준",
  "support": { "phone": { "langs": ["ko"] }, "line": { "langs": ["ko", "en"] } },
  "lineUrl": "https://line.me/R/ti/p/@example",
  "phone": "+81312345678",
  "shipping": { "coldDelivery": true, "yamatoBranchHold": true },
  "ad": { "active": true, "start": "2026-12-01", "end": "2027-11-30" }
}
```

## 5. 앱의 적용 규칙

- 노출 조건: `ad.active`가 `true`이고 오늘이 `ad.start`~`ad.end` 안에 있을 때.
- 정렬: `priceJpy` 오름차순 자동. 목록 순서는 신경 쓰지 않아도 됩니다.
- 응대 배지: `support`에서 자동 생성합니다. 전화·LINE 모두 한국어·영어가 되면 "모두 가능"으로 표시합니다.
- 형식 오류나 `id` 중복인 항목은 목록에서 제외되고, 브라우저 콘솔(F12)에 제외 사유가 표시됩니다.

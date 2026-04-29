# QA 실행 요약

- **대상:** https://tassi.alicerabbit.space/landing
- **실행 시각:** 2026-04-29T07:21:49.967Z
- **크롤 페이지 수:** 8 (HTTP 오류 페이지: 7)
- **동작 검증 요약:** 통과 37 / 실패 1 / 무반응 36 / 차단 12
- **스크린샷·네트워크 로그:** `reports/2026-04-29T07-21-49-967Z/artifacts`
- **동작 QA 대시보드:** `report.dashboard.html` — 핵심 성공/실패 흐름 + Lighthouse 평균/페이지별 이동 제공
- **Lighthouse 평균 점수 (8개 페이지):** 성능 43 · 접근성 89 · 권장 96 · SEO 81
- **로그인 페이지 QA 모드:** crawl_only

## 입력값 설정 (로그인 등)

- **입력 방식:** `.env`의 `QA_LOGIN_*` / `QA_EXTRA_*` (KEY=VALUE, JSON 아님). **무엇을 넣을지:** 매 실행 끝에 `config/qa-inputs.REQUEST.env`만 갱신(한글로 변수 설명) — 필요한 키를 `.env`에 그대로 채우면 됩니다.
- **출처:** environment-only
- 로그인용 값 채움 여부: 사용자명/아이디=아니오, 이메일=아니오, 비밀번호=아니오
- 값이 비어 있으면 해당 필드는 **빈 값**으로 탐색합니다. 추가 필드는 `EXTRA_이름=값` 또는 환경 변수 `QA_EXTRA_이름`.

## 로그인 페이지에서 감지된 입력 필드 (참고)

### https://tassi.alicerabbit.space/login/register
- **크롤에서 발견 경로:** https://tassi.alicerabbit.space/
- **비어 있어 탐색에 못 쓴 항목(채우면 재시도 권장):** password (qa-inputs.loginPage.password), username / userId / email 중 하나 (qa-inputs.loginPage.username 등)
| 타입 | name | id | placeholder | 필수 |
|------|------|-----|-------------|------|
| text | managerName | - | 이름을 입력하세요. |  |
| text | phoneNumber | - | 연락처를 숫자만 입력하세요. |  |
| text | email | - | 이메일을 입력하세요. |  |
| password | password | - | 8~20자, 영문·숫자·특수문자 각 1개 이상 |  |
| password | passwordConfirm | - | 비밀번호 확인 |  |
| checkbox | agreeTerms | - |  |  |
| checkbox | agreePersonalInfoCollection | - |  |  |

### https://tassi.alicerabbit.space/login
- **크롤에서 발견 경로:** https://tassi.alicerabbit.space/login/register
- **비어 있어 탐색에 못 쓴 항목(채우면 재시도 권장):** password (qa-inputs.loginPage.password), username / userId / email 중 하나 (qa-inputs.loginPage.username 등)
| 타입 | name | id | placeholder | 필수 |
|------|------|-----|-------------|------|
| email | email | email | 이메일을 입력하세요. |  |
| password | password | password | 비밀번호를 입력하세요. |  |
| checkbox | - | - |  |  |

## 페이지별 동작 결과 (요소 단위)

### https://tassi.alicerabbit.space/

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ⏸️ NO_RESPONSE | interaction | 버튼 「비밀번호 표시」 클릭 | href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 버튼 「로그인」 클릭 | 클릭 후 반응 확인 (오류·경고 스타일 UI, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「카카오로 시작하기」 클릭 | 클릭 후 반응 확인 (URL 변경, 제목 변경, 알림·라이브 영역, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 링크 「회원가입 하기」 클릭 | 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ❌ FAIL | form | form | XHR/fetch error during submit: [{"url":"https://tassi-api.alicerabbit.space:3004/auth/login","method":"POST","status":40 |
| ✅ PASS | form | form | Validation feedback present (intent=true, nativeInvalid=true). Messages: 이메일 올바르지 않습니다. \| 이메일 주소에 '@'를 포함해 주세요. 'not-an |

### https://tassi.alicerabbit.space/landing

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ⏸️ NO_RESPONSE | interaction | 버튼 「서비스소개」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 버튼 「이용요금」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「무료 견적신청」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ⏸️ NO_RESPONSE | interaction | 버튼 「무료 견적 알아보기」 클릭 (1/4 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「무료 견적 알아보기」 클릭 (2/4 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「지금 신청하기」 클릭 (1/3 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「무료 견적 알아보기」 클릭 (3/4 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「무료 견적 알아보기」 클릭 (4/4 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「지금 신청하기」 클릭 (2/3 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「지금 신청하기」 클릭 (3/3 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://pf.kakao.com/_PRFAG |
| ✅ PASS | interaction | 링크 「이용약관」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ✅ PASS | interaction | 링크 「개인정보 처리방침」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| 🚫 BLOCKED | form | form | HTTP 403 |

### https://tassi.alicerabbit.space/landing/estimate

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ✅ PASS | interaction | 버튼 「서비스소개」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「이용요금」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ⏸️ NO_RESPONSE | interaction | 버튼 「무료 견적신청」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「기능 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「사용자 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「클라이언트 성능 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「네트워크 성능 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「벤치마크 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「로그 확인 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「인증 테스트」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「앱 + 웹」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「앱」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「모바일 웹」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「웹」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「5 일」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「15 일」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「30 일」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「60 일」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「정기 구독 (연 단위)」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「ASAP」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| 🚫 BLOCKED | form | form | HTTP 403 |

### https://tassi.alicerabbit.space/landing/terms-of-service

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ✅ PASS | interaction | 버튼 「서비스소개」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「이용요금」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「무료 견적신청」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| 🚫 BLOCKED | form | form | HTTP 403 |

### https://tassi.alicerabbit.space/landing/privacy-policy

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ✅ PASS | interaction | 버튼 「서비스소개」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「이용요금」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「무료 견적신청」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 링크 「privacy.kisa.or.kr」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://privacy.kisa.or.kr/main.do |
| ✅ PASS | interaction | 링크 「www.kopico.go.kr」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://www.kopico.go.kr/main/main.do |
| ✅ PASS | interaction | 링크 「www.simpan.go.kr」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: chrome-error://chromewebdata/ |
| ✅ PASS | interaction | 링크 「www.spo.go.kr」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: chrome-error://chromewebdata/ |
| ✅ PASS | interaction | 링크 「ecrm.cyber.go.kr」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://ecrm.police.go.kr/minwon/main |
| 🚫 BLOCKED | form | form | HTTP 403 |

### https://tassi.alicerabbit.space/landing/pricing

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ✅ PASS | interaction | 버튼 「서비스소개」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ⏸️ NO_RESPONSE | interaction | 버튼 「이용요금」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 버튼 「무료 견적신청」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 본문·안내 문구 변화). |
| ⏸️ NO_RESPONSE | interaction | 버튼 「문의하기」 클릭 (1/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「문의하기」 클릭 (2/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://pf.kakao.com/_PRFAG |
| ⏸️ NO_RESPONSE | interaction | 버튼 「무료 견적 받아보기」 클릭 (1/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「무료 견적 받아보기」 클릭 (2/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「지금 신청하기」 클릭 (1/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 링크 「지금 신청하기」 클릭 (2/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 외부 링크가 새 탭에서 열림: https://pf.kakao.com/_PRFAG |
| ✅ PASS | interaction | 링크 「이용약관」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ✅ PASS | interaction | 링크 「개인정보 처리방침」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| 🚫 BLOCKED | form | form | HTTP 403 |

### https://tassi.alicerabbit.space/login/register

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ✅ PASS | interaction | 링크 「로그인으로 돌아가기」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「비밀번호 표시」 클릭 (1/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ⏸️ NO_RESPONSE | interaction | 버튼 「비밀번호 표시」 클릭 (2/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 버튼 「보기」 클릭 (1/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (모달·대화상자, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「보기」 클릭 (2/2 동일 라벨) | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (모달·대화상자, 본문·안내 문구 변화). |
| 🚫 BLOCKED | form | form | HTTP 403 |
| 🚫 BLOCKED | login | Click: 로그인으로 돌아가기 | Page load HTTP 403 |
| ⏸️ NO_RESPONSE | login | login_group | No matching control found for this category. |
| ⏸️ NO_RESPONSE | login | login_group | No matching control found for this category. |
| 🚫 BLOCKED | login | Click: 비밀번호 표시 | Page load HTTP 403 |
| ⏸️ NO_RESPONSE | login | login_group | No matching control found for this category. |

### https://tassi.alicerabbit.space/login

| 결과 | 구분 | 동작/요소 | 비고 |
|------|------|-----------|------|
| ⏸️ NO_RESPONSE | interaction | 버튼 「비밀번호 표시」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] href/data-to 없고 화면·모달·알림 변화 없음 — 필요 시 interaction-rules로 경로·제목 기대값을 지정. |
| ✅ PASS | interaction | 버튼 「로그인」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (오류·경고 스타일 UI, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 버튼 「카카오로 시작하기」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 클릭 후 반응 확인 (URL 변경, 제목 변경, 알림·라이브 영역, 본문·안내 문구 변화). |
| ✅ PASS | interaction | 링크 「회원가입 하기」 클릭 | [HTTP 403 응답이지만 페이지 렌더링/클릭 가능] 이동 후 URL(경로·쿼리)가 링크 기대와 일치함. |
| 🚫 BLOCKED | form | form | HTTP 403 |
| 🚫 BLOCKED | login | Click: 로그인 | Page load HTTP 403 |
| 🚫 BLOCKED | login | Click: 회원가입 하기 | Page load HTTP 403 |
| ⏸️ NO_RESPONSE | login | login_group | No matching control found for this category. |
| 🚫 BLOCKED | login | Click: 비밀번호 표시 | Page load HTTP 403 |
| ⏸️ NO_RESPONSE | login | login_group | No matching control found for this category. |

## 우선 확인할 실패·차단 (최대 15건)

- **FAIL** [form] https://tassi.alicerabbit.space/ — form: XHR/fetch error during submit: [{"url":"https://tassi-api.alicerabbit.space:3004/auth/login","method":"POST","status":401,"resourceType":"xhr","timestamp":1777447573669,"bodySnippet":null}]
- **BLOCKED** [form] https://tassi.alicerabbit.space/landing — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/landing/estimate — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/landing/terms-of-service — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/landing/privacy-policy — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/landing/pricing — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/login/register — form: HTTP 403
- **BLOCKED** [form] https://tassi.alicerabbit.space/login — form: HTTP 403
- **BLOCKED** [login] https://tassi.alicerabbit.space/login/register — Click: 로그인으로 돌아가기: Page load HTTP 403
- **BLOCKED** [login] https://tassi.alicerabbit.space/login/register — Click: 비밀번호 표시: Page load HTTP 403
- **BLOCKED** [login] https://tassi.alicerabbit.space/login — Click: 로그인: Page load HTTP 403
- **BLOCKED** [login] https://tassi.alicerabbit.space/login — Click: 회원가입 하기: Page load HTTP 403
- **BLOCKED** [login] https://tassi.alicerabbit.space/login — Click: 비밀번호 표시: Page load HTTP 403

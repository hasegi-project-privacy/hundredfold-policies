# hundredfold-policies

[백일백번 (Hundredfold)] 앱의 공개 정책 페이지. (com.hasegi.hundredfold)

- [개인정보 처리방침](https://hasegi-project-privacy.github.io/hundredfold-policies/privacy/)
- [이용약관](https://hasegi-project-privacy.github.io/hundredfold-policies/terms/)
- [고객 지원](https://hasegi-project-privacy.github.io/hundredfold-policies/support/)
- English: [Privacy Policy](https://hasegi-project-privacy.github.io/hundredfold-policies/privacy/#en) · [Terms of Service](https://hasegi-project-privacy.github.io/hundredfold-policies/terms/#en) · [Support](https://hasegi-project-privacy.github.io/hundredfold-policies/support/#en)

시행·최종 개정 = 문서 안 `{{발행일}}` 자리표시(**발행 직전에 그날 날짜를 넣는다** — 아래 「발행 전 확인할 것」 첫 줄) · v1.0 (로컬 커밋만 — **미발행**, 레포 생성·발행은 대표님)

앱 쪽 상수 `lib/utils/legal_urls.dart` 가 `…/privacy/` `…/terms/` `…/support/` 디렉터리 주소를 가리키므로 파일은 `privacy/index.html` 꼴이다(churchrank-policies 구조). 한국어·영어는 한 페이지 안에 있고 영어는 `#en` 앵커다.

## 이 앱의 특이점 (표준 패턴과 다른 곳)

근거: 계획서 `~/.claude/plans/luminous-wishing-piglet.md` §5.7·§6.9, 앱 코드(2026-10-19, 미커밋 작업 트리 — `lib/core/rules/age_gate.dart` · `lib/core/backup/backup_service.dart` · `lib/data/media_paths.dart` · `lib/core/ads/consent_gate.dart` · `lib/core/iap/premium_service.dart` · `ios/Runner/Info.plist` · `assets/locales/ko.json`), 양식 답변 정본은 앱 저장소 `docs/STORE_PRIVACY.md`.

- **회원가입·로그인·분석 SDK·푸시 서버 없음.** 목표·기록·손글씨 획·감사·증거·녹음·사진은 기기에만(sqflite + Documents/media). 백업은 사용자가 내보내는 zip(기록·설정·**녹음·사진 포함** — `backup_service.dart` schema 2) + 운영체제 자동 백업.
- **마이크·음성 인식**: 횟수만 세고 소리는 저장·전송하지 않음. 기기 내 인식 우선, **[설정 - 말하기 - 온라인 인식 허용]을 켠 경우에만** 기기 내 인식이 없을 때 OS 제공자(Apple/Google) 서버 인식 → 처리방침 §01·§04 에 조건형으로 적었고 `NSSpeechRecognitionUsageDescription` 도 같은 뜻.
- **손글씨**: ML Kit Digital Ink 기기 내 인식, 모델(약 20MB) 첫 사용 때 다운로드(획은 전송 0). **종이 모드 공책 사진은 줄 수를 센 뒤 바로 지운다**(앱 `lib/features/write/paper_mode_panel.dart` — 세기가 끝나면 `finally` 에서 `discardPhoto`, 화면을 떠나도 지움. 앨범에서 고른 경우 지우는 것은 image_picker 가 만든 임시 사본이고 사용자의 원본 사진은 그대로) → 처리방침 §01 「손글씨」·「사진」 행.
- **내 목소리 녹음**: 사용자가 직접 녹음(문장 판마다 1개) → Documents/media/recordings, 전송 0.
- **출생 연도**: 저장하지 않고 「통과」·「18세 미만일 수 있음」 두 판정값만(`age_gate.dart`). **만 14세 미만 이용 불가**, 18세 미만일 수 있으면 비개인화 광고·ATT 생략.
- **광고**: AdMob 5종(배너·전면·앱오픈·보상형·종료 네이티브) + UMP(EEA·영국·스위스 동의 창, 미국 주 안내 창 — 조건형) + ATT. Google 의 SDK 공개 자료(2026-10-16 갱신)대로 광고 식별자·앱 상호작용·IP(대략 위치)·진단이 Google 로 간다고 §01 표에 적었다.
- **AI 코치(선택, 기본 꺼짐)**: 목표 문장(또는 「지금 원하는 것」 한 줄)·카테고리·화면 언어(손님 한 줄은 **손님 종류만** — 메모는 앱 a9bcbbe 부터 보내지 않음)만 Cloudflare Worker → Anthropic. 운영자 본문 저장 0. **다만 Worker D1 에 설치 ID(서버 발급 무작위 UUID, HMAC 서명 토큰)·플랫폼·앱 버전·발급 시각·설치 키 해시·사용 횟수(`coach_usage`)·일별 전체 지출(`spend`)·남용 차단(`abuse`)·해시한 IP 속도 제한 카운터(최대 2시간)를 저장하고, 요청당 로그 1줄(route·설치 ID 앞 8자·free|pro·model·토큰·USD·ms·status)을 남긴다**(앱 `worker/README.md` §5·`worker/schema.sql`) → 처리방침 §01 「AI 코치 사용 기록」 행·§03·§04·§05 「서버 기록 삭제」(설치 번호는 앱 [설정 - AI 코치 - 설치 번호]에서 복사). Anthropic 은 상용 약관상 학습 미사용 + 입력·출력 30일 내 자동 삭제(정책 위반 의심·법적 보존 예외) — **2026-10-19 Anthropic 공개 문서 기준**(아래 확인 항목). 생성형 AI 고지·「AI」 표시·신고 버튼.
- **결제**: Pro 월·연 구독(자동 갱신) + 평생권, 자체 서버 검증 없음(영수증은 기기 밖으로 안 나감) → 처리방침에 「구매 상태 표시용 사본」만. 가격 숫자는 어디에도 적지 않았다(약관 §07 도 「결제 화면의 스토어 가격」).
- **면책**: 효과 보장 없음·의료/심리 치료 아님·위기어 안내는 응급 대체 아님·증서는 「Hasegi Inc. 자체 발급·공인 아님」·AI 회사/출판사/저자와 제휴 없음.
- **책 조어 0**: `hundredfold/tool/term_gate.py` 의 TERMS 로 이 레포 전체를 검사해 0건(2026-10-19).

## 발행 절차 (대표님 승인 뒤에만)

`GUIDE_app_legal_setup.md` §1·§3 그대로. `singongjineung` 은 org 에 레포를 만들 수 없다.

1. https://github.com/organizations/hasegi-project-privacy/repositories/new → Name `hundredfold-policies` · Public · 초기화 옵션 전부 비움 → Create
2. Settings → Access → Collaborators → `singongjineung` · Write 로 추가 (Claude 쪽에서 `gh api user/repository_invitations … -X PATCH` 로 수락)
3. 이 폴더에서 `git remote add origin https://github.com/hasegi-project-privacy/hundredfold-policies.git && git push -u origin main`
4. Settings → Pages → Deploy from a branch · `main` · `/ (root)` → Save (admin 권한이라 대표님)
5. 약 1분 뒤 `curl -sI https://hasegi-project-privacy.github.io/hundredfold-policies/privacy/ | head -3` → `HTTP/2 200`. terms/·support/ 도 같이.

(aiclassroom 때처럼 이 맥의 gh 에 `hasegi-project-privacy` 사용자 계정이 인증돼 있으면 `gh auth switch --user hasegi-project-privacy` → `gh repo create hasegi-project-privacy/hundredfold-policies --public --source=. --remote=origin --push` → `gh api -X POST repos/hasegi-project-privacy/hundredfold-policies/pages -f "source[branch]=main" -f "source[path]=/"` → `gh auth switch --user singongjineung` 네 줄로 끝난다. **승인 전에는 실행하지 않는다.**)

## app-ads.txt

이 저장소에는 두지 않는다. Play/ASC 등록정보의 개발자 웹사이트를 `https://hasegi-project-privacy.github.io/` 로 두면 루트 저장소의 app-ads.txt 가 AdMob 인증을 담당한다.

## 발행 전 확인할 것

- [ ] **발행 직전 날짜 넣기** — 날짜는 미리 박지 않고 `{{발행일}}` 로 두었다(privacy KO/EN meta·§09, terms KO/EN meta, support KO/EN). 발행하는 날: `sed -i '' 's/{{발행일}}/YYYY-MM-DD/g' privacy/index.html terms/index.html support/index.html` → `grep -rn '{{' privacy terms support` 0 → 이 README 맨 위 줄도 그 날짜로. 처리방침 §03 의 「2026-10-19 Anthropic 공개 정책 기준」은 확인일이라 별개(다음 항목에서 다시 보고 그날로 바꾼다).
- [ ] **Anthropic 보존 정책 재확인(사실 확인 필요).** 처리방침 §03·§04(KO·EN)는 「모델 학습 미사용(상용 약관 B 조 "Anthropic may not train models on Customer Content from Services", 2025-06-17 발효판)」과 「입력·출력 수신 후 30일 안에 자동 삭제(Anthropic Privacy Center 「How long do you store my organization's data?」, 2026-07-01 갱신 — 정책 위반 의심은 최대 2년·법적 보존 예외)」를 적었다. 발행 직전에 두 문서를 다시 열어 문장이 그대로인지 보고, Worker 가 Files API 같은 「보존이 더 긴 서비스」를 쓰지 않는지 확인한다. 제로 데이터 보존(ZDR) 계약을 맺으면 「저장하지 않는다」로 바꿀 수 있다.
- [x] **AI 코치 Worker 가 실제로 보내는·남기는 항목**(2026-10-20 대조): 앱 `lib/core/coach/worker_coach.dart` 요청 JSON = sentences `{category, wish, lang, pro_claimed}` · bigger `{sentence, lang, pro_claimed}` · guest `{guest, lang, pro_claimed}`(메모 없음) · install `{platform, app_version, install_key, prev_token?, install_id?}`. 서버 저장은 위 「AI 코치」 줄. §01 에 「AI 코치 사용 기록」 행을 더하고 §01 요청 행·접속 정보 행, §02, §03, §04 Cloudflare·Anthropic 행, §05 「서버 기록 삭제」, §06 을 고쳤다(KO·EN). Play 양식은 앱 `docs/STORE_PRIVACY.md` §5.1(삭제 요청 = 예·메일).
- [ ] **Cloudflare 로그 보관 일수**: invocation log 는 껐다(`worker/wrangler.jsonc` `invocation_logs:false`, `tool/check_config.mjs` 가 배포 전 확인). 남는 것은 Worker 가 `console.log` 로 쓰는 요청당 1줄(Workers Logs)이고 처리방침 §03 은 「요금제에 따라 3일 또는 7일」 — 배포 계정 요금제의 Workers Logs 보관 일수가 이와 같은지 대시보드에서 본다.
- [x] **신고 버튼**: AI 결과마다 「부적절해요」 → 로컬 기록 + 메일 초안(앱 M7a 10/20, `lib/core/coach/coach_report.dart`). 문안 「제안 옆의 신고 버튼 또는 메일」과 맞다.
- [x] **백업 zip 의 녹음·사진 포함**: 앱 문구를 코드 기준(포함)으로 고쳤다(M7b 10/20). 처리방침 §01·약관 §02 와 같다. Android 자동 백업 범위(DB·설정·녹음·사진, 내보낸 zip·캐시 제외)는 처리방침 §01 아래 문단에 적었다(앱 `android/app/src/main/res/xml/backup_rules.xml`·`data_extraction_rules.xml`).
- [x] **종이 모드 사진의 보관 여부**: 세고 즉시 지운다(코드 확인 10/20) → §01 「손글씨」·「사진」 행에 「센 뒤 바로 지우며 저장·전송하지 않음」(KO·EN).
- [ ] **온라인 인식 허용 설정**이 출시 빌드에 남아 있는지(`settings.speak_online`). 빠지면 §01·§04·§05 의 조건 문장을 「기기 내 인식만」으로 바꾼다.
- [ ] 위젯 문장 숨김 토글·동작 줄이기·광고 개인정보 설정 메뉴 이름이 출시 빌드의 화면 글자와 같은지(문서는 `assets/locales/ko.json`·`en.json` 2026-10-20 기준으로 맞췄다: 설정 - 말하기 - 온라인 인식 허용 / 설정 - AI 코치 - AI 코치 쓰기·설치 번호 / 설정 - 백업 - 백업 내보내기·백업 복원 / 설정 - 정보 - 광고 개인정보 설정 / 설정 - 화면과 느낌 - 그림 움직임 / 설정 - Pro - 구매 복원. EN: Settings - Backup / Display & feel - Scene motion / AI coach - Install number). 발행 직전에 한 번 더.
- [ ] **법무 판단 칸**(사실만 적었다): ① §04 국외 이전 문단이 법정 기재 사항(이전받는 자의 연락처·이전 국가·거부 방법과 불이익)을 다 채웠다고 보지 않는다 ② 광고 식별자의 Google 「공유」를 제3자 제공으로 볼지 ③ 영문판의 「13세 미만」 병기(COPPA)와 한국 14세 기준의 병존 ④ EEA·영국을 열기 전 영문 처리방침의 유럽 독자용 칸(처리 근거·감독기관·역내 대리인) 필요 여부 — aiclassroom README 의 같은 항목.
- [ ] 스토어 ID: index.html 주석의 Apple 숫자 ID·Play 링크.
- [ ] 발행 후 `curl -sI …/privacy/` `…/terms/` `…/support/` 200 → 앱 `lib/utils/legal_urls.dart` 와 문자 단위로 같은지.

## 운영자

- Hasegi Inc. · 개발팀
- 문의: hasegi.cs@gmail.com

© 2026 Hasegi Inc.

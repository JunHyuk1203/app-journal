# 작은 도구들

JunHyuk1203의 공개 앱을 소개하는 한국어 블로그입니다. 종이 느낌의 반응형 화면, 앱 실행 디렉터리, 개별 사용 가이드 6편, 소개·문의·개인정보처리방침·이용 안내 페이지를 제공합니다. 콘텐츠는 초기 편집본이며 운영자가 실제 앱 상태와 설명을 검토하고 유지해야 합니다.

## 실행과 수정

Node.js 24에서 별도 패키지 설치 없이 실행합니다.

```powershell
npm run build
npm run check
npm run dev
```

미리보기: http://127.0.0.1:4173

- `content.mjs`: 앱 정보와 글 본문. 앱 추가 시 기존 항목과 같은 형식으로 추가합니다.
- `site.config.mjs`: 최종 도메인, 공개 문의 이메일, 광고 설정.
- `build.mjs`: 별도 주소의 정적 HTML, 구조화 데이터, 사이트맵 생성.
- `styles.css`, `app.js`: 화면 및 검색·분류·주소 복사.
- `dist/`: 배포 가능한 결과물. 빌드할 때 갱신됩니다.

브라우저 JavaScript를 꺼도 글과 앱 링크를 읽을 수 있습니다. 검색은 페이지 안에서만 처리하며 광고·분석 스크립트는 현재 비활성 상태입니다.

## main.junhyuk.site로 공개하기

현재 설정의 canonical 및 사이트맵은 `https://main.junhyuk.site`를 기준으로 합니다. DNS와 HTTPS 배포 상태는 GitHub Pages 설정에서 확인하세요. Sites 배포는 소유자 전용 검토본이며, AdSense 심사에는 로그인 없이 접근 가능한 공개 배포가 필요합니다.

이 저장소는 GitHub Pages 배포용 소스를 담고 있습니다. 기존 사이트를 교체할 경우 현재 파일을 먼저 검토해야 합니다.

1. 저장소 Settings → Pages → Build and deployment에서 Source를 GitHub Actions로 선택합니다.
2. 포함된 `.github/workflows/pages.yml`이 main 브랜치에서 빌드하고 `dist`를 배포합니다.
3. Pages의 Custom domain에 `main.junhyuk.site`를 저장합니다.
4. DNS 관리 화면에서 `main` 호스트의 CNAME 대상을 `junhyuk1203.github.io`로 지정합니다. 기존 main 레코드가 있다면 용도를 확인한 뒤 변경하세요. DNS 제공업체에 따라 자동으로 붙는 도메인 접미사와 프록시 설정에 주의하세요.
5. 도메인 확인과 인증서 발급이 완료되면 Enforce HTTPS를 활성화합니다.
6. 첫 화면, 개별 글, `/robots.txt`, `/sitemap.xml`, `/ads.txt`를 공개 주소에서 확인합니다.

`dist/CNAME`도 생성하지만 GitHub Actions 방식에서는 Pages 설정의 Custom domain을 별도로 저장해야 합니다. GitHub의 도메인 소유권 검증 TXT가 요구되면 계정 화면에 표시된 실제 값을 DNS에 추가하세요.

공식 안내: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site

## AdSense 준비 상태

AdSense 계정의 게시자 ID가 설정되어 있으며 소유권 확인용 메타 태그와 `ads.txt`가 생성됩니다. 계정 승인 및 동의 설정 전이므로 `adsense.enabled`는 false이고 광고 단위 ID는 비어 있습니다. 광고 스크립트는 삽입하지 않습니다.

준비한 항목: 검색 가능한 개별 HTML 글, 모바일 레이아웃, 설명·문의·정책 페이지, canonical·Open Graph·BlogPosting·사이트맵, 본문의 광고 단위 삽입 위치. 메뉴와 문의 화면 등에는 광고를 넣지 않습니다.

실제 신청 전 할 일:

1. 본인 도메인으로 공개하고 크롤러와 일반 방문자의 접근을 확인합니다.
2. 게시된 모든 앱 링크와 글의 내용, 운영자 연락 방법을 검토합니다. 글 수나 분량만으로 승인이 결정되지는 않습니다.
3. AdSense에 가입해 실제 사이트를 추가하고, 계정에서 제공하는 소유권 확인 방식을 진행합니다. `site.config.mjs`에 실제 `ca-pub-` 게시자 ID를 넣고 빌드하면 `ads.txt`가 생성됩니다. 광고를 켜기 전에도 이 파일을 사용할 수 있습니다.
4. 광고를 게재하기 전에 실제 데이터 처리와 호스팅에 맞게 개인정보처리방침을 검토합니다. 현재 개인정보 안내는 광고가 꺼진 상태를 명시합니다.
5. EEA·영국·스위스 사용자에 대한 광고를 운영하면 Google 인증 CMP를 구성합니다. `consentReady`는 단순 플래그이며 동의 기능 자체가 아닙니다. 실제 CMP 스크립트와 동의 전달을 먼저 구현·검증해야 합니다.
6. 승인과 동의 구성이 완료된 후 실제 광고 단위 번호를 `articleSlot`에 넣고 `consentReady`, `enabled`를 true로 변경해 빌드합니다. 개별 글 본문 안에 '광고' 표시와 광고 단위가 추가되며 정책 페이지도 광고 운영 문구로 갱신됩니다.

설정만으로 승인·광고 게재·수익을 보장하지 않습니다. Google의 콘텐츠·광고 정책과 계정 심사가 별도로 적용됩니다.

- 콘텐츠 및 탐색: https://support.google.com/adsense/answer/81904?hl=ko
- ads.txt: https://support.google.com/adsense/answer/12171612?hl=ko
- CMP: https://support.google.com/adsense/answer/13554116?hl=ko

## 출처

2026-10-27 확인한 JunHyuk1203의 superword, easyupload, apigemini, modelcheck 저장소 README, weather·pdfchanger의 index.html과 기존 junhyuk1203.github.io의 앱 링크를 바탕으로 작성했습니다. 앱 소스 코드는 재사용하지 않았습니다. 각 글의 마지막에서 해당 저장소의 근거 파일을 확인할 수 있습니다.

블로그 글의 발행일은 이 블로그 초안 작성일이며, 개별 앱의 출시일을 뜻하지 않습니다. 카드 그림은 실제 앱 화면 캡처가 아닌, 이 블로그를 위해 CSS로 구성한 설명용 일러스트입니다.

## 공개 앱 링크 확인 결과

2026-10-27 확인에서 Superword와 PDF·GeminiChat·날씨·모델 감지기 앱의 HTTPS 주소가 모두 200 응답을 반환했습니다. 상위 도메인의 A 레코드 설정 직후에는 DNS 캐시가 갱신되기까지 일부 환경에서 접속이 지연될 수 있습니다.

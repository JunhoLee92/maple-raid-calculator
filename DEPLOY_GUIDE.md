# 배포 가이드

이 폴더는 정적 사이트로 바로 배포할 수 있게 구성했습니다.

## 파일

- `index.html`: 실제 사이트 진입 파일
- `ads.txt`: Google AdSense용 파일
- `privacy.html`: 개인정보처리방침 페이지

## 1. 사이트 공개

가장 간단한 방법은 아래 셋 중 하나입니다.

1. GitHub Pages
2. Netlify
3. Vercel

셋 다 `index.html` 하나만 있어도 배포할 수 있습니다.

## 2. 도메인 연결

광고 승인을 받으려면 보통 본인 도메인이 있는 편이 유리합니다.

예시:

- `https://raidcalc.kr`
- `https://maple-settlement.com`

현재 기본 주소는 `https://maple-raid-calculator.vercel.app/`로 설정해 두었습니다.
나중에 커스텀 도메인을 연결한 뒤 `index.html` 안의 아래 값을 새 주소로 바꾸세요.

- `window.SITE_CONFIG.siteUrl`
- `<meta property="og:url" ...>`
- `<link rel="canonical" ...>`

## 3. Firebase 공유 기능 활성화

현재 `index.html`은 Firebase 설정이 비어 있어도 계산기 자체는 동작합니다.
다른 사람이 같은 링크로 같은 데이터를 보게 하려면 Firestore를 연결해야 합니다.

해야 할 일:

1. Firebase 프로젝트 생성
2. Firestore Database 활성화
3. 웹 앱 추가
4. `index.html`의 `window.SITE_CONFIG.firebase` 값을 실제 값으로 교체

예시 위치:

```html
window.SITE_CONFIG = {
  siteUrl: "https://maple-raid-calculator.vercel.app/",
  adsenseClient: "ca-pub-XXXXXXXXXXXXXXXX",
  firebase: {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
  }
};
```

추가로 Firestore 보안 규칙도 설정해야 합니다.
지금 구조는 `rooms/{공유코드}` 문서를 읽고 씁니다.

## 4. Google AdSense 연결

다음 3곳을 실제 값으로 교체하세요.

1. `index.html` 상단 AdSense script의 `client=ca-pub-...`
2. 광고 `<ins class="adsbygoogle">`의 `data-ad-client`
3. `ads.txt`의 `pub-XXXXXXXXXXXXXXXX`

그리고 광고 슬롯 ID도 실제 값으로 바꾸세요.

```html
<ins
  class="adsbygoogle"
  style="display:block"
  data-ad-client="ca-pub-XXXXXXXXXXXXXXXX"
  data-ad-slot="1234567890"
  data-ad-format="auto"
  data-full-width-responsive="true"
></ins>
```

## 5. AdSense 승인 전 체크

- 사이트가 실제 도메인에서 열려야 함
- 모바일에서도 정상 동작해야 함
- `ads.txt`가 루트 경로에서 열려야 함
- 개인정보처리방침 페이지를 추가하는 편이 좋음
- Search Console 등록 권장

## 6. 추천 배포 순서

1. GitHub에 업로드
2. Vercel 또는 Netlify로 배포
3. 커스텀 도메인 연결
4. Firebase 설정
5. AdSense 신청

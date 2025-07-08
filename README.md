# 쉽게 예쁜 모바일 청첩장 만들기

이 프로젝트는 모바일에 최적화된 웨딩 청첩장 웹사이트입니다. 반응형 디자인으로 모든 기기에서 최적의 사용자 경험을 제공합니다. 직접 쓰기 위해 만들었습니다.
유용하다면 Fork와 Star 해주세요~

## 샘플 사이트
[사이트 바로가기](https://wedding-invitation-neon-five.vercel.app)

## 개발자 후원하기

이 프로젝트가 도움이 되었다면, 개발자에게 커피 한 잔으로 응원해주세요! 여러분의 후원이 더 나은 오픈 소스 프로젝트를 만드는 데 큰 도움이 됩니다.

[☕ 개발자에게 커피 사주기](https://qr.kakaopay.com/Ej8soKM2U)

## 기능

- 세로형 전체 화면 메인 이미지와 결혼 정보 표시
- 섹션별로 구분된 결혼 관련 정보 제공 (동적 배경색 시스템)
- 실시간 D-day 카운트다운 타이머 (숫자 고정 너비 적용)
- 네이버 지도 API를 활용한 위치 정보 제공 및 길찾기 기능
- 이미지 갤러리 (스크롤/그리드 레이아웃 선택 가능, 위치 설정 가능, 터치 스와이프 지원)
- 참석 여부 응답 시스템 및 Slack 연동 알림 (활성화/비활성화 설정 가능)
- 계좌번호 아코디언 펼침/접기 기능 및 복사 기능
- 신랑/신부측 배차 안내 아코디언 기능
- 고급스러운 버튼 리플 효과 및 터치 피드백
- Web Share API를 활용한 청첩장 공유 기능
- URL 복사 기능

## 기술 스택

- Next.js 14 (App Router)
- TypeScript
- styled-components
- 네이버 지도 API
- Web Share API
- Slack Webhook API

## 시작하기

### 환경 설정

1. 저장소 클론하기:
   ```
   git clone https://github.com/your-username/wedding-invitation.git
   cd wedding-invitation
   ```

2. 의존성 설치:
   ```
   npm install
   ```

3. `.env.local` 파일 생성:
   ```
   # 네이버 지도 API 클라이언트 ID
   NEXT_PUBLIC_NAVER_MAP_CLIENT_ID=your_naver_map_client_id
   
   # Slack Webhook URL (참석 여부 알림용)
   NEXT_PUBLIC_SLACK_WEBHOOK_URL=your_slack_webhook_url
   
   # 사이트 URL (배포 후)
   NEXT_PUBLIC_SITE_URL=https://your-wedding-site.com
   ```

4. 개발 서버 실행:
   ```
   npm run dev
   ```

## 콘텐츠 수정하기

청첩장의 내용을 수정하려면 `src/config/wedding-config.ts` 파일을 편집하세요. 이 파일에서 다음과 같은 정보를 수정할 수 있습니다:

- 메타 정보 (타이틀, 설명 등)
- 메인 화면 정보 (제목, 이미지, 날짜, 장소)
- 일정
- 장소 정보 (지도 좌표, 길찾기 URL, 교통 정보 등)
- 신랑/신부측 배차 안내 정보
- 갤러리 이미지 및 레이아웃 설정
- 초대의 말씀
- 계좌 정보
- **RSVP 설정 (활성화/비활성화)**
- Slack 알림 설정

### RSVP 섹션 설정

참석 여부 회신 섹션을 표시하거나 숨길 수 있습니다:

```typescript
rsvp: {
  enabled: true, // RSVP 섹션 표시 여부 (true: 표시, false: 숨김)
  showMealOption: false, // 식사 여부 입력 옵션 표시 여부
},
```

- `enabled: false`로 설정하면 RSVP 섹션이 완전히 숨겨집니다.
- 이 경우 동적 배경색 시스템이 자동으로 조정되어 다른 섹션들의 색상이 자연스럽게 번갈아 나타납니다.

## 네이버 지도 API 설정하기

### 네이버 클라우드 플랫폼 API 키 발급 방법

1. [네이버 클라우드 플랫폼](https://www.ncloud.com/)에 가입 및 로그인합니다.
2. 콘솔에서 "Products & Services" > "AI·Application Service" > "Maps"로 이동합니다.
3. "Application 등록" 버튼을 클릭하여 새 애플리케이션을 생성합니다.
4. 애플리케이션 이름을 입력하고, "WEB 환경"을 선택합니다.
5. 웹 서비스 URL에 배포할 도메인 또는 개발용 URL([http://localhost:3000](http://localhost:3000))을 등록합니다.
   - 로컬 개발 환경: http://localhost:3000
   - 실제 배포 환경: https://your-wedding-site.com
6. 인증 정보(Client ID)를 `.env.local` 파일의 `NEXT_PUBLIC_NAVER_MAP_CLIENT_ID` 값으로 설정합니다.

### 지도 설정하기

웨딩 장소의 지도는 네이버 지도 API를 사용합니다. `wedding-config.ts` 파일에서 다음과 같이 설정할 수 있습니다:

```typescript
venue: {
  name: "홀 이름",
  address: "주소",
  tel: "전화번호",
  naverMapId: "네이버 지도 검색용 장소명",
  coordinates: {
    latitude: 37.5045,  // 위도
    longitude: 127.0495 // 경도
  },
  placeId: "12136346", // 네이버 지도 장소 ID
  mapZoom: "17.08",    // 지도 줌 레벨
  // ... 기타 설정
  
  // 신랑측 배차 안내 (없으면 표시되지 않음)
  groomShuttle: {
    location: "서울역 2번 출구 앞",
    departureTime: "오전 10시 30분 출발",
    contact: {
      name: "김철수",
      tel: "010-1234-5678"
    }
  },
  
  // 신부측 배차 안내 (없으면 표시되지 않음)
  brideShuttle: {
    location: "강남역 4번 출구 앞",
    departureTime: "오전 10시 30분 출발",
    contact: {
      name: "박영희",
      tel: "010-8765-4321"
    }
  }
}
```

#### 좌표 및 장소 ID 찾는 방법

1. [네이버 지도](https://map.naver.com)에서 결혼식장을 검색합니다.
2. 검색된 장소를 클릭하여 상세 정보를 확인합니다.
3. 브라우저 주소창에서 URL을 확인합니다:
   - `https://map.naver.com/p/search/장소명/place/12345678` 형식의 URL에서 마지막 숫자가 `placeId`입니다.
4. 길찾기 URL에 들어가면 줌 레벨을 포함한 좌표를 확인할 수 있습니다:
   - `https://map.naver.com/p/directions/-/-/-/walk/place/12345678?c=17.08,0,0,0,dh`
   - 위 URL에서 `c=` 다음의 첫 번째 값(`17.08`)이 `mapZoom` 값입니다.
5. 위도와 경도는 네이버 지도 개발자 도구를 열고 콘솔에서 다음 명령어로 확인할 수 있습니다:
   ```javascript
   // 브라우저 콘솔에서 실행
   console.log(map.getCenter().toString());
   // 출력: lat: 37.1234, lng: 127.5678
   ```

### 갤러리 설정하기

갤러리는 두 가지 레이아웃과 두 가지 위치로 표시할 수 있습니다. `wedding-config.ts` 파일에서 다음과 같이 설정할 수 있습니다:

```typescript
gallery: {
  layout: "scroll", // "scroll" 또는 "grid" 선택
  position: "middle", // "middle" 또는 "bottom" 선택
  images: [
    "/images/gallery/image1.jpg",
    "/images/gallery/image2.jpg",
    "/images/gallery/image3.jpg",
    "/images/gallery/image4.jpg",
    "/images/gallery/image5.jpg",
  ],
},
```

#### 갤러리 레이아웃 옵션

- **`"scroll"`**: 수평 스크롤 형태로 갤러리를 표시합니다.
  - 좌우 화살표 버튼으로 탐색 가능
  - 터치 스와이프로 이동 가능
  - 화면 공간을 효율적으로 사용
  
- **`"grid"`**: 3열 그리드 형태로 모든 이미지를 한 번에 표시합니다.
  - 모든 갤러리 이미지를 한 화면에서 확인 가능
  - 반응형 디자인으로 모바일에서도 최적화
  - 이미지 개수가 많을 때 유용

#### 갤러리 위치 옵션

- **`"middle"`**: 기본 위치 (장소 정보 다음)
  - 메인 화면 → 초대의 말씀 → 일정 → 장소 정보 → **갤러리** → 참석 여부 → 마음 전하실 곳
  
- **`"bottom"`**: 맨 하단 위치 (Footer 바로 위)
  - 메인 화면 → 초대의 말씀 → 일정 → 장소 정보 → 참석 여부 → 마음 전하실 곳 → **갤러리**

#### 갤러리 이미지 순서 설정

갤러리 이미지는 `wedding-config.ts` 파일에 설정된 순서대로 표시됩니다. 이미지 순서를 변경하려면 `images` 배열의 순서를 조정하면 됩니다:

```typescript
gallery: {
  layout: "grid",
  position: "middle",
  images: [
    "/images/gallery/favorite1.jpg", // 첫 번째로 표시될 이미지
    "/images/gallery/favorite2.jpg", // 두 번째로 표시될 이미지
    "/images/gallery/other1.jpg",
    "/images/gallery/other2.jpg",
    // ... 나머지 이미지들
  ],
},
```

두 레이아웃 모두 이미지 클릭 시 확대 보기를 지원하며, 확대 보기에서는 키보드 방향키, 마우스 휠, 터치 제스처로 이미지를 탐색할 수 있습니다.

## Slack 웹훅 설정하기

참석 여부 응답을 Slack으로 알림 받기 위한 설정 방법:

1. [Slack API 웹사이트](https://api.slack.com/apps)에 접속하여 로그인합니다.
2. "Create New App"을 클릭하고 "From scratch"를 선택합니다.
3. 앱 이름(예: "Wedding RSVP")과 Slack 워크스페이스를 선택합니다.
4. 왼쪽 메뉴에서 "Incoming Webhooks"를 선택하고 활성화합니다.
5. "Add New Webhook to Workspace"를 클릭합니다.
6. 알림을 받을 채널을 선택하고 "Allow"를 클릭합니다.
7. 생성된 Webhook URL을 `.env.local` 파일의 `NEXT_PUBLIC_SLACK_WEBHOOK_URL` 값으로 설정합니다.
8. `wedding-config.ts` 파일에서 Slack 채널 이름을 설정합니다:
   ```typescript
   slack: {
     webhookUrl: process.env.NEXT_PUBLIC_SLACK_WEBHOOK_URL || "",
     channel: "#wedding-rsvp", // 알림을 받을 채널
   }
   ```

## 이미지 및 폰트 추가하기

### 이미지 추가

웨딩 이미지는 `public/images/` 디렉토리에 추가하세요:
- 메인 이미지: `public/images/main.jpg`
- 갤러리 이미지: `public/images/gallery/image1.jpg`, `public/images/gallery/image2.jpg`, ...

이미지는 최적화를 위해 다음 사항을 고려하세요:
- 메인 이미지: 1080x1920px 권장 (세로형 모바일 최적화)
- 갤러리 이미지: 1200x900px 권장 (4:3 비율)
- 파일 크기 최적화: JPG 또는 WebP 형식, 이미지당 500KB 이하 권장

### 폰트 추가

사용자 정의 폰트는 `public/fonts/` 디렉토리에 추가하세요:
- MaruBuri-ExtraLight.ttf
- MaruBuri-Light.ttf
- MaruBuri-Regular.ttf
- MaruBuri-SemiBold.ttf
- MaruBuri-Bold.ttf
- PlayfairDisplay-Italic.ttf

## 배포하기

이 프로젝트는 Vercel 또는 Netlify에 쉽게 배포할 수 있습니다.

### Vercel 배포

1. [Vercel](https://vercel.com)에 가입하고 GitHub 계정을 연결합니다.
2. 새 프로젝트를 생성하고 이 저장소를 선택합니다.
3. 환경 변수를 설정합니다 (`.env.local` 파일의 내용).
4. 배포를 클릭합니다.
5. (선택 사항) 커스텀 도메인을 설정합니다.

#### Vercel CLI로 배포하기

1. Vercel CLI 설치:
   ```
   npm install -g vercel
   ```

2. 로그인:
   ```
   vercel login
   ```

3. 배포:
   ```
   vercel
   ```
   
4. 프로덕션 환경으로 배포:
   ```
   vercel --prod
   ```

### Netlify 배포

1. [Netlify](https://netlify.com)에 가입하고 GitHub 계정을 연결합니다.
2. 새 사이트를 생성하고 이 저장소를 선택합니다.
3. 빌드 설정: 
   - Build command: `npm run build`
   - Publish directory: `.next`
4. 환경 변수를 설정합니다 (`.env.local` 파일의 내용).
5. (선택 사항) 커스텀 도메인을 설정합니다.

## 보안 및 개인정보 보호

### API 키 및 환경 변수 보호

- `.env.local` 파일은 절대 Git 저장소에 커밋하지 마세요. 이 파일은 기본적으로 `.gitignore`에 포함되어 있습니다.
- 모든 API 키와 비밀 정보는 환경 변수로 저장하세요.
- Vercel이나 Netlify 같은 배포 서비스에서 환경 변수를 안전하게 설정하세요.

### 개인정보 처리 주의사항

- 청첩장에 포함되는 개인정보(전화번호, 계좌번호 등)를 신중하게 관리하세요.
- 신랑, 신부, 부모님 등의 정보를 포함하기 전에 동의를 구하세요.
- 결혼식 참석자 정보를 수집할 경우, 수집 목적을 명확히 설명하고 필요한 기간 동안만 보관하세요.
- 배포 전 모든 테스트 데이터와 더미 정보가 제거되었는지 확인하세요.

### 무단 복제 방지 및 식별 기능

- 복제본이 발견될 경우 법적 조치가 취해질 수 있습니다.

**참고**: 이 프로젝트는 CC BY-NC-ND(저작자표시-비영리-변경금지) 라이선스 하에 공개되어 있으며, 개인적인 결혼식 용도 외에 상업적 목적으로 사용하는 것은 명시적으로 금지되어 있습니다.

### 배포 전 체크리스트

- [ ] 테스트용 API 키가 실제 키로 변경되었는지 확인
- [ ] 모든 개인정보가 적절하게 처리되었는지 확인
- [ ] `.gitignore` 파일이 `.env.local`과 같은 민감한 파일을 제외하는지 확인
- [ ] `wedding-config.ts` 파일에 실제 정보만 포함되어 있는지 확인
- [ ] 테스트 데이터 및 플레이스홀더 텍스트가 실제 콘텐츠로 대체되었는지 확인

## 성능 최적화

이 프로젝트는 다음과 같은 성능 최적화를 적용했습니다:

1. **코드 분할**: 동적 임포트를 사용하여 각 섹션을 필요할 때 로드합니다.
2. **이미지 최적화**: next/image를 사용하여 이미지를 최적화하였습니다.
3. **지연 로딩**: 갤러리 이미지에 지연 로딩을 적용했습니다.
4. **스크립트 최적화**: 네이버 지도 API는 필요할 때만 동적으로 로드됩니다.
5. **모바일 터치 최적화**: 버튼에 리플 효과 및 터치 피드백 적용으로 UX 향상
6. **동적 색상 시스템**: 빌드 타임에 색상을 계산하여 런타임 성능 향상
7. **조건부 렌더링**: RSVP 등 불필요한 섹션은 완전히 제외하여 번들 크기 최적화

### 공유 기능

청첩장을 쉽게 공유할 수 있는 두 가지 방법을 제공합니다:
1. URL 복사하기: 클립보드에 현재 URL을 복사
2. 공유하기: Web Share API를 사용하여 기기의 공유 메뉴 호출

## 문제 해결

### 네이버 지도가 표시되지 않는 경우

- `.env.local` 파일에 올바른 클라이언트 ID가 설정되어 있는지 확인하세요.
- 네이버 클라우드 플랫폼 콘솔에서 웹 서비스 URL이 정확히 등록되었는지 확인하세요.
- 브라우저 콘솔에 오류 메시지를 확인하세요.
- 인증 오류 메시지가 나타나면 다음을 확인하세요:
  - 클라이언트 ID가 정확한지
  - 웹 서비스 URL에 현재 접속 중인 도메인이 등록되어 있는지
  - 로컬 개발 시 http://localhost:3000이 등록되어 있는지
  - 배포 시 실제 도메인이 등록되어 있는지

### 참석 여부 알림이 Slack으로 전송되지 않는 경우

- `.env.local` 파일에 올바른 Slack Webhook URL이 설정되어 있는지 확인하세요.
- Webhook URL의 형식을 확인하세요 (https://hooks.slack.com/services/... 형식).
- Slack 앱이 워크스페이스에 올바르게 설치되어 있는지 확인하세요.
- 알림을 보낼 채널이 존재하는지, 그리고 앱이 채널에 접근 권한이 있는지 확인하세요.

### 공유 기능이 작동하지 않는 경우

- 모바일 기기에서는 Web Share API가 대부분 지원되지만, 데스크톱 브라우저에서는 지원이 제한적일 수 있습니다.
- 브라우저 지원 여부는 [Can I use Web Share API](https://caniuse.com/web-share) 에서 확인할 수 있습니다.
- 지원되지 않는 브라우저에서는 URL 복사 기능만 작동합니다.

## 기여하기

이 프로젝트에 기여하는 것을 환영합니다! 다음은 기여 방법에 대한 가이드라인입니다.

### 기여 방법

1. 이 저장소를 포크하세요.
2. 새로운 브랜치를 생성하세요 (`git checkout -b feature/amazing-feature`).
3. 변경 사항을 커밋하세요 (`git commit -m 'Add some amazing feature'`).
4. 브랜치를 푸시하세요 (`git push origin feature/amazing-feature`).
5. Pull Request를 생성하세요.

### Pull Request 가이드라인

Pull Request를 제출할 때는 다음 정보를 포함해주세요:

```markdown
## 변경 사항 설명
변경된 내용과 그 이유를 간략히 설명해주세요.

## 관련 이슈
해당 PR이 해결하는 이슈 번호를 적어주세요 (예: #123).

## 체크리스트
- [ ] 코드가 프로젝트 스타일 가이드를 따르는지 확인했습니다.
- [ ] 기존 코드의 동작을 변경하는 경우, 주석으로 이유를 설명했습니다.
- [ ] 필요한 경우 문서를 업데이트했습니다.
- [ ] 코드가 잘 작동하는지 테스트했습니다.
```

### 코드 스타일

- TypeScript 코드는 프로젝트의 eslint 및 prettier 설정을 따라주세요.
- 컴포넌트는 함수형 컴포넌트로 작성해주세요.
- styled-components를 사용하여 스타일을 작성해주세요.
- 가독성을 위해 적절한 주석과 함께 명확한 변수명을 사용해주세요.

## 라이선스

이 프로젝트는 CC BY-NC-ND(저작자표시-비영리-변경금지) 라이선스 하에 공개되어 있습니다.

### 라이선스 안내

이 저작물은 [크리에이티브 커먼즈 저작자표시-비영리-변경금지 4.0 국제 라이선스](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.ko)에 따라 이용할 수 있습니다.

다음과 같은 제한이 적용됩니다:
- **저작자표시(BY)**: 원저작자를 밝혀야 합니다.
- **비영리(NC)**: 이 저작물은 상업적 목적으로 이용할 수 없습니다. 이 프로젝트를 활용하여 유료 서비스를 제공하거나 상업적 이익을 취할 수 없습니다.
- **변경금지(ND)**: 이 저작물을 개작, 수정하여 다른 저작물을 만들 수 없습니다. 이 프로젝트의 코드를 수정하여 다른 프로젝트를 만들거나 배포할 수 없습니다.

또한 개인적인 목적으로 본인의 웨딩을 위한 경우를 제외하고는 이 프로젝트를 비공개 프로젝트로 복제하여 사용하는 것도 금지됩니다.

## 기타 정보

- 폰트: [마루부리](https://hangeul.naver.com/font) 폰트는 Naver에서 제공하는 무료 폰트입니다. [PlayfairDisplay](https://fonts.google.com/specimen/Playfair+Display) 폰트는 오픈 폰트 라이센스를 따릅니다.
- 이미지: 예시 이미지는 Unsplash에서 가져왔거나 직접 촬영한 사진들로 이루어져 있습니다. 테스트용으로만 사용하세요. 실제 사용 시 저작권에 유의하세요.

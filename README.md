# 10% ジュッパーセント

여행객의 소중한 10%를 위한 서포터 웹앱 (2026.11.1 리펀드 방식 대응).

## 폴더 구성

```
juppasento/
├─ public/index.html   # 앱 화면 (HTML/CSS/JS 한 파일)
├─ api/scan.js         # 영수증 사진 인식 (Claude API 호출하는 서버 함수)
├─ vercel.json         # Vercel 설정
└─ package.json
```

## 배포 방법

### 방법 A. GitHub 연결 (추천)
1. 이 폴더를 GitHub 저장소에 올립니다.
2. vercel.com → **Add New… → Project** → 저장소 선택 → **Deploy**
   (Framework Preset은 `Other` 그대로, Build Command는 비워 둡니다.)
3. 배포 후 **Settings → Environment Variables**에 아래 값을 추가하고 **Redeploy** 합니다.

| 이름 | 값 |
|---|---|
| `ANTHROPIC_API_KEY` | console.anthropic.com에서 발급한 API 키 |
| `ANTHROPIC_MODEL` (선택) | 기본값 `claude-sonnet-5-5` |

### 방법 B. CLI
```bash
npm i -g vercel
cd juppasento
vercel            # 첫 배포 (미리보기)
vercel env add ANTHROPIC_API_KEY
vercel --prod     # 실서비스 배포
```

## Claude 아티팩트 버전과 다른 점
- **저장 위치**: 영수증·체크 상태는 사용자의 브라우저(localStorage)에 저장돼요. 기기 간 동기화는 되지 않아요.
- **사진 인식**: `/api/scan` 서버 함수가 내 API 키로 Claude를 호출해요. 사용량만큼 API 비용이 나가요.
- **첫 방문 팝업**: 브라우저당 한 번만 떠요. 같은 브라우저에서 새로고침해도 다시 뜨지 않아요.

## 운영 전 체크
- `/api/scan`은 누구나 호출할 수 있어서 API 비용이 새어 나갈 수 있어요. 공개 전에 Vercel의 Firewall/Rate limiting을 켜 두는 걸 권해요.
- 면세 제도 안내 문구는 2026년 10월 기준이에요. 일본 관광청·세관 공지가 바뀌면 `public/index.html`의 가이드 탭을 함께 수정하세요.

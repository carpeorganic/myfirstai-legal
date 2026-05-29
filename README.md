# MyFirstAI — 법적 문서 (Privacy & Terms)

App Store / Google Play 등록에 필요한 **개인정보처리방침**과 **이용약관** 호스팅용 정적 사이트입니다. GitHub Pages로 무료 호스팅합니다.

## 구성

| 파일 | 용도 |
|---|---|
| `index.html` | 두 문서 링크 모음 (랜딩) |
| `privacy.html` | 개인정보처리방침 |
| `terms.html` | 이용약관 |
| `style.css` | 공통 스타일 |

## 플레이스홀더 (배포 전 반드시 채워야 함)

전부 `{{...}}` 형태로 표시되어 있고, HTML에서 노란 배경의 `<span class="placeholder">`로 감싸져 있습니다. 모든 `.html` 파일에서 다음 항목을 일괄 치환하세요.

| 플레이스홀더 | 채울 내용 |
|---|---|
| `{{상호}}` | 사업자등록증상의 상호 |
| `{{대표자명}}` | 대표자 이름 |
| `{{사업자등록번호}}` | NNN-NN-NNNNN 형식 |
| `{{주소}}` | 사업장 주소 |
| `{{AI제공자명}}` | 예: Anthropic, OpenAI 등 실제로 사용하는 AI 모델 제공자 |
| `{{시행일}}` | YYYY-MM-DD (스토어 등록 직전 날짜 권장) |

이메일은 `carpeorganic@gmail.com`으로 박혀 있습니다. 다른 메일을 쓰려면 `.html` 3개에서 일괄 치환.

### PowerShell 일괄 치환 예시

```powershell
Set-Location C:\ai\myfirstai-legal
$map = @{
    '{{상호}}'           = '카르페오가닉'
    '{{대표자명}}'       = '홍길동'
    '{{사업자등록번호}}' = '123-45-67890'
    '{{주소}}'           = '서울특별시 ...'
    '{{AI제공자명}}'     = 'Anthropic'
    '{{시행일}}'         = '2026-06-15'
}
Get-ChildItem *.html | ForEach-Object {
    $t = Get-Content $_ -Raw
    foreach ($k in $map.Keys) { $t = $t.Replace($k, $map[$k]) }
    Set-Content $_ -Value $t -Encoding UTF8
}
```

## 배포 (GitHub Pages)

### 옵션 A — gh CLI 로그인된 경우

```powershell
Set-Location C:\ai\myfirstai-legal
git init -b main
git add .
git commit -m "Initial legal docs"
gh repo create myfirstai-legal --public --source=. --push
gh api -X POST repos/:owner/myfirstai-legal/pages -f source[branch]=main -f source[path]=/
```

배포 URL:
- `https://<GitHub사용자명>.github.io/myfirstai-legal/privacy.html`
- `https://<GitHub사용자명>.github.io/myfirstai-legal/terms.html`

### 옵션 B — 웹 UI

1. <https://github.com/new> 에서 새 리포지토리 생성. 이름: `myfirstai-legal`, Public.
2. 로컬에서 푸시:
   ```powershell
   Set-Location C:\ai\myfirstai-legal
   git init -b main
   git add .
   git commit -m "Initial legal docs"
   git remote add origin https://github.com/<GitHub사용자명>/myfirstai-legal.git
   git push -u origin main
   ```
3. 리포지토리 페이지 → Settings → Pages → Source: `Deploy from a branch` → Branch: `main`, Folder: `/ (root)` → Save.
4. 1~2분 후 배포 완료. 위 URL로 접근 가능.

### 옵션 C — 커스텀 도메인 (선택)

자체 도메인을 쓰려면:
1. 루트에 `CNAME` 파일 생성 → 도메인 한 줄 기입 (예: `legal.myfirstai.app`).
2. DNS에 `CNAME` 레코드 추가 → `<GitHub사용자명>.github.io`.
3. Settings → Pages → Custom domain 입력 → Enforce HTTPS 체크.

## 스토어 등록 시 입력

- **Apple App Store Connect** → App Privacy → Privacy Policy URL: `https://<사용자명>.github.io/myfirstai-legal/privacy.html`
- **Google Play Console** → Policy → App content → Privacy Policy URL: 동일
- 이용약관 URL은 결제·구독 상품 등록 시, 그리고 앱 내 “이용약관” 화면에서 링크.

## 갱신

법령 변경·서비스 변경 시 `.html` 파일 수정 → `git commit && git push`. 1~2분 후 자동 반영.

`시행일` / `최종 개정일` 갱신 잊지 말기.

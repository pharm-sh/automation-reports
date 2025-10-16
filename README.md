# Automation Test Reports

GitHub Pages 기반 테스트 리포트 호스팅

## 📁 구조

```
automation-reports/
├── index.html              # 메인 뷰어 페이지
├── data/
│   └── reports.json        # 리포트 메타데이터
├── reports/
│   ├── api/                # API 테스트 리포트 (Newman)
│   │   ├── newman-TIMESTAMP.html
│   │   └── ...
│   └── ui/                 # UI 테스트 리포트 (Pytest)
│       ├── pytest-TIMESTAMP.html
│       └── ...
└── README.md
```

## 🚀 설정 방법

### 1단계: 레포지토리 생성

```bash
# 퍼블릭 레포지토리 생성
gh repo create automation-reports --public

# 클론
git clone https://github.com/USERNAME/automation-reports.git
cd automation-reports
```

### 2단계: 파일 업로드

```bash
# 이 폴더의 파일들을 복사
cp -r .github/pages-setup/* automation-reports/

# 폴더 생성
cd automation-reports
mkdir -p reports/api reports/ui

# 커밋 & 푸시
git add .
git commit -m "Initial setup"
git push origin main
```

### 3단계: GitHub Pages 활성화

```
1. GitHub 레포지토리 → Settings
2. 좌측 "Pages" 클릭
3. Source: Deploy from a branch
4. Branch: main / (root)
5. Save
```

## 📊 리포트 업로드 방식

### GitHub Actions에서 자동 업로드

```yaml
- name: Upload Report to GitHub Pages
  run: |
    # automation-reports 레포 클론
    git clone https://github.com/USERNAME/automation-reports.git

    # 리포트 복사
    cp newman-$TIMESTAMP.html automation-reports/reports/api/

    # reports.json 업데이트
    # (스크립트로 메타데이터 추가)

    # 푸시
    cd automation-reports
    git config user.name "GitHub Actions"
    git config user.email "actions@github.com"
    git add .
    git commit -m "Add report $TIMESTAMP"
    git push
```

## 🔗 URL

```
https://USERNAME.github.io/automation-reports/
```

## 📝 reports.json 형식

```json
{
  "api": [
    {
      "id": "20251016-103045",
      "timestamp": "2025-10-16 10:30:45",
      "status": "success",
      "passed": 15,
      "failed": 0,
      "total": 15,
      "duration": "3m 42s",
      "environment": "Staging",
      "trigger": "Push",
      "url": "reports/api/newman-20251016-103045.html"
    }
  ],
  "ui": [...]
}
```

## 🎨 기능

- **탭 전환**: API / UI 테스트 구분
- **LNB**: 왼쪽 리포트 목록 (날짜 기반)
- **상태 표시**: 🟢 성공 / 🔴 실패
- **날짜 포맷**: 25/10/16 - 10:30
- **자동 로드**: 최신 리포트 자동 표시
- **Load More**: 10개씩 더 보기

## 🔧 유지보수

### 오래된 리포트 삭제

```bash
# 30일 이상된 리포트 삭제
find reports/api -name "*.html" -mtime +30 -delete
find reports/ui -name "*.html" -mtime +30 -delete

# reports.json도 함께 정리
```

### reports.json 재생성

```bash
# 스크립트로 자동 생성
python scripts/generate-reports-json.py
```

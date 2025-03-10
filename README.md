# GitHub Pages 데모 프로젝트

이 프로젝트는 GitHub Pages를 사용하여 정적 웹사이트를 배포하는 방법을 보여줍니다.

## 배포 방법

이 프로젝트를 GitHub Pages로 배포하려면 다음 단계를 따르세요:

1. GitHub 저장소에 코드를 푸시합니다.
2. GitHub 저장소 설정으로 이동합니다.
3. 왼쪽 사이드바에서 "Pages"를 클릭합니다.
4. "Source" 섹션에서 "Deploy from a branch"를 선택합니다.
5. "Branch" 드롭다운에서 "main"을 선택하고 "/(root)" 폴더를 선택합니다.
6. "Save" 버튼을 클릭합니다.

몇 분 후에 사이트가 `https://[사용자명].github.io/[저장소명]/`에서 배포됩니다.

## 수동으로 GitHub Actions 워크플로우 설정하기

더 고급 배포 옵션을 위해 GitHub Actions 워크플로우를 설정할 수 있습니다:

1. 저장소에 `.github/workflows` 디렉토리를 생성합니다.
2. 해당 디렉토리에 `deploy.yml` 파일을 생성하고 다음 내용을 추가합니다:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: 'pages'
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: '.'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

## 로컬에서 테스트하기

로컬에서 사이트를 테스트하려면 간단한 HTTP 서버를 사용할 수 있습니다:

```
python -m http.server
```

그런 다음 브라우저에서 `http://localhost:8000`으로 접속하세요.

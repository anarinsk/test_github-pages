## Quarto Project 생성 

[Quarto](https://quarto.org/)를 이용하여 프로젝트를 생성하고 문서를 작성한다.

- `_quarto.yml`에 프로젝트 렌더링에 관한 정보가 들어 있다. 
- `.nojekyll` 파일은 Github Pages가 Jekyll을 사용하지 않도록 하는 파일이다. 반드시 생성하자. 
- .gitignore에 아래 폴더를 넣어 두자. 

```
/.quarto/
/_site/
/.pixi/
```
## Github Pages 생성

기본은 아래를 참고하자. 

<https://quarto.org/docs/publishing/github-pages.html>

### 기본 원리 

기본적으로 Quarto로 html 페이지를 모두 생성해서 해당 페이지를 github에 올리고 깃허브의 웹 서버를 통해서 페이지를 제공하는 것이다. 

#### Actions 없이 쓰기 

<https://quarto.org/docs/publishing/github-pages.html#render-to-docs>

Actions를 쓰지 않는다면 위에 소개된 방법 중에서 main 브랜치 root 혹은 `docs` 폴더를 활용하는 방법이 좋다. Github Pages는 `docs` 폴더를 자동으로 인식하고, 푸시가 발생하면 퍼블리싱을 위한 절차를 개시한다. 

`.github/workflows` 내에 스크립트가 없었도 "pages build and deployment" 액션스가 자동으로 실행된다. 

<img style="border:1px solid black;" src="/images/main.png" width=400>

#### Actions로 쓰기 

<https://quarto.org/docs/publishing/github-pages.html#github-action>

- `_quarto.yml` 파일에서 별도의 아웃풋 폴더를 지정할 필요는 없다. 
- 위의 설명에 나와 있듯이 로컬에서 반드시 아래 명령을 실행해야 한다. 

```shell
$ quarto publish gh-pages
```

로컬에 gh-pages가 생성되야 원격에도 생성된다. 이후부터 main에 내용을 반영한 렌더링과 퍼블리싱은 깃허브 액션스가 자동으로 처리한다. 

이 리포의 `.github/workflows/publish.yml` 파일을 참고하자.

```yml
on:
  workflow_dispatch:
  push:
    branches: main
  
name: Quarto Publish
  
jobs:
  build-deploy:
    runs-on: ubuntu-latest # 우분투 셋업 
    permissions:
      contents: write
      id-token: write
      pages: write
    steps:
      - name: Check out repository 
        uses: actions/checkout@v4
  
      - name: Set up Quarto # 쿼토 설치 
        uses: quarto-dev/quarto-actions/setup@v2
  
      - name: Render and Publish # gh-pages 페이지를 렌더해서 발행한다. 
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> [!NOTE] 
> - `on:` 액션스 실행 조건을 나타낸다. 
> - `build-deploy:` 실행 OS와 권한을 설정한다. 
> - `steps:` 실행 단계를 나타낸다.
> - `uses:` 사용할 액션스를 지정한다.
> - `with:` 액션스에 전달할 인자를 지정한다.
> - `env:` 환경 변수를 지정한다.

> [!NOTE]
> - `actions/checkout@v4`: 리포의 브랜치에 접근할 수 있게 해준다. 
> - `quarto-dev/quarto-actions/setup@v2`: 쿼토를 설치한다.
> - `quarto-dev/quarto-actions/publish@v2`: 쿼토를 이용해 페이지 렌더링 및 발행 작업을 수행한다. 

Pages의 설정은 아래와 같이 둔다. 

![](/images/actions.png)

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
## Github Pages 설정 

기본은 여기를 참고하자. 


`_quarto.yml` 파일에서 
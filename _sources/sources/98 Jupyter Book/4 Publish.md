# 배포
## Guide
### 환경 설정
Github에 public repository를 만든다.

원격 repository 이름을 `origin`으로 설정한다.

### 라이브러리 설치
손쉬운 배포를 위해 ghp-import library를 설치한다.
```
pip install ghp-import
```

### 라이브러리 사용
ghp-import library를 사용한다.
```
ghp-import -n -p -f (빌드된 html 파일이 있는 폴더 경로)
```

예를들면 다음과 같다
```
ghp-import -n -p -f _build/html
```

그러면 자동으로 gh-pages branch가 생기고 빌드된 파일들이 push된다.

### Repository 설정
github repository로 들어간 다음 다음 설정 페이지를 연다.
```
Settings >> code and automation >> Pages
```

다음과 같이 설정한다.
```
source : deploy form a branch
branch : gh-pages /(root)
```

### 홈페이지 URL
배포되는 주소는 다음과 같은 규칙을 따른다.
```
https://(아이디)/github.io/(Repository이름)
```

예를 들면 다음과 같다.
```
https://rla523at.github.io/Study
```

### 향후 배포
한번 배포한 후에는 변경사항이 있을 때마다 라이브러리를 사용해서 재배포 해주기만 하면 된다.

즉, 라이브러리 사용 section에 있는 명령어만 다시 입력해주면 된다.

> Reference  
> [jupyterbook](https://jupyterbook.org/en/stable/start/publish.html)
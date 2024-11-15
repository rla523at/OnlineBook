# Jupyter Book

## 환경셋팅

### Jupyterbook 설치
손쉬운 가상 환경을 위해 [Anaconda](https://www.anaconda.com/download/#windows)를 사용한다.

Anaconda prompt에서 Jupyterbook 가상환경을 만든다
```
conda create -n jb python=3.9
```

가상환경에 들어간다.

```
conda activate jb
```

conda를 업데이트 한다.

```
conda update --all
```

Jupyterbook을 설치한다

```
pip install -U jupyter-book
```

### cmd로 ananconda 사용하기
anaconda 를 cmd 에서 사용하려면 환경변수 설정을 해야한다.
* anaconda prompt에서 `where conda`와 `where python`로 conda.exe와 python.exe의 경로를 찾는다
* 시작 >> 시스템 환경 변수 편집 >> 환경변수 >> 시스템 변수 >> Path >> 편집
* conda.exe 가 있는 경로를 추가한다.

cmd창에서 `conda --version`을 입력하여 정상동작하면 환경변수가 제대로 설정된 것이다.

anaconda prompt에서 `conda init cmd.exe` 명령어를 실행해주면 cmd를 ananconda prompt처럼 사용할 수 있다.



## 기본 문서 작성 문법
[jupyterbook - cheatsheet](https://jupyterbook.org/en/stable/reference/cheatsheet.html) 참고  

### 주의사항
* .md 파일의 제목에 한글이 들어가면 안된다

* 하나의 .md 파일에는 하나의 H1 header(#)만 사용한다. 
  * H1 header(#)가 여러번 나올 경우 TOC가 제대로 동작하지 않는다.

*  header는 순차적으로 내려가야 한다.

* Tex를 사용할때, 위에 줄과 붙어있으면 inline으로 인식되서 제대로 나오지 않기 때문에 한 줄 띄어줘야 한다.
```
(Wrong)
This is equation.
$$ y = x + 1 $$

(correct)
This is equation.

$$ y = x + 1 $$
```

* 문서에서 인용이 되야지만 References 페이지에 나타난다.

## Custom CSS
[jupyterbook - custom-css-or-javascript](https://jupyterbook.org/en/stable/advanced/html.html#custom-css-or-javascript) 참고

## TeX 매크로
_config.yml 파일에 아래 예시와 같이 매크로를 추가한다.

```
sphinx:
  config:
    bibtex_reference_style: author_year
    mathjax_path: https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js
    mathjax3_config:
       tex:
        macros:
          "st" : "\\quad s.t \\quad"
          "pdiff": ["\\frac{\\partial #1}{\\partial #2}", 2]
          ...
```

> Reference  
> [jupyterbook - sphinx-tex-macros](https://jupyterbook.org/en/stable/advanced/sphinx.html#sphinx-tex-macros)  
> [docs.mathjax - macros](https://docs.mathjax.org/en/latest/input/tex/macros.html)  


## Citation
`References/_reference.bib` 파일에 아래 형식으로 내용을 입력한다.

```
@book{LeeTM,
  title     = "Introduction to Topological Manifolds",
  author    = "Lee, John M",
  publisher = "Springer",
  series    = "Graduate texts in mathematics",
  edition   =  2,
  month     =  dec,
  year      =  2010,
  address   = "New York, NY",
  language  = "en"
}
```

그러면 LeeTM이 name이 되며 name으로 문서에서 인용이 가능하다.

{cite}`LeeTM`

```
{cite}`LeeTM`
```

> Reference   
> [jupyterbook - create-a-bibtex-file](https://jupyterbook.org/en/stable/tutorials/references.html#create-a-bibtex-file)  

### 참고1
`_reference.bib` 파일에 Bibtex를 입력할 떄, name, author, title, year, publisher은 필수로 작성해야 된다.

```
@misc{ Nobody06,
       author = "Nobody Jr",
       title = "My Article",
       year = "2006",
       publisher = "My Pusblisher"}
```

### 참고2
[bibtex - author](https://bibtex.eu/fields/author/)을 참고해서 저자명(`author = `)을 작성하면 된다.


## Image
_image폴더를 만들고 그안에 image 파일을 추가한다.

아래와 같이 문서에 입력해서 image 파일을 사용한다.

````
```{figure} _image/Stress-strain_curve.svg
```
````

> Reference  
> [jupyterbook - figures](https://jupyterbook.org/en/stable/content/figures.html)  

## TOC 구성
web page의 TOC 구성은 `_toc.yml`파일에 의해 결정된다.

현재 `_toc/TOC_Writer.py`를 이용해 `_toc.yml`파일을 작성한다.

`_toc/TOC_Writer.py`은 아래 규칙에 맞춰 작성되었다고 가정한다.

### 규칙1
TOC에 나타나는 순서를 정해주기 위해서 폴더나 md파일 이름을 `## [name]` 형태로 작성한다.
예를들면 `01 math`, `01 Power Set.md`와 같다.

**주의사항1**

순서가 상관없을 경우, 파일 이름은 저 형태를 지키지 않아도 된다.

하지만 폴더명은 항상 `## [name]` 형태로 작성한다.

또한 반드시 띄어쓰기가 있어야 한다. 

```
01math 	(X) 
01 math (O)
```

**주의사항2**

TOC에 표시되는 이름은 md파일의 파일명으로 생성되는것이 아니다.

TOC는 md파일 안에 있는 H1 header(#)의 이름으로 생성이 된다.

```
document1.md
# document123.md
```
그러면 TOC에는 document123으로 표시된다.

### 규칙2

section 폴더 안에는 section 이름과 동일한 .md 파일이 반드시 존재해야된다.

`section.md` 파일은 TOC에서 section을 눌렀을 때 나타날 페이지이다.

**주의사항1**

section의 이름은 section.md 파일 안에 있는 H1 header의 이름으로 생성된다.

### 규칙3

파일이 하나만 있는 경우 section 폴더로 만들지 않는다.

아래와 같이 section 안에 section.md만 있는 경우를 생각해보자.

```
(bad example)
├─section
│  │  section.md
│
│  document1.md
│  document2.md
```


이 경우에는 다음과 같이 구성 해야 한다.

```
(good example)
│  section.md
│  document1.md
│  document2.md
```

## 빌드
anaconda prompt를 실행한다.

jupyter 가상환경을 실행한다.

```
conda activate jb
```

make.py가 있는 폴더로 이동한다.

```
cd (path)
```

명령어를 실행한다.

```
# Build 
python -m make build

# Clean
python -m make Clean

# Rebuild
python -m make Rebuild
```

`_build/build/index.html` 파일을 열어서 확인한다.

> Reference  
> [jupyterbook - overview](https://jupyterbook.org/en/stable/start/overview.html)   
> [jupyterbook - build](https://jupyterbook.org/en/stable/start/build.html)  

### ETC

* Jupyter book install by conda

Jupyter book을 설취하기 위해 conda를 사용하면 왜 인지 안된다
```
conda install -c conda-forge jupyter-book
```

* Jupyter book install by mamba

mamba를 사용하여 설치하면 된다.
```
conda install -c conda-forge mamba
mamba install -c conda-forge jupyter-book
```

참고로 경로에 한글이 있는 경우 mamba install이 안될 수 있다. 예를 들어 `C/User/(계정명)`에서 계정명이 한글이면 install이 안된다.

### Anaconda 명령어
가상환경 리스트 확인
conda info --envs

가상환경 추가
conda create -n 가상환경 이름 python = 파이썬 버전

가상환경 삭제
conda remove --name [가상환경명] --all

가상환경 실행
conda activate [가상환경 이름]

### .bat 파일에서 anaconda 사용하기
anaconda prompt에서 conda로 시작하는 명령어들은 사실 전부 `call conda.bat`과 같다. 

따라서 .bat 파일로 anaconda prompt의 명령어들을 사용하려면 conda로 시작하는 명령어들 앞에 전부 call conda.bat을 붙여줘야 한다.

```
call conda.bat activate
call conda.bat info --envs
call conda.bat deactivate
```

만약, `call conda.bat`이 정상동작하지 않는다면 conda.bat 파일이 있는 경로가 환경변수에 등록되어 있는지 확인해야한다.

> Reference  
> [blog - [Python] 윈도10 환경 아나콘다3에서 conda.bat 를 이용하여 파이썬 가상환경 구축하기](https://resumetmachine.tistory.com/entry/%EC%9C%88%EB%8F%8410-%ED%99%98%EA%B2%BD-%EC%95%84%EB%82%98%EC%BD%98%EB%8B%A43%EC%97%90%EC%84%9C-condabat-%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)  
> [stackoverflow - How to make batch files run in anaconda prompt](https://stackoverflow.com/questions/46305569/how-to-make-batch-files-run-in-anaconda-prompt)  
> [stackoverflow - Why does my batch script stop after activating a new conda env?](https://stackoverflow.com/questions/42693320/why-does-my-batch-script-stop-after-activating-a-new-conda-env)  


## 배포
### 수동 배포 Guide

Repository 생성 및 설정

_build/html 폴더 안에 들어있는 파일들을 전부 OnlineBook Repository에 복사한다.


### 라이브러리 배포 Guide

Github에 public repository를 만든다.

원격 repository 이름을 `origin`으로 설정한다.

손쉬운 배포를 위해 ghp-import library를 설치한다.

```
pip install ghp-import
```

ghp-import library를 사용한다.

```
ghp-import -n -p -f (빌드된 html 파일이 있는 폴더 경로)
```

예를들면 다음과 같다

```
ghp-import -n -p -f _build/html
```

그러면 자동으로 gh-pages branch가 생기고 빌드된 파일들이 push된다.

github repository로 들어간 다음 다음 설정 페이지를 연다.

```
Settings >> code and automation >> Pages
```

다음과 같이 설정한다.

```
source : deploy form a branch
branch : gh-pages /(root)
```

배포되는 주소는 다음과 같은 규칙을 따른다.

```
https://(아이디)/github.io/(Repository이름)
```

예를 들면 다음과 같다.

```
https://rla523at.github.io/Study
```

한번 배포한 후에는 변경사항이 있을 때마다 라이브러리를 사용해서 재배포 해주기만 하면 된다. 

즉, 라이브러리 사용 section에 있는 명령어만 다시 입력해주면 된다.

> Reference  
> [jupyterbook - publish](https://jupyterbook.org/en/stable/start/publish.html)

### 렌더링이 제대로 안되는 경우
OnlineBook Repository에 .nojekyll 파일을 만들면 해결된다.

> Reference   
> [981004.tistory - 작성한 문서 github page로 공유하기 (무료!) Sphinx(3)](https://981004.tistory.com/5)   
> [velog.io/@drypot - GitHub-Pages-No-Jekyll](https://velog.io/@drypot/GitHub-Pages-No-Jekyll)  
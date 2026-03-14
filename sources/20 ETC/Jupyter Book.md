# Jupyter Book

## 환경셋팅

### Jupyterbook 설치
손쉬운 가상 환경을 위해 [Anaconda](https://www.anaconda.com/download/#windows)를 사용한다.


```
# python 이 설치된 Jupyterbook 가상환경 jb 을 만든다
conda create -n jb python

conda activate jb

# Jupyterbook을 설치한다
pip install jupyter-book
```

### 업그레이드
```
conda activate jb

# conda package 업데이트
conda update --name base conda

# 현재 환경 python 업데이트
conda update python

# 현재 환경 Jupyterbook 업데이트
pip install jupyter-book --upgrade
```

## 기본 문서 작성시 주의사항
하나의 .md 파일에는 하나의 H1 header(#)만 사용한다.

Tex를 사용할때, 위에 줄과 붙어있으면 inline으로 인식되서 제대로 나오지 않기 때문에 한 줄 띄어줘야 한다.
```
(Wrong)
This is equation.
$$ y = x + 1 $$

(correct)
This is equation.

$$ y = x + 1 $$
```

문서에서 인용이 되야지만 References 페이지에 나타난다.

## Custom CSS
myst.yml 의 site: options: 항목 아래에 style: 을 추가해줘야 한다.
```
site:
  options:
    style: ./_static/custom.css
```

> Reference
> https://mystmd.org/guide/website-style


## Math
Math 는 KaTex 기반이다
* https://mystmd.org/guide/math#fn-katex

### 매크로 추가
myst.yml 파일에 project: 하위에 math: 항목에 아래 예시와 같이 매크로를 추가한다.

```
project:
  ...
  math:
    "\\Diff": "\\frac{D #1}{D #2}"
    "\\ninf": "n \\rightarrow \\infty"
    "\\rank": "\\text{rank}"
    ...
```

> Reference  
> https://mystmd.org/guide/math#math-macros 


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
> https://mystmd.org/guide/figures

## TOC 구성
web page의 TOC 구성은 `toc.yml`파일에 의해 결정된다.

### Children Section
TOC 에서 하위의 항목이 있는 경우를 Children Section 이라고 정의한다.

그리고 Children Secction 을 만들기 위해서는 폴더를 만들면 된다.

#### 이름 규칙
TOC에 나타나는 순서를 정해주기 위해서 "## [children_section_name]" 형태로 작성하고 "[children_section_name]" 은 반드시 영어 + 숫자만 사용한다.

#### childeren File
Childeren 파일은 파일명이 "[children_section_name].md" 이고 H1 이 "children_section_name" 인 파일이라고 정의한다.

TOC 에서 Children Section 을 눌렀을 때, 렌더링 되는 페이지로 하고 싶다면 Children File을 생성해야 한다.

### 파일 규칙
파일명은 반드시 영어 + 숫자만 사용한다.

참고로 TOC 에 표시되는 이름은 H1 header(#)의 이름으로 생성이 되며 이 경우에는 한글로 작성하여도 된다.

* EX
  ```
  document1.md
  # 문서1.md
  ```
  그러면 TOC에는 "문서1"로 표시된다.

TOC 구조
* https://jupyterbook.org/stable/authoring/table-of-contents/
  

## 빌드
```
conda activate jb

python jb_manager.py --test
```


## 배포
```
conda activate jb

python jb_manager.py --deploy
```

> Reference
> https://jupyterbook.org/stable/get-started/publish/#github-
> https://mystmd.org/guide/deployment-github-pages
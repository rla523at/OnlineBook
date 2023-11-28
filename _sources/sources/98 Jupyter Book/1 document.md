# 문서작성법

## 기본문법
기본 마크다운 문법은 [MyST markdown cheatsheet](https://jupyterbook.org/en/stable/reference/cheatsheet.html)을 참고해서 작성한다.

### CSS
Custom css는 [Jupyterbook](https://jupyterbook.org/en/stable/advanced/html.html?highlight=css#custom-css-or-javascript)을 참고한다.

## 기본규칙

### 규칙1
.md 파일의 제목에 한글이 들어가면 안된다

### 규칙2
.md 파일 시작시 아래와 같이 H1 header(#)로 시작한다.

```
source.md

# source
...
```

### 규칙3
하나의 .md 파일에는 하나의 H1 header(#)만 사용한다.

아래의 예시처럼 .md 파일안에 H1 header(#)가 여러번 나올 경우 TOC가 제대로 동작하지 않는다.

```
source.md

# source

...

# another header
```

그 이하의 header들은 아래와 같이 여러번 나와도 상관 없다. 

```
source.md

# source

...

## small header1

### small header2

## small header 3

```

### 규칙4
header는 순차적으로 내려가야 한다.

만약 header의 순서가 아래의 예시처럼 되어 있다고 해보자.
```
# H1
### H3
##### H5
...
```

그러면 html파일에는 다음 순서로 나온다
```
# H1
## H3
### H5
...
```

단, 올라오는것은 순차적으로 올라올 필요는 없다
```
# H1
## H2
### H3
#### H4
## H2
```


## Equation
아래와 같이 $\LaTeX$ 표현을 사용한다. 
```
$$ \boldsymbol{\sigma} = \mathbf{C} \boldsymbol{\varepsilon} $$
```
$$ \boldsymbol{\sigma} = \mathbf{C} \boldsymbol{\varepsilon} $$


다음과 같이 끝에 name을 정할 수있다. 
```md
$$ \boldsymbol{\sigma} = \mathbf{C} \boldsymbol{\varepsilon} $$ (hooks-law)
```

$$ \boldsymbol{\sigma} = \mathbf{C} \boldsymbol{\varepsilon} $$ (hooks-law)


name을 정하면 다음과 같이 참조도 가능하다.

```
{eq}`hooks-law`는 Hook's law이다.
```

{eq}`hooks-law`는 Hook's law이다.

### Macro
Macro 사용법은 [Blog - Jupyterbook](https://jupyterbook.org/en/stable/advanced/sphinx.html#sphinx-tex-macros)와 [Docs - mathjax](https://docs.mathjax.org/en/latest/input/tex/macros.html)를 참고한다. 

### 주의사항1
아래 예시처럼 줄과 붙어있으면 inline으로 인식되서 제대로 나오지 않는다.
```
This is equation.
$$ y = x + 1 $$
```

This is equation.
$$ y = x + 1 $$

반드시 한줄 띄어줘야 된다.
```
This is equation.

$$ y = x + 1 $$
```

This is equation.

$$ y = x + 1 $$


## Citation
* [ISBN to BibTeX converter](https://www.bibtex.com/c/isbn-to-bibtex-converter/) 사이트에서 ISBN으로부터 BibTeX를 얻는다.
* `References/_reference.bib` 파일에 입력한다.
* 문서에서 다음과 같이 인용한다.
 
```
{cite}`Fung1965`
```
{cite}`Fung1965`

### 주의사항
#### 1
'(single quote)가 아니라 `(back tick)으로 감싸줘야 한다.

#### 2
문서에서 인용이 되야지만 References 페이지에 나타난다.

#### 3
아래 예시와 같이 url 인용 시 특수문자 있는 경우

```bibtex
@misc{Wiki-hooks-law,
  title = {Hooks law},
  author = {wiki},
  howpublished = {\url{https://en.wikipedia.org/wiki/Hooke%27s_law}},
}
```

`Extension error (sphinxcontrib.bibtex.domain):` 발생한다.

이 떄는 `Hooke%27s_law`을 `Hooke's_law`로 바꾸면 해결된다.

{cite}`Wiki-hooks-law`

### 참고사항
#### 1
추후, `Zotero`, `Mendeley` 등과 같은 전문 프로그램의 도움을 받아 `.bib` 파일 및 원본 파일들을 관리하는 것이 좋을 것 같다.

#### 2
BibTex가 다음과 같이 주어졌다고 하자.
```Bibtex
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

그러면 LeeTM이 name이 되며 name으로 인용이 가능하다.

```
{cite}`LeeTM`
```

{cite}`LeeTM`

#### 3
reference style을 설정할 수 있다.

> Reference   
> [Blog - Jupyterbook](https://jupyterbook.org/en/stable/tutorials/references.html?highlight=bibtex#create-a-bibtex-file)

## Image
### 파일로 첨부
* `_image`폴더를 만들고 그안에 image 파일을 추가한다.
* 아래와 같이 md파일에 입력해서 image 파일을 사용한다.

````
```{figure} _image/Stress-strain_curve.svg
```
````

```{figure} _image/Stress-strain_curve.svg
```

* 아래와 같이 image에 대한 설정 및 caption을 추가 할 수 있다.

````
```{figure} _image/Stress-strain_curve.svg
:name: ss-curve
:scale: 50%

Stress-strain curve
```
````

```{figure} _image/Stress-strain_curve.svg
:name: ss-curve
:scale: 50%

Stress-strain curve
```

* name을 설정한 경우 이름으로 참조도 가능하다.
```
{ref}`ss-curve`는 재료의 Stress-strain curve이다.
```
{ref}`ss-curve`는 재료의 Stress-strain curve이다.

#### 주의사항
파일 이름에 띄어쓰기 있으면 인식하지 못한다.

### 직접 첨부
```
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEYAAAAUCAAAAAAVAxSkAAABrUlEQVQ4y+3TPUvDQBgH8OdDOGa+oUMgk2MpdHIIgpSUiqC0OKirgxYX8QVFRQRpBRF8KShqLbgIYkUEteCgFVuqUEVxEIkvJFhae3m8S2KbSkcFBw9yHP88+eXucgH8kQZ/jSm4VDaIy9RKCpKac9NKgU4uEJNwhHhK3qvPBVO8rxRWmFXPF+NSM1KVMbwriAMwhDgVcrxeMZm85GR0PhvGJAAmyozJsbsxgNEir4iEjIK0SYqGd8sOR3rJAGN2BCEkOxhxMhpd8Mk0CXtZacxi1hr20mI/rzgnxayoidevcGuHXTC/q6QuYSMt1jC+gBIiMg12v2vb5NlklChiWnhmFZpwvxDGzuUzV8kOg+N8UUvNBp64vy9q3UN7gDXhwWLY2nMC3zRDibfsY7wjEkY79CdMZhrxSqqzxf4ZRPXwzWJirMicDa5KwiPeARygHXKNMQHEy3rMopDR20XNZGbJzUtrwDC/KshlLDWyqdmhxZzCsdYmf2fWZPoxCEDyfIvdtNQH0PRkH6Q51g8rFO3Qzxh2LbItcDCOpmuOsV7ntNaERe3v/lP/zO8yn4N+yNPrekmPAAAAAElFTkSuQmCC"/>
```

<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEYAAAAUCAAAAAAVAxSkAAABrUlEQVQ4y+3TPUvDQBgH8OdDOGa+oUMgk2MpdHIIgpSUiqC0OKirgxYX8QVFRQRpBRF8KShqLbgIYkUEteCgFVuqUEVxEIkvJFhae3m8S2KbSkcFBw9yHP88+eXucgH8kQZ/jSm4VDaIy9RKCpKac9NKgU4uEJNwhHhK3qvPBVO8rxRWmFXPF+NSM1KVMbwriAMwhDgVcrxeMZm85GR0PhvGJAAmyozJsbsxgNEir4iEjIK0SYqGd8sOR3rJAGN2BCEkOxhxMhpd8Mk0CXtZacxi1hr20mI/rzgnxayoidevcGuHXTC/q6QuYSMt1jC+gBIiMg12v2vb5NlklChiWnhmFZpwvxDGzuUzV8kOg+N8UUvNBp64vy9q3UN7gDXhwWLY2nMC3zRDibfsY7wjEkY79CdMZhrxSqqzxf4ZRPXwzWJirMicDa5KwiPeARygHXKNMQHEy3rMopDR20XNZGbJzUtrwDC/KshlLDWyqdmhxZzCsdYmf2fWZPoxCEDyfIvdtNQH0PRkH6Q51g8rFO3Qzxh2LbItcDCOpmuOsV7ntNaERe3v/lP/zO8yn4N+yNPrekmPAAAAAElFTkSuQmCC"/>

써두기는 하지만 사용하지 않는 것이 좋을 것 같다.

## 관련 링크
* Jupyter book manual
[Jupyter book](https://jupyterbook.org/en/stable/intro.html)



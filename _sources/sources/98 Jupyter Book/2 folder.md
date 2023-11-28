# 폴더구성법
문서들은 sources 폴더안에 구조를 갖춰서 저장한다.

sources 내부에 폴더 구성을 어떻게 하냐에 따라 페이지 왼쪽에 나타나는 TOC(table of contents)가 달라진다.

아래 예시를 통해 폴더 구성과 TOC의 관계를 확인해보자.

폴더 구성이 다음과 같이 되어 있다고 하자.

```
├─01 math
│  ├─01 Set
│  │      Set.md
│  │      01 Power Set.md
│  │      02 Product Set.md
│  │      03 Relations.md
│  │	  ...
│  │
│  ├─02 Analysis
│  │  │  Analysis.md
│  │  │
│  │  └─01 Metric Space
│  │      │  Metric Space.md
│  │      │  ...
│  │
│  └─03 Geometry
│      │  Geometry.md
│      │
│      └─01 Topology
│              Topology.md
│              ...
│
├─02 programming
│  │  CMake.md
│  │  git.md
│  │  One Definition Rule.md
│  │
│  ├─01 C++
│  │      C++.md
│  │      ...
│  │
│  ├─02 Build
│  │  │  Build.md
│  │  │	 ...
│  │  │

...

```

이 경우 TOC는 다음과 같이 구성된다.

```{figure} _image/0201.png
```

## 규칙

### 1
TOC에 나타나는 순서를 정해주기 위해서 폴더나 md파일 이름을 `## [name]` 형태로 작성한다.

예를들면 `01 math`, `01 Power Set.md`와 같다.

#### 주의사항1
순서가 상관없을 경우, 파일 이름은 저 형태를 지키지 않아도 된다.

하지만 폴더명은 항상 `## [name]` 형태로 작성한다.

또한 반드시 띄어쓰기가 있어야 한다. 
```
01math 	(X) 
01 math (O)
```


#### 주의사항2
TOC에 표시되는 이름은 md파일의 파일명으로 생성되는것이 아니다.

TOC는 md파일 안에 있는 H1 header(#)의 이름으로 생성이 된다.

##### Example1
현재 document1.md파일은 다음과 같다.
```
document1.md

# document1.md
```

만약 document1.md의 내용을 다음과 같이 바꿨다고 가정해보자
```
document1.md

# document123.md
```

그러면 TOC에는 document123으로 표시된다.

### 2
section 폴더 안에는 section 이름과 동일한 .md 파일이 반드시 존재해야된다.

`section.md` 파일은 아래와 같이 TOC에서 section을 눌렀을 때 나타날 페이지 이다.
```{figure} _image/section.png
```

#### 주의사항1
section의 이름은 section.md 파일 안에 있는 H1 header의 이름으로 생성된다.

### 3
파일이 하나만 있는 경우 section 폴더로 만들지 않는다.

#### Example
아래와 같이 section 안에 section.md만 있는 경우를 생각해보자.
```
(bad example)

├─section
│  │  section.md
│
│  document1.md
│  document2.md
```

이 경우에는 다음과 같이 폴더를 구성을 해야 한다.

```
(good example)

│  section.md
│  document1.md
│  document2.md
```
# 빌드

## 빌드 환경 설정
* 손쉬운 가상 환경을 위해 Anaconda를 사용합니다.
* Anaconda prompt에서 Jupyterbook 가상환경을 만듭니다
```
conda create -n jupyterbook python=3.9
```
* 가상환경에 들어갑니다
```
conda activate jupyterbook
```
* conda를 업데이트 해줍니다.
```
conda update --all
```
* Jupyterbook을 설치합니다.
  * mamba를 사용하여 설치 - 현재 되는 방법
```
conda install -c conda-forge mamba
mamba install -c conda-forge jupyter-book
```

> Reference  
> [jupyterbook](https://jupyterbook.org/en/stable/start/overview.html)

### 주의사항1
경로에 한글이 있는 경우 mamba install이 안될 수 있다.

예를 들어 `C/User/(계정명)`에서 계정명이 한글이면 install이 안된다.

### 주의사항2
Jupyter book을 설취하기 위해 conda를 사용하면 왜 인지 안된다
```
conda install -c conda-forge jupyter-book
```  


## 빌드 환경

Python 3.9
Jupyter Book 0.13.1

## 빌드 방법
* anaconda prompt 실행
* jupyter 가상환경을 실행
```
conda activate jupyterbook
```
* make.py가 있는 폴더로 이동
```
cd (path)
```
* 명령어 실행

```bash
# Build
python -m make build

# Clean
python -m make Clean

# Rebuild
python -m make Rebuild
```
* `_build/build/index.html` 파일을 열어서 확인
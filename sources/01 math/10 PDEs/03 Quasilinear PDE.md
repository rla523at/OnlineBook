# Quasilinear PDE
## 정의
PDE의 dependent variable을 $u$라고 하자.

PDE가 $u$의 최고차 derivatives외에는 scalar로 봤을 때 선형결합형태를 가지고 있을 때 `quasilinear PDE`라고 한다.

### 참고
2변수 second order quasilinear PDE의 일반적인 형태는 다음과 같다.

$$ a_{1}(u_{x},u_{y},u,x,y)u_{xx}+a_{2}(u_{x},u_{y},u,x,y)u_{xy}+a_{3}(u_{x},u_{y},u,x,y)u_{yx}+a_{4}(u_{x},u_{y},u,x,y)u_{yy}+f(u_{x},u_{y},u,x,y)=0 $$

$u$의 최고차 derivatives는 2차임으로 2차 derivatives외에 전부 scalar로 보면 $f$는 scalar들의 함수이다.

따라서 $u$의 최고차 derivatives를 기준으로 선형결합형태를 가지고 있다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Partial_differential_equation)
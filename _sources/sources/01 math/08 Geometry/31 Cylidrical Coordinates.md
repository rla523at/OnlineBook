# Cylindrical Coordinates
3차원 Cartesian coordinate 위에 있는 임의의 점은 $(r,\theta,z)$를 갖는 원통좌표계로 표현할 수 있다.

```{figure} _image/3101.png
```

## Change of Variables
Cartesian coordinate에서 Spherical coordinate로 바꿔주는 함수를 $f: \R^3\rightarrow\R^3$라고 하자.

그러면 $f$의 component functions $f^1,\cdots,f^3$는 다음과 같이 정의되어 있다.

$$\begin{aligned} r &= f^1(x,y,z) = \sqrt{x^2+y^2} \\ \theta &= f^2(x,y,z) = \arctan(y/x) \\ z &= f^3(x,y,z) = z \end{aligned} $$

$f$는 역함수$f^{-1}$가 존재하며 $f^{-1}$의 component functions $(f^{-1})^1,\cdots,(f^{-1})^3$는 다음과 같이 정의되어 있다.

$$\begin{aligned} x &= (f^{-1})^1(r,\theta,z) = r\cos\theta \\ y &= (f^{-1})^2(r,\theta,z) = r\sin\theta \\ z &= (f^{-1})^3(r,\theta,z) = z \end{aligned} $$

### Jacobian matrix
$J_f$는 다음과 같다.

$$ J_f = \pdiff{f^i}{x^j}e_{ij} = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\\\ -\dfrac{\sin\theta}{r} & \dfrac{\cos\theta}{r} &0 \\\\ 0&0&1 \end{bmatrix} $$

따라서 1번 미분가능한 함수 $g : \R^3 \rightarrow \R$이 있다고 할 때, 다음이 성립한다.

$$ \begin{bmatrix} \dpdiff{g}{x} & \dpdiff{g}{y} & \dpdiff{g}{z} \end{bmatrix} = \begin{bmatrix} \dpdiff{g}{r} & \dpdiff{g}{\theta} & \dpdiff{g}{z} \end{bmatrix}  \begin{bmatrix} \cos\theta & \sin\theta & 0 \\\\ -\dfrac{\sin\theta}{r} & \dfrac{\cos\theta}{r} &0 \\\\ 0&0&1 \end{bmatrix} $$

> Reference  
> [brown.edu](https://www.brown.edu/Departments/Engineering/Courses/En221/Notes/Polar_Coords/Polar_Coords.htm)  
> [wiki](https://en.wikipedia.org/wiki/Del_in_cylindrical_and_spherical_coordinates)  
> [math.stackexchange](https://math.stackexchange.com/questions/1445288/del-operator-in-cylindrical-coordinates-problem-in-partial-differentiation)  

## Change of Basis
Cartesian coordinate의 standard basis를 $e_{x,y,z}$, cylindrical coordinate의 $Q$점에서의 standard basis를 $e^Q_{r,\theta,z}$라고 하자.

두 basis 모두 $\R^3$의 basis임으로 한 baiss로 다른 basis로 표현할 수 있다.

basis간의 관계를 알아보자.

맨 위의 그림을 통해 $e_z = e^Q_z$임은 자명하며 $e^Q_{r,\theta}$는 $e_{x,y}$를 $\theta$만큼 회전시킨것임을 알 수 있다.

```{figure} _image/3102.png
```

따라서, 다음이 성립한다.

$$ \begin{aligned} e_r &= \cos\theta e_x + \sin\theta e_y \\ e_\theta &= -\sin\theta e_x + \cos\theta e_y \\ e_z &= e_z \end{aligned} $$

이를 행렬형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} e_r & e_\theta & e_z \end{bmatrix} = \begin{bmatrix} e_x & e_y & e_z \end{bmatrix}\begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} $$

따라서, $e_{x,y,z}$에서 $e^Q_{r,\theta,z}$로 변환하는 change of basis matrix를 $B$라고 하면 다음과 같다.

$$ B:= \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} $$

반대로 $e^Q_{r,\theta,z}$에서 $e_{x,y,z}$로 변환하는 change of basis matrix는 $B^{-1}$이다.

$$ B^{-1} = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} e_x &= \cos\theta e_r - \sin\theta e_\theta \\ e_y &= \sin\theta e_r + \cos\theta e_\theta \\ e_z &= e_z \end{aligned} $$

## Change of Coordinate
$\R^3$의 임의의 vector를 $v$라고 하면 어떤 $a^1,\cdots,a^3,b^1,\cdots,b^3 \in \R$에 대해서 다음이 성립한다.

 $$\begin{aligned} v &= a^1e_x + a^2e_y+a^3e_z \\&= b^1e_r + b^2e_\theta + b^3e_z  \end{aligned}$$

$e_{x,y,z}$에서 $e^Q_{r,\theta,z}$로 변환될 때 change of coordinate matrix를 $C$라고 하면 다음과 같다.

$$ C = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} $$

따라서 다음이 성립한다.

$$ \begin{bmatrix} b^1 \\ b^2 \\ b^3 \end{bmatrix} = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} \begin{bmatrix} a^1 \\ a^2 \\ a^3 \end{bmatrix} $$

## Change of Del Operator
1번 미분가능한 함수 $g : \R^3 \rightarrow \R$이 있다고 하자.

$\nabla g$는 다음과 같다.

$$ \begin{aligned} \nabla g &= \pdiff{g}{x}e_x + \pdiff{g}{y}e_y + \pdiff{g}{z}e_z \\&= \begin{bmatrix} \dpdiff{g}{x} & \dpdiff{g}{y} & \dpdiff{g}{z} \end{bmatrix} \begin{bmatrix} e_x \\ e_y \\ e_z \end{bmatrix} \\&= \begin{bmatrix} \dpdiff{g}{r} & \dpdiff{g}{\theta} & \dpdiff{g}{z} \end{bmatrix}  \begin{bmatrix} \cos\theta & \sin\theta & 0 \\\\ -\dfrac{\sin\theta}{r} & \dfrac{\cos\theta}{r} &0 \\\\ 0&0&1 \end{bmatrix} \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0&0&1 \end{bmatrix} \begin{bmatrix} e_r \\ e_\theta \\ e_z \end{bmatrix} \\&= \begin{bmatrix} \dpdiff{g}{r} & \dpdiff{g}{\theta} & \dpdiff{g}{z} \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\\\ 0 & \dfrac{1}{r} &0 \\\\ 0&0&1 \end{bmatrix} \begin{bmatrix} e_r \\ e_\theta \\ e_z \end{bmatrix} \\&= \pdiff{g}{r}e_r + \frac{1}{r}\pdiff{g}{\theta}e_\theta + \pdiff{g}{z}e_z \end{aligned} $$

따라서, cyliderical coordinate의 del operator는 다음과 같이 표현할 수 있다.

$$ \nabla = \begin{bmatrix} \dpdiff{g}{r} & \dfrac{1}{r}\dpdiff{g}{\theta} & \dpdiff{g}{z} \end{bmatrix} $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Del_in_cylindrical_and_spherical_coordinates)  


## 주의
$(r,\theta,z)$는 cylindrical coordinates에서 position vector의 standard basis에 대한 coordinates가 아니다.

### Cartesian coordinate
Cartesian coordinate에서 $(x,y,z)$로 표현되는 점 $P$가 있다고 하자.

이 떄, $P$점의 position vector를 Cartesian coordinate의 standard basis $e_{x,y,z}$로 표현하면 다음과 같다.

$$ \overline{OP} = xe_x + ye_y + ze_z $$

따라서, $\overline{OP}$의 standard basis에 대한  coordinates는 $(x,y,z)$이다.

그럼으로 $P$점을 나타내는 $(x,y,z)$와  $\overline{OP}$의 standarad basis에 대한 coordinate는 같다.

### Cylindirical coordinate
Cylindrical coordinate에서 $(r,\theta,z)$로 표현되는 점 $Q$가 있다고 하자.

이 떄, $Q$점의 position vector를 cylindrical coordinate의 $Q$점에서의 standard basis $e^Q_{r,\theta,z}$로 표현하면 다음과 같다.

$$ \overline{OQ} = re^Q_r + ze^Q_z $$

따라서, $\overline{OQ}$의 $Q$점에서의 standard basis에 대한 coordinates는 $(r,0,z)$이다.

그럼으로 $Q$점을 나타내는 $(r,\theta,z)$와 $\overline{OQ}$의 $Q$점에서의 standard basis에 대한 coordinate $(r,0,z)$는 다르다.
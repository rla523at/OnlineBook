# Diffuse Reflection
Diffuse reflection(확산 반사)는 빛이 물체의 표면에 닿았을 때, 여러 방향으로 고르게 퍼지는 반사 현상을 의미한다.

물체 표면이 거칠거나 미세한 불균형을 가지고 있으면 광선이 표면에 닿았을 때 특정한 방향으로 반사되지 않고 여러 방향으로 퍼지게 된다. 이러한 특성 때문에 확산 반사를 일으키는 물체는 다양한 각도에서 동일한 밝기로 보이는 특징이 있다.

> Reference  
> [wikipedia - Diffuse_reflection](https://en.wikipedia.org/wiki/Diffuse_reflection)  

## Lambert's Cosine Law
표면의 법선 벡터와 표면에서 관측자로의 벡터의 각도를 $\theta$라고 하자.

이 때, 표면의 reflected radiant intensity가 $\cos\theta$에 비례한다는 법칙이다.

Lambert's cosine law를 만족하는 표면을 Lambertian surface라고 하며, Lambertian sruface에서 발생하는 반사현상을 Lambertian reflection이라고 한다.

Lambertian reflection이 발생하면, 모든 방향으로의 radiance가 일정하게 되며 이를 equal brightness effect라고 한다.

따라서, Lambertian reflection은 관측자 각도와 관계 없이 동일한 밝기로 보이게 됨으로 이상적인 diffuse reflection 모델이다.

Lambertian reflection의 경우 equal brightness effect에 의해 brightness는 관측자와 무관하게 표면의 법선 벡터와 광원으로의 벡터간의 각도에 의해서만 결정된다.

$I_L$을 광원의 강도, $l$을 광원으로의 벡터, $n$을 법선벡터 그리고 $C$를 광원의 색깔이라고 하면 brightness $B_D$는 다음과 같이 계산된다. 

$$ B_D = I_L (l\cdot n) C $$

> Reference  
> [wikipedia - Lambertian_reflectance](https://en.wikipedia.org/wiki/Lambertian_reflectance)  
> [wikipedia - Lambert%27s_cosine_law](https://en.wikipedia.org/wiki/Lambert%27s_cosine_law)  
> [{cite}`Luna` Chpater8.4]  

### Equal Brightness Effect
radiance source의 넓이를 $dA$, 임의의 solid angle을 $d\Omega$ surface normal 방향으로 방출되는 radiance를 $I$라고 하자.

Lambert's cosine law를 만족함으로 surface normal과의 각도를 $\theta$라고 할 때, 방출되는 radiant energy는 다음과 같다.

$$ I \cos\theta d\Omega dA $$

다음으로, 관측자가 radiant energy를 받아들이는 넓이를 $dA_0$ 그리고 surface normal 위치에 있을 때 source를 보는 solid angle을 $d\Omega_0$라고 하자.

관측자와 surface normal 과의 각도를 $\theta$라고 하면, source가 차지하는 solid angle은 $d\Omega_0\cos\theta$가 된다.

이 때, source가 특정 각도로 방출하는 radiant energy를 전부 관측자가 관찰한다고 일반성을 잃지 않고 가정하면 관측자가 관측한 radiance는 다음과 같다.

$$ I_0 = \frac{I \cos\theta d\Omega dA}{d\Omega_0 \cos\theta dA_0} = \frac{I d\Omega dA}{d\Omega_0 dA_0}  $$

따라서, 관측자와의 각도와 관계없이 radiance는 일정하다.

> Reference  
> [wikipedia - Lambert%27s_cosine_law](https://en.wikipedia.org/wiki/Lambert%27s_cosine_law)  

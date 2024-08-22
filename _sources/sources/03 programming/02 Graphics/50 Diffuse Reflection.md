# Diffuse Reflection
Diffuse reflection(확산 반사)는 빛이 물체의 표면에 닿았을 때, 여러 방향으로 고르게 퍼지는 반사 현상을 의미한다.

물체 표면이 거칠거나 미세한 불균형을 가지고 있으면 광선이 표면에 닿았을 때 특정한 방향으로 반사되지 않고 여러 방향으로 퍼지게 된다. 이러한 특성 때문에 확산 반사를 일으키는 물체는 다양한 각도에서 동일한 밝기로 보이는 특징이 있다.

> Reference  
> [wikipedia - Diffuse_reflection](https://en.wikipedia.org/wiki/Diffuse_reflection)  

## Lambertian Reflection
Lambertian reflection은 이상적인 diffuse reflection 모델이다.

Lamber reflection에서는 표면에서 반사되는 빛의 밝기가 모든 방향에 일정하여 관찰자의 시선에 관계 없이 일정하다고 가정한다.

따라서, 표면의 밝기는 표면의 법선 벡터와 광원으로의 벡터간의 각도에 의해서만 결정된다.

$I_L$을 광원의 강도, $l$을 광원으로의 벡터, $n$을 법선벡터 그리고 $C$를 광원의 색깔이라고 하면 Diffuse reflection에 의한 반사광 $B_D$는 다음과 같이 계산된다. 

$$ B_D = I_L (l\cdot n) C $$

> Reference  
> [wikipedia - Lambertian_reflectance](https://en.wikipedia.org/wiki/Lambertian_reflectance)  

## Lambert's Cosine Law



> Reference  
> [{cite}`Luna` Chpater8.4]  
> [wikipedia - Lambert%27s_cosine_law](https://en.wikipedia.org/wiki/Lambert%27s_cosine_law)  
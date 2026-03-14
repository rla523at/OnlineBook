# Miscelleneous
## Solid Angle

> Refernece  
> [wikipeida - Solid_angle](https://en.wikipedia.org/wiki/Solid_angle)  
> [wolfram - SolidAngle](https://mathworld.wolfram.com/SolidAngle.html)   

## Radiant Flux
단위 시간당 방출, 입사 되는 radiant energy이다.

$$ \Phi_e = \diff{Q_e}{t} $$

## Radiant Intensity
단위 solid angle당 방출,입사 되는 radiant flux이다.

$$ I_{e,\Omega} = \pdiff{\Phi_e}{\Omega} $$

> Reference  
> [wikipedia - Radiant_intensity](https://en.wikipedia.org/wiki/Radiant_intensity)  

## Radiance
단위 solid angle, 단위 projected area당 방출, 입사 되는 radient flux이다.

$$ L_{e, \Omega} = \frac{\partial^2\Phi_e}{\partial_\Omega\partial(A\cos\theta)} $$

source에서 방출되는 radiance를 계산할 때, $A$는 source의 표면적이 되며 $\Omega$는 빛이 방출되는 고체각을 의미한다.

관측자에게 입사되는 radiance를 계산할 때는 $A$는 관측자가 빛을 받아들이는 표면적이 되며 $\Omega$는 source가 관측자의 시야에서 차지하는 각도를 나타낸다.

> Reference  
> [wikipedia - Radiance](https://en.wikipedia.org/wiki/Radiance)  

## Specular Reflection
specular reflection은 표면에서 거울과 같은 반사가 이루어지는 현상을 의미한다.

specular reflection의 경우 surface normal을 기준으로 입사되는 빛과 반사되는 빛이 이루는 각도를 각각 입사각과 반사각이라고 할 때, 입사각과 반사각이 같은 law of reflection을 따른다.

따라서, specular refelction의 경우 반사각 위치에 빛이 모이기 때문에, 관측자의 위

> Reference  
> [wikipedia - Specular_reflection](https://en.wikipedia.org/wiki/Specular_reflection)  

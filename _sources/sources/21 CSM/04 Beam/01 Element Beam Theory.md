# Element Beam Theory

## Beam

```{figure} _image/0101.png
```

`보(beam)`는 위의 그림과 같이 축 방향에 수직인 `횡하중(transverse loading)`이 작용해 `굽힘(bending)`을 받아 휘는 부재를 의미한다. 

## Assumption

### Pure bending
Element beam theory에서는 오직 bending만 발생하면서 하중을 지지한다고 가정한다. 

즉, `비틀림(twisting)`과 같은 현상은 발생하지 않는다고 가정한다.

이는 그림의 파란색 면으로 표현된 `logitudinal plane of symmetry`가 존재하여, 이 절단면을 기준으로 하중이나 지지조건이 대칭이라고 가정한것과 같다.

longitudinal plane of symmetry를 `plane of bending`이라고 부르기도 한다.

> Reference  
> [kelly chap7.4.1](https://pkel015.connect.amazon.auckland.ac.nz/SolidMechanicsBooks/Part_I/BookSM_Part_I/07_ElasticityApplications/07_Elasticity_Applications_04_Beam_Theory.pdf)  

## Neutral Surface
```{figure} _image/0201.png
```

위의 그림에 있는 Beam을 $x-z$평면상에 있는 얇을 면을 $+y$축방향으로 쌓아서 만들었다고 생각해보자.

그리고 applied force로 $-y$방향의 transverse load가 주어졌다고 하자.

그러면 그림과 같이 beam이 변형되면서 변형된 beam의 윗면에 있던 면은 길이가 줄어들면서 압축력이 발생하고 beam의 아랫면에 있던 면은 길이가 늘어나면서 인장력이 발생하게 된다. 

이 때, 보 내부에서 물리적 특성은 연속적으로 변하기 때문에 보의 어느 단면에는 압축력과 인장력이 작용하지 않고 동일한 길이를 유지하는 면이 존재하게 된다.

이러한 면을 `중립면(neutral surface)`이라고 정의한다.

> Reference  
> [kelly chap7.4.1](https://pkel015.connect.amazon.auckland.ac.nz/SolidMechanicsBooks/Part_I/BookSM_Part_I/07_ElasticityApplications/07_Elasticity_Applications_04_Beam_Theory.pdf)  

## Axis of the beam
Neutral surface와 longituidnal plane of symmetry의 교선을 `axis of the beam`이라고 한다.

Tranver load가 작용하면 neutral surface가 변형고 axis도 변형된다. 

이 때, 변형된 axis를 `deflection curve`라고 부르기도 한다.

일반적으로 beam의 coordinate system은 $x$축을 axis of the beam으로 두고 $y$축을 transverse direction으로 둔다. 

이 경우에 $x-y$평면은 longitudinal plane of symmetry가 된다.

> Reference  
> [kelly chap7.4.1](https://pkel015.connect.amazon.auckland.ac.nz/SolidMechanicsBooks/Part_I/BookSM_Part_I/07_ElasticityApplications/07_Elasticity_Applications_04_Beam_Theory.pdf)  
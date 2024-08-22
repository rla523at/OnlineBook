# ETC

Memory Bandwidth는 Memory Clock X Memory Bus로 결정된다.

> Reference   
> [blog - 메모리 반도체 대역폭 (클럭 / 버스 / 입출력 라인 / 노스브릿지 / FSB / GDDR / 듀얼 채널)](https://blog.naver.com/shakey7/221435517430)   


Memory에 접근하는데 소요되는 시간을 계산하려면, 먼저 Memory에 접근하는데 몇 clock이 소요되는 지 파악한다. 다음으로 Memory Clock 성능을 파악하여 특정 Memory에 접근하는데 몇초가걸리는지 알 수 있다.

예를 들어, Memory 접근하는데 10 clock이 필요하고, Memory Clock이 2GHZ면 Memory clock당 0.5ns가 필요함으로 memory 접근에는 총 5ns가 필요하다.
# Memory
`메모리(memory)`란 프로그램 수행에 필요한 명령어와 데이터를 저장하는 장치이다. 메모리는 크게 CPU가 직접 접근 가능한 내부 기억장치인 `주기억장치(primary memory)`와 외부 기억장치인 `보조 기억장치(secondary memory)`로 분류된다. DRAM(RAM, DDR4) 등의 메모리, CPU 안에 있는 레지스터(register)와 캐쉬(cache memory) 등이 주기억장치 이며 SSD, HDD 등이 보조 기억장치이다.

## Last Level Cache (LLC)
LLC 란, 현대 CPU 아키텍처에서 코어별 L1/L2 캐시를 거친 뒤에 접근하는 최종 캐시 단계를 의미한다 예를들어 Intel CPU 의 경우 L3 캐시가 LLC 에 해당한다.

## DRAM, Local Memory, Remote Memory
먼저 **DRAM**은 **Dynamic Random Access Memory**의 약자로, 우리가 흔히 말하는 시스템 메인 메모리를 의미한다.

그런데 **멀티 소켓(Multi-socket)** 또는 **NUMA(Non-Uniform Memory Access)** 구조에서는 각 CPU 소켓마다 물리적으로 연결된 DRAM 의 부분이 따로 있을 수 있다. 이때 각 소켓에서 바라보는 메모리의 위치에 따라 접근 속도(지연 시간, Latency)가 달라진다.

### 1. 로컬 메모리(Local Memory)
- **정의**: 현재 코어(또는 소켓)가 직접 연결되어 있는 물리적 DRAM  
- **특징**:  
  - 해당 CPU 소켓에서 접근하기에 가장 **지연이 낮고** 대역폭이 **높습니다.**  
  - “내 소켓”에 붙어있는 DRAM이라고 이해하시면 됩니다.

### 2. 원격 메모리(Remote Memory)
- **정의**: 현재 코어(또는 소켓)와는 다른 소켓에 물리적으로 연결된 DRAM  
- **특징**:  
  - NUMA 환경에서, 다른 CPU 소켓까지 인터커넥트(예: QPI, UPI, Infinity Fabric 등)를 통해 접근해야 하므로, **지연이 높고** 대역폭도 **상대적으로 낮습니다.**  
  - “남의 소켓”에 붙어있는 DRAM이라고 볼 수 있습니다.

### 3. DRAM
- **정의**: 컴퓨터가 사용하는 **주 메모리(시스템 메모리)**의 물리적 형태  
- **특징**:  
  - 로컬 메모리, 원격 메모리 모두 사실은 DRAM이지만, 어느 소켓에 물리적으로 붙어 있느냐에 따라 “로컬” 또는 “원격” 메모리로 구분됩니다.  

# Graphics

Todo
* back buffer resource type 확인하기 ID3D12Resource::GetDesc
* mipmap 과 sampler
* D3D12_BUFFER_UAV 의 CounterOffsetInBytes

## 모니터 출력 과정
<details> <summary> <h3 style="display:inline-block"> 1. 최종 프레임 버퍼 데이터 준비  </h3></summary>
그래픽 연산 후, 모든 픽셀에 대한 계산이 완료되면 GPU는 그 결과를 프레임 버퍼( Frame Buffer ) 라 불리는 GPU 메모리(VRAM)상의 특정 메모리 영역에 저장한다. 이 Frame Buffer는 모니터에 표시할 최종 이미지(픽셀들의 컬러 값) 정보가 담겨 있는 배열 형태의 데이터다.
</details>


<details> <summary> <h3 style="display:inline-block"> 2. 디스플레이 컨트롤러( Display Controller )의 역할  </h3></summary>
Frame Buffer 가 준비되면, GPU 내부에 있는 **디스플레이 엔진( Display Engine )** 또는 **디스플레이 컨트롤러( Display Controller )** 가 모니터에 출력할 Frame Buffer ( Front Buffer ) 의 데이터를 읽어온다.

이 때, Display Controller 는 화면의 왼쪽 위 픽셀부터 시작해 오른쪽으로, 그리고 다음 줄로 이동하는 식으로 한 줄씩 순차적으로 Front Buffer 를 읽는다. 이렇게 Front Buffer 전체 픽셀 데이터를 줄 단위로 차례차례 읽어오는 과정을 스캔아웃( scan-out )이라고 한다.

그리고 Display Controller 는 일정한 픽셀 클록( Pixel Clock ) 주파수에 맞추어 매 픽셀 데이터를 scan-out 한다. Pixel Clock 은 해상도와 주사율에 따라 결정된다.
</details>


<details> <summary> <h3 style="display:inline-block"> ## 3. 신호 변환 및 전송   </h3></summary>
Display Controller 가 읽은 Front Buffer 정보는 모니터에 전송하기 위해 **HDMI, DisplayPort, DVI** 등과 같은 디지털 인터페이스, 디지털 신호로 변환된다.

이 단계에서 픽셀 색상 정보, 동기 신호(수직, 수평 동기), 빈 구간(blanking interval) 등의 패턴에 맞춰 데이터가 전송 라인을 통해 모니터로 전송됩니다.
</details>


<details> <summary> <h3 style="display:inline-block"> 4. 모니터 수신 및 디스플레이 드라이버 회로 처리 </h3></summary>
모니터는 GPU에서 전송받은 디지털 신호를 해독하여 각 픽셀에 해당하는 RGB 데이터 스트림을 얻는다. 모니터 내부의 스케일러, 컬러 변환 회로, 오버드라이브(Overdrive) 처리 등이 있을 수 있으며, 이를 통해 모니터 패널에 정확한 색상과 밝기를 나타내기 위한 신호로 변환된다.

최종적으로 모니터 내부의 드라이버 IC와 전자 회로는 각 픽셀 전극을 구동하여 LCD 셀을 열거나 OLED 소자를 점등해, 실제 빛을 발생 또는 투과시키게 된다. 이로써 우리가 화면에 최종 그림을 보게 된다.

이 전 과정은 끊임없이 반복되며, 모니터의 주사율(예: 60Hz, 144Hz)에 따라 매 주기마다 새로운 프레임이 읽히고 전송되며 표시된다.
</details>


## Tearing
주사 주기 동안 화면이 순차적으로 업데이트되어 최종적으로 한 프레임이 온전하게 화면 전체에 표시되는 상황과, tearing 이 발생하는 상황의 차이는 프레임 데이터가 변경되는 시점과 디스플레이의 스캔아웃 시점 간의 동기화 여부에 달려있다.

<details> <summary> <h3 style="display:inline-block"> 정상적인 상태(동기화된 상태, 예: VSync 활성화) </h3></summary>
- Display Controller 는 Front Buffer 를 상단에서 하단으로 순차적으로 읽어와 모니터로 전송한다. 이 과정을 frame interval 동안 진행하며, 이 시점에선 한 프레임 내에서 상·하단 픽셀들이 순차적으로 새 프레임 데이터로 바뀌어 간다.  
- 중요한 점은, Front Buffer 변경 시점이 디스플레이가 프레임을 모두 표시하고 난 직후, 즉 수직 동기화 간격(VBlank) 시점에서 일어난다.
- 이를 통해 한 프레임이 표시되는 동안 Front Buffer 의 내용은 변하지 않으며, 프레임 전체가 "한 세트"로 순차적 업데이트를 거친 뒤 다음 프레임으로 깔끔하게 전환된다.  
- 결국 모니터에선 상단에서 하단까지 시간이 지남에 따라 새로운 프레임이 천천히 "스캔"되며 표시되지만, 프레임 중간에 갑자기 데이터가 바뀌는 일이 없어 완전히 한 프레임이 끝난 시점에 전체 화면이 새로운 프레임으로 통일되게 됩니다.
</details>

<details> <summary> <h3 style="display:inline-block"> Tearing 상태 </h3></summary>
- Tearing 은 Display Controller 가 scan out 하는 도중에 Front Buffer 의 내용이 변경되면 발생하는 현상이다.
- 예를 들어, Display Controller 가 화면 상단 부분을 scan out 하다 화면 중간쯤에 도달했을 때 Front Buffer 를 스왑해버린다면, 화면 하단 부분은 새로운 Front Buffer 에서  scan out 하게 된다.  
- 이렇게 되면 화면 상단 부분과 하단 부분은 서로 다른 Front Buffer 의 내용을 표시되게 되어 화면에 경계선이 발생하며 "찢어진(Tear)" 듯한 현상이 관찰된다.
- 이 현상은 수직 동기화(VSync)나 프레임 레이트 동기 기술(G-Sync, FreeSync 등)이 꺼져 있거나 제대로 맞춰지지 않아 Front Buffer 업데이트 시점이 Frame interval 과 어긋날 때 주로 발생한다.
</details>






VBLANK
* Display 에서 화면을 출력하는 시간( 60HZ 모니터면 16.66ms )과 GPU 에서 화면에 출력할 데이터를 만드는데 필요한 시간 사이의 간격이다.




Render Pass vs Pipeline Pass
* Render Pass
  * Render Target 을 중심으로 수행되는 일련의 명령 집합이다.
* Pipeline Pass
  * graphics 또는 compute Pipe line 에서의 한 번의 실행 단위를 의미한다.
  * 각 Pipeline Pass 는 렌더링 파이프라인에서 각각의 작업 단위를 나타내며 특정 shader 를 실행한다.
  * 하나의 Render Pass 내에 여러개의 Pipeline Pass 가 실행될 수 있다.
    * Compute Shader Pipeline Pass
      * GPU 에서 데이터를 계산하고 텍스처에 결과를 기록
    * Vertex Processing Pipeline Pass
      * 정점 데이터를 변환하고 래스터화에 필요한 정보를 생성
     
* GRP ( Graphics Render Pass ) 와 CRP ( Compute Render Pass )
  * GRP
    * 그래픽스 명령( 드로우 호출... ) 으로 구성된 GPU 작업 단위를 의미
    * PSO 에 의해 정의되며 Vertex Shader, Pixel Shader 등의 Shader stage 를 포함한다.
  * CRP
    * compute shader 를 통해 데이터를 계산하거나 텍스처와 같은 자원에 결과를 기록한다.

# Graphics

* Render Pass vs Pipeline Pass
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

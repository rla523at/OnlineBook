# DirectX11
DirectX는 Microsoft에서 개발한 API(응용 프로그램 인터페이스) 집합으로, 멀티미디어 애플리케이션, 특히 게임 및 비디오 애플리케이션을 쉽게 개발할 수 있게 도와준다. DirectX API는 다양한 하위 API로 구성되어 있으며, 각 하위 API는 특정 멀티미디어 기능을 처리하는 데 사용된다. 

## Effect Framework

Effect Framework 는 shader 프로그램과 관련된 설정을 관리하기 위한 고수준 API 이다. 

shader 와 그에 관련된 상태를 하나의 effect 파일(.fx)에 정의하여 그래픽 렌더링 파이프라인을 보다 쉽게 관리할 수 있게 해준다. 

> Reference  
> [learn.microsoft - d3d11-graphics-programming-guide-effects](https://learn.microsoft.com/ko-kr/windows/win32/direct3d11/d3d11-graphics-programming-guide-effects)  

### 주요 개념 및 구성 요소

- **Effect (효과):** 여러 상태, 셰이더, 변수 등의 집합이다. Effect 파일(.fx)에서 HLSL 코드를 작성하고, 필요한 렌더링 상태를 함께 정의한다.

- **Technique (기법):** Effect 안에 정의된 렌더링 기술의 모음으로, 각 Technique는 특정한 렌더링 방법을 나타낸다. 다양한 렌더링 기법을 가진 하나의 Effect에서 상황에 따라 적절한 Technique를 선택하여 사용할 수 있다.

- **Pass (패스):** Technique 내부에는 여러 Pass가 있을 수 있으며, 각각의 Pass는 렌더링 파이프라인을 구성하는 하나의 단계이다. 이 Pass를 통해 정점 셰이더, 픽셀 셰이더, 렌더 상태 등이 설정된다.

- **Variables (변수):** Effect 파일 내에서 사용할 수 있는 다양한 변수로, 텍스처, 행렬, 조명 데이터 등을 관리할 수 있다. 애플리케이션에서 이러한 변수 값을 동적으로 변경할 수 있다.

### 작동 방식
1. **.fx 파일 로드:** Effect 파일은 HLSL 로 작성되며, 여기에는 셰이더 코드와 렌더링 상태가 포함된다. 이 파일을 Effect Framework 를 통해 로드하면, 내부적으로 HLSL 컴파일러가 파일을 파싱하고 셰이더를 컴파일한다.

2. **Technique 및 Pass 선택:** 원하는 Technique 를 선택하고 그 안에서 Pass 를 실행한다. 각 Pass 는 필요한 shader 를 셋업하고 그래픽 파이프라인에 적용한다.

3. **변수 설정 및 업데이트:** 애플리케이션에서 Effect 의 변수를 설정하고, 셰이더가 이 값을 사용하여 렌더링을 수행한다.

Effect 를 사용하려면 먼저 .fx파일 안의 필요한 shader 프로그램들을 컴파일 해야한다.

> Reference  
> [blog.eeum1108](https://m.blog.naver.com/eeum1108/222026507900)  

### 참고
Direct3D 11의 Effect Framework는 Direct3D 9에서 처음 도입된 이후, Direct3D 10을 거쳐 Direct3D 11에서도 사용되었지만, Direct3D 11에서는 더 이상 공식적으로 지원되지 않는 기능이다.

따라서, 최신 Direct3D 11 애플리케이션 개발에서는 권장되지 않는다.

Direct3D 11에서 Effect Framework가 더 이상 공식적으로 지원되지 않는 이유는 다음과 같다.

- **성능 최적화 부족:** Effect Framework는 다양한 기능을 제공하지만, 그래픽 엔진에 최적화된 형태로 사용하기 어렵고, 불필요한 오버헤드가 발생할 수 있다.

- **HLSL의 성숙:** Direct3D 11에서는 HLSL 코드와 그래픽 파이프라인을 더 직접적으로 제어할 수 있도록 제공되어 개발자에게 더 큰 유연성을 준다. 이에 따라, Effect Framework 없이도 더 효율적으로 렌더링 파이프라인을 구성할 수 있게 되었다.

- **Direct3D API의 발전:** Direct3D 11은 더 발전된 파이프라인 상태 관리 및 셰이더 관리 방식을 제공하기 때문에, Framework 없이도 더 나은 성능과 제어를 가능하게 한다.


## Direct3D
Direct3D는 3D 그래픽스를 처리하는 API로, 게임 및 그래픽 응용 프로그램에서 가장 널리 사용된다. Direct3D는 하드웨어 가속을 통해 고성능 3D 렌더링을 제공하며, 최신 그래픽 카드의 기능을 최대한 활용할 수 있게 한다.

## Direct2D
Direct2D는 2D 그래픽스를 처리하는 API로, 고성능의 2D 렌더링을 제공한다. 이는 벡터 그래픽, 텍스처 매핑, 픽셀 쉐이딩 등을 지원하며, 하드웨어 가속을 통해 효율적인 렌더링을 가능하게 한다.

## DirectWrite
DirectWrite는 텍스트 렌더링을 처리하는 API로, 고품질의 텍스트 출력을 제공한다. 이는 다양한 글꼴, 스타일, 레이아웃을 지원하며, 특히 ClearType 텍스트 렌더링 기술을 통해 가독성을 향상시킨다.

## DirectCompute
DirectCompute는 GPU를 이용한 일반 연산을 처리하는 API로, 그래픽스가 아닌 계산 작업을 수행하는 데 사용된다. 이는 GPGPU(General-Purpose computing on Graphics Processing Units) 프로그래밍을 지원하며, 물리 연산, 이미지 처리, 과학 계산 등에 활용된다.

## DirectSound
DirectSound는 오디오 스트림의 재생, 녹음, 믹싱을 처리하는 API로, 실시간 오디오 처리를 지원한다. 이는 다양한 오디오 효과를 적용할 수 있으며, 멀티채널 사운드를 지원한다.

## DirectMusic
DirectMusic는 음악 및 사운드 효과를 관리하는 API로, MIDI 스트림의 재생과 디지털 사운드의 믹싱을 지원한다. 이는 다이나믹한 음악 생성과 재생을 위한 다양한 기능을 제공한다.

## DirectInput
DirectInput는 사용자 입력을 처리하는 API로, 키보드, 마우스, 게임 컨트롤러 등 다양한 입력 장치를 지원한다. 이는 고성능 입력 처리를 가능하게 하며, 사용자 입력에 빠르게 반응할 수 있다.

## DirectPlay
DirectPlay는 네트워크 통신을 처리하는 API로, 멀티플레이어 게임의 네트워크 기능을 지원한다. 이는 게임 상태 동기화, 채팅, 로비 관리 등을 포함한 다양한 네트워크 기능을 제공한다.

## DirectShow
DirectShow는 오디오 및 비디오 스트림의 재생과 편집을 처리하는 API로, 멀티미디어 애플리케이션에서 널리 사용된다. 이는 다양한 코덱과 필터를 지원하며, 스트림의 캡처, 처리, 재생 기능을 제공한다.

## DXGI (DirectX Graphics Infrastructure)
DXGI는 Direct3D와 하드웨어 드라이버 간의 상호작용을 관리하는 API로, 스왑 체인, 디스플레이 설정, 리소스 공유 등을 관리한다. 이는 Direct3D의 하위 시스템으로, 그래픽 어댑터와 디스플레이 장치 간의 통신을 조정한다.

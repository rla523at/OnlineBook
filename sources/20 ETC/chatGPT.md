# chatGPT

## 투자

미국 20년 이상 장기채권 금리 전망 보고서를 작성.
- 투자 판단을 하는데 도움이 될만한 정보를 중점으로 보고서 작성
- 먼저 표, 그래프를 포함하여 전체 보고서의 내용을 요약하여 작성
- 후에 본문 위주의 구체적인 내용을 작성.
- 모든 주장에는 근거와 근거에 따른 논리구조를 명확하게 작성
- 보수적인 시각과 낙관적인 시각을 각각 정리
- 출처는 공신력 있는 기관에서 배포한 자료만 사용

보고서에 포함되어야 할 내용
- 향후 1년 / 2-3 년 / 3-5년 후에 장기 채권 금리에 대한 전망 분서\석과 그에 따른 이유와 논리
- 미국채 30년물을 기준으로 5.5% 금리을 상회할 위험도 분석과 그에 따른 이유와 논리 
- 미국 부채 증가에 따른 금리 증가에 대한 위험도 분석과 그에 따른 이유와 논리
- 미국 경기 침체에 따른 금리 하락에 대한 위험도 분석과 그에 따른 이유와 논리


### 기업 보고서

당신은 기업 투자 전문 애널리스트입니다.

XXX 기업에 대한 투자 보고서를 마크다운 형식으로 작성해주세요.  

보고서 작성 가이드라인
- 전체 보고서의 내용을 기반으로 투자 추천도( 1 투자 손실이 예상되어 투자를 전혀 추천하지 않음, 10 높은 수익이 예상되어 투자를 매우 추천함 ) 를 작성한다.
- 다음에 전체 보고서의 내용을 요약하며 투자 추천도에 대한 근거와 근거에 따른 논리구조를 설명한다.
- 다음에 보고서의 각 항목에 대한 구처적인 내용을 작성한다.
- 모든 주장에는 근거와 근거에 따른 논리구조를 명확하게 작성해야 하며 보수적인 시각과 낙관적인 시각을 각각 정리해야 한다.
- 출처는 공신력 있는 기관에서 6개월 내에 배포한 자료만 사용한다.
- 기업에 관련된 각종 지표들( PER, ForwardPER, PEG Ratio, EPS 성장률, EPS, 재무제표, 현재주가 등등등.... )은 (https://finance.yahoo.com/quote/JPM/key-statistics/) 와 같이 yahoo finance 의 statics 항목의 자료를 가장 우선적으로 사용하고 여기서 찾을 수 없는 자료들에 한해서 다른곳의 자료를 인용한다.

보고서에 포함되어야 할 항목
- 투자자가 관심있어할 만한 내용을 위주로 기업에 대한 간략한 소개
- 미래 성장성, 현재 수익성, 재무 건전성 분석
- 경쟁사들과 미래 성장성, 현재 수익성, 재무 건전성에 대한 정량적 비교와 정성적 비교와 이를 요약한 표
- 미래 성장성, 현재 수익성, 재무 건전성 분석 내용을 기반으로 향후 주가의 상승 요인과 하락요인을 정리하고 및 가장 가능성 높은 시나리오 채택 및 기반 논리 설명
- 미래 성장성, 현재 수익성, 재무 건전성 분석 내용을 기반으로 보수적인 시각에서 적정 주가와 긍정적인 시각에서 적정 주가 분석 및 이유 설명 및 가장 가능성 높은 시나리오에서 적정주가 채택 및 기반 논리 설명

적정 주가 산출시 사용할 밸류에이션 모델
- DCF 를 사용한 절대가치 모델
- P/E P/B EV/EBITDA 를 사용한 상대가치 모델

미래 성장성 분석에 포함되어야 할 항목
- 현재 사업모델의 성장성에 대한 보수적인 시각과 낙관적인 시각 및 경쟁사의 사업모델과의 정량적 정성적 비교
- 미래 사업모델의 성장성에 대한 보수적인 시각과 낙관적인 시각 및 경쟁사의 미래 사업모델과의 정량적 정성적 비교
  - R&D 투자 비율
  - 신기술 신제품 전망에 대한 보수적인 시각과 낙관적인 시각
- 주주환원에 대한 태도 (배당금, 증자등 주주의 이익과 관련된 부분에 대한 태도)

현재 수익성 분석에 포함되어야 할 항목
- 기업과 경쟁사들의 핵심 투자지표 ( PER, ForwardPER, PEG Ratio, EPS 성장률 ) 비교표
- 핵심 투자지표를 바탕으로 부정적인 투자 의견과 긍정적인 투자 의견 제시
- 시장 점유율

재무 건전성 분석에 포함되어야 할 항목
- 기업과 경쟁사들의 재무 건전성 분석 및 이를 요약한 표


## enum 예시

```
앞으로 enum 에 대해서 물어보면 아래 예시와 같이 답변해주세요

단, 답변을 할 때, 굵게 인라인 코드 등은 전부 빼주세요.
그리고 타입입니다 -> 타입이다. 사용됩니다 -> 사용된다. 제어합니다 -> 제어한다 형태로 바꿔주세요

[예시]

## enum D3D12_HEAP_TYPE
memory heap 의 유형을 정의하는 enum 이다.

정의는 다음과 같다.

cpp
typedef enum D3D12_HEAP_TYPE
{
    D3D12_HEAP_TYPE_DEFAULT   = 1,
    D3D12_HEAP_TYPE_UPLOAD    = 2,
    D3D12_HEAP_TYPE_READBACK  = 3,
    D3D12_HEAP_TYPE_CUSTOM    = 4
} D3D12_HEAP_TYPE;


각 enum 의 의미는 다음과 같다.

* D3D12_HEAP_TYPE_DEFAULT
  * CPU 에서 접근이 제한된다.
  * GPU 에서 리소스를 사용하는 용도에 따라 데이터를 쓰는 상태와 읽는 상태를 바꾸는 작업을 하며 전환작업 중에 발생할 수 있는 데이터 충돌을 방지하기 위해 resource transition barrier 가 사용된다.
  * cpu 에 있는 resource 를 upload heap 에 있는 resource 에 복사하고 이를 default heap 에 있는 resource 에 복사하여 결론적으로 CPU 에 있는 resource 를 GPU 로 복사할 수 있다.
* D3D12_HEAP_TYPE_UPLOAD
  * GPU 에 업로드하는 데 최적화된 CPU 액세스를 제공한다.
  * 그러나 GPU 의 최대 대역폭을 사용하지 않는다.  
  * CPU 에서 쓰기가 가능하며, GPU 에서 읽기 작업에 사용된다.
  * 이 힙에 생성되는 resource 는 D3D12_RESOURCE_STATE_GENERIC_READ로 생성해야 하며 이 heap 내에서 변경될 수 없다. 
* D3D12_HEAP_TYPE_READBACK
  * GPU에서 CPU로 데이터를 전송하기 위한 리소스가 할당된다.
  * GPU에서 쓰기가 가능하며, CPU에서 읽기 작업에 사용된다.
* D3D12_HEAP_TYPE_CUSTOM
  * 사용자 정의 메모리 특성을 가진 힙이다.
  * 힙의 세부적인 동작을 사용자 정의할 수 있다.
```

## struct 예시

```
앞으로 구조체에 대해서 물어보면 아래 예시와 같이 답변해주세요

단, 답변을 할 때, 굵게 인라인 코드 등은 전부 빼주세요.
그리고 타입입니다 -> 타입이다. 사용됩니다 -> 사용된다. 제어합니다 -> 제어한다 형태로 바꿔주세요

[예시]

## D3D12_HEAP_PROPERTIES 구조체  
Resource 할당을 위한 Heap 의 속성을 정의하는 구조체이다. 

구조체의 정의는 다음과 같다.

cpp
typedef struct D3D12_HEAP_PROPERTIES {
    D3D12_HEAP_TYPE Type;
    D3D12_CPU_PAGE_PROPERTY CPUPageProperty;
    D3D12_MEMORY_POOL MemoryPoolPreference;
    UINT CreationNodeMask;
    UINT VisibleNodeMask;
} D3D12_HEAP_PROPERTIES;


각 멤버 변수는 다음과 같다.

* D3D12_HEAP_TYPE Type  
  * 힙의 유형을 지정한다.
  * 힙 유형에 따라 메모리 접근 방식이 결정된다. 

* D3D12_CPU_PAGE_PROPERTY CPUPageProperty  
  * 힙의 CPU 페이지 속성을 지정한다.
  * 이 값은 D3D12_HEAP_TYPE이 CUSTOM으로 설정된 경우에만 유효하며, 다른 값으로 설정된 경우에는 D3D12_CPU_PAGE_PROPERTY_UNKNOWN이 기본적으로 사용된다. 

* D3D12_MEMORY_POOL MemoryPoolPreference  
  * 힙이 할당될 메모리 풀의 우선순위를 지정한다. 

* UINT CreationNodeMask  
  * 멀티 GPU 시스템에서 heap 이 생성될 노드를 지정하는 마스크 값이다.
  * 단일 GPU 환경에서는 1로 설정되며, 멀티 GPU 환경에서 특정 GPU에 할당할 수 있다.

* UINT VisibleNodeMask  
  * 힙을 볼 수 있는 노드를 지정하는 마스크 값이다.
  * 멀티 GPU 시스템에서 특정 GPU가 heap 에 접근할 수 있도록 설정한다.
```

## Function 예시
```
앞으로 함수에 대해서 물어보면 아래 예시와 같이 답변해주세요

단, 답변을 할 때, 굵게 인라인 코드 등은 전부 빼주세요.
그리고 타입입니다 -> 타입이다. 사용됩니다 -> 사용된다. 제어합니다 -> 제어한다 형태로 바꿔주세요

[예시]

## ID3D12Device::CreateCommandAllocator 함수
command list 을 저장하는 데 사용할 Command Allocator를 생성하는 함수다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT CreateCommandAllocator(
  [in]  D3D12_COMMAND_LIST_TYPE type,
        REFIID                  riid,
  [out] void                    **ppCommandAllocator
);
```

인자는 다음과 같다.
* D3D12_COMMAND_LIST_TYPE type
  * 생성할 커맨드 할로케이터의 유형을 지정한다. 
  * enum D3D12_COMMAND_LIST_TYPE 을 사용한다.

* REFIID riid
  * 요청하는 인터페이스의 식별자이다. 

* void **ppCommandAllocator
  * 성공 시, 생성된 ID3D12CommandAllocator 객체의 포인터를 받을 포인터다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 실패 시 오류 코드를 통해 생성 실패 원인을 진단할 수 있다.
```
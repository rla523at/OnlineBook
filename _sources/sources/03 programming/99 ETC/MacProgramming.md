# Protocol
프로토콜(protocol)이란 **메서드와 속성의 요구사항을 정리한 일종의 ‘계약’**이다.
즉, 어떤 역할을 수행하기 위해 반드시 구현해야 할 메서드 목록을 미리 정의해 놓은 것이다.

이 때, 프로토콜 안에서 @required(기본값) 로 선언된 프로퍼티는 반드시 구현해야 하며 @optional 아래 선언된 프로퍼티는 구현하지 않아도 된다.

클래스가 프로토콜을 **adopt**(채택)한다는 것은

1. 그 프로토콜이 정의한 **필수 메서드**를 해당 클래스에서 구현하겠다고 선언하는 것
2. 선언한 이후에는 실제로 그 메서드들을 구현해서 프로토콜의 “계약”을 지키는 것

예를 들어 Objective‑C에서는 클래스 인터페이스 선언부에 꺾쇠(<>) 안에 채택할 프로토콜 이름을 적는다:

```objc
@interface MyLoader : NSObject <AVAssetResourceLoaderDelegate>
@end
```

이렇게 선언하면 컴파일러는 “AVAssetResourceLoaderDelegate 프로토콜의 필수 메서드를 반드시 구현해야 한다”고 체크하고,
리소스 로더는 이 객체를 delegate로 지정했을 때 메시지를 보낼 수 있게 된다.

# AVFoundation

## AVAsset & AVAssetTrack
AVAsset은 내부적으로 하나 이상의 AVAssetTrack 객체를 포함하는 “컨테이너” 역할을 합니다.

AVAsset 은 미디어 파일 전체를 추상화한 객체이고 AVAssetTrack 은 AVAsset 내의 **개별** 미디어 트랙(비디오·오디오·자막 등)을 나타내는 객체이다.  

# Binary 데이터로 AVAsset 만들기
AVURLAsset 객체가 resource loading 을 해야 될 때 AVAssetResourceLoader 객체( loader 객체 )에게 도움을 요청한다. loader 객체는 표준 Scheme ( http://, https://, file:// ) 을 쓰면, AVAssetResourceLoader는 delegate 호출 없이 내부의 URL 로딩 시스템(CFNetwork/NSURLSession 기반)을 사용한다. 하지만 표준 Scheme 이 아닌 Custom Scheme 을 사용하게 되면 resource loading 에 필요한 정보를 AVAssetREsourceLoadingRequest 객체 ( request 객체 )에 담아 AVAssetResourceLoaderDelegate Protocol 을 따르는 객체( delegate 객체 )에 shouldWaitForLoadingOfRequestedResource method 를 호출해서 request 객체를 전달한다. delegate 객체는 shouldWaitForLoadingOfRequestedResource method 에서 request 객체에 정보들을 채워넣고 다 채워지면 AVAssetResourceLoadingRequest 의 finishLoading method 를 호출해 request 객체에 필요한 정보가 다 채워졌음을 표시한다. loader 객체는 delegate 객체가 채워넣었다는 신호를 보낼때 까지 대기하다가 신호가 오면 delegate 가 채워준 정보를 바탕으로 resource loading 을 진행한다.
* https://developer.apple.com/documentation/avfoundation/avassetresourceloadingrequest?language=objc

## AVAssetResourceLoaderDelegate
Resource loading 이 필요할 떄 동작을 overriding 하기 위해서는 AVAssetResourceLoaderDelegate Protocol 의 shouldWaitForLoadingOfRequestedResource method 를 재정의 해야한다. 

request 객체에 contentInformationRequest getter 의 반환값이 nil 이 아닌경우, dataRequest getter 의 반환값도 nil 이 아니다. 하지만 dataRequest 의 반환값은 실제로 사용되지 않으며, 사용할 경우 문제를 야기할 수 있다.
* It’s crucial to note here that content info requests are always accompanied by a data request for the first two bytes of the file. The actual received bytes are not used by the resource loader.
* Warning: do not pass the two requested bytes of data to the loading request’s dataRequest. This will lead to an undocumented bug where no further loading requests will be made, stalling playback indefinitely.

로딩이 완료되면 AVAssetResourceLoadingRequest 의 finishLoading method 를 호출해줘야 한다. 그렇지 않으면 loader 객체가 무한히 기다리게 된다.

```objc
- (BOOL)resourceLoader:(AVAssetResourceLoader *)resourceLoader
shouldWaitForLoadingOfRequestedResource:(AVAssetResourceLoadingRequest *)loadingRequest
{
    AVAssetResourceLoadingContentInformationRequest* infoReq = loadingRequest.contentInformationRequest;
    if (infoReq != nil) {
        infoReq.contentType = _contentType;
        infoReq.contentLength = _data.length;
        infoReq.byteRangeAccessSupported = YES;
        
        [loadingRequest finishLoading];
        return YES;
    }
    
    AVAssetResourceLoadingDataRequest* dataReq = loadingRequest.dataRequest;
    if (dataReq != nil) {
        NSInteger start = (NSInteger)dataReq.requestedOffset;
        NSInteger len   = dataReq.requestedLength;
        NSInteger end   = MIN(start + len, _data.length);
        NSData *chunk   = [_data subdataWithRange:NSMakeRange(start, end - start)];
        [dataReq respondWithData:chunk];

        [loadingRequest finishLoading];
        return YES;
    }
    
    // 다른 종류의 요청은 지원하지 않으면 NO
    return NO;
}
```

* https://jaredsinclair.com/2016/09/03/implementing-avassetresourceload.html?utm_source=chatgpt.com

## ETC
직접 정의한 AVAssetResourceLoaderDelegate 로 Resource Loading 을 진행하기 위해서는 AVURLAsset 을 만들 떄, custom URL 을 제공해야 한다.

커스텀 URL 스킴(Custom URL Scheme)이란
   * 보통의 웹 요청은 `http://…` 또는 `https://…` 스킴을 쓴다.
   * 하지만 키 서버 요청만을 구분하기 위해 `skd://` 또는 `myappkey://` 같이 개발자가 직접 정의한 프로토콜 이름을 URL 앞에 붙인다.
   * 이렇게 하면 시스템 입장에서는 “알 수 없는 스킴”이 들어왔다고 보고, AVAssetResourceLoaderDelegate에게 처리 기회를 제공하게 된다.

custom URL 을 제공하였을 때, 호출 흐름은
   1. AVURLAsset이 플레이리스트(HLS) 혹은 샘플 버퍼 내에서 `skd://key-id-1234` 같은 URL을 만난다
   2. 내부의 AVAssetResourceLoader가 “이 스킴은 HTTP가 아니니 도와 달라”고 판단하고 delegate의 `resourceLoader:shouldWaitForLoadingOfRequestedResource:`를 호출


* https://github.com/xybp888/iOS-SDKs/blob/master/iPhoneOS13.0.sdk/System/Library/Frameworks/AVFoundation.framework/Headers/AVAssetResourceLoader.h


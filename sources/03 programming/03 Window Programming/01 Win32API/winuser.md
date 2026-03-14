# Winuser
Winuser.h 는 Windows API에서 사용자 인터페이스 관련 기능을 정의하는 헤더 파일이다.

> Reference  
> [learn.microsoft - api/winuser](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/)

## ShowWindow
ShowWindow 함수는 지정된 창을 화면에 표시하거나 숨기는 기능을 제공하는 함수이다. 

함수의 시그니처는 다음과 같다.
```cpp
BOOL ShowWindow(
  [in] HWND hWnd,
  [in] int  nCmdShow
);
```

인자는 다음과 같다.
* HWND hWnd
  * 표시하거나 숨길 window 의 핸들이다. 

* int nCmdShow
  * 창의 표시 상태를 지정하는 값이다.
  * 사용 가능한 값은 [learn.microsoft - nf-winuser-showwindow](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-showwindow) 참고

반환값은 다음과 같다.
* 성공 시
  * 창이 이전에 표시 상태였으면 `TRUE`를 반환한다.

* 실패 시
  * 창이 이전에 숨겨져 있던 상태였으면 `FALSE`를 반환한다.

> Reference  
> [learn.microsoft - nf-winuser-showwindow](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-showwindow)

## UpdateWindow
UpdateWindow 함수는 지정된 window 의 클라이언트 영역을 즉시 다시 그리도록 요청하는 함수다. 

이 함수는 window 의 유효하지 않은 영역이 있을 때 WM_PAINT 메시지를 보내지 않고 강제로 다시 그리기 작업을 수행한다. 

함수의 시그니처는 다음과 같다.
```cpp
BOOL UpdateWindow(
  HWND hWnd
);
```

인자는 다음과 같다.
* HWND hWnd
  * 업데이트할 창의 핸들이다. 

반환값은 다음과 같다.
* 성공 시
  * `TRUE`를 반환한다.

* 실패 시
  * `FALSE`를 반환한다.
  * 다시 그리기 작업이 실패할 경우 GetLastError()를 호출하여 오류 원인을 확인할 수 있다.


> Reference  
> [learn.microsoft - nf-winuser-updatewindow](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-updatewindow)  
> [blog.naver.com/tipsware - update 함수에 대하여](https://blog.naver.com/tipsware/221697472368)  

# Cmake

## Download

[홈페이지](https://cmake.org/download/)에 들어가서 platform에 맞는 Binary distribution을 다운받고 실행시키면 설치가 완료된다.

## CMakeLists.txt
기본구조는 다음과 같다.
```
cmake_minimum_required(VERSION 3.11)		# CMake 프로그램의 최소 버전

project(									# 프로젝트 정보
  	CMakeTest                       		# 프로젝트 이름(필수)
  	VERSION 0.0                     
  	DESCRIPTION "예제 프로젝트"      
  	LANGUAGES CXX
)

add_executable(											# 실행 파일 생성 명령어
  	program             								# 실행 파일 이름
  	main.cpp            								# 소스1
  	foo.cpp             								# 소스2  
) 

target_compile_options(								# 컴파일러 옵션 설정
  	program                 						# 실행 파일 이름
  	PUBLIC                  						#
  	/std:c++17              						# 언어표준
	/W4                   							# 경고수준
)
```

주석은 `#`로 처리한다.

실행파일을 만들때는 `add_excutable`명렁어를 사용한다.

CMake에서 target이란 프로그램을 구성하는 요소(실행파일, 라이브러리)를 뜻한다.

CMake의 모든 명령어들은 target을 기준으로 돌아간다.

Target에는 속성(property)를 정의할 수 있다.

Compile option을 주는 것도 target에 compile option이라는 property를 주는 것이다.

CMake는 target을 정의하고(add_excutable 같은 명령어를 통해), 해당 target들의 property를 지정하는 명령어(target_compile_options)들로 이루어져 있다.


### CMake (camke-gui)
1. CMake (camke-gui)를 실행한다.
Where is the source code에 CMakeLists.txt가 있는 폴더를 설정한다.
2. Where to build the binaries에 CMakeLists.txt가 있는 폴더에 bin폴더를 만들어주고 거기로 설정한다.
3. Configure 
4. Generate
5. bin 폴더에 들어가 `프로젝트이름.sln`(CMakeTest.sln)을 원하는 configuration으로 빌드한다.


> Reference
> [모두의코드](https://modoocode.com/332)
> [MSVC target compile option](https://learn.microsoft.com/ko-kr/cpp/build/reference/compiler-options-listed-by-category?view=msvc-170)

### 빌드 결과 생성 경로 지정하기

```
set_target_properties(
	mssel														# Target 이름
	PROPERTIES
		ARCHIVE_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/lib"	# property - value
)
```
> Reference  
> [blog.오늘도 야근](https://tttsss77.tistory.com/80)

## CMake Message

### Example
```
message("src:	CMAKE_CURRENT_SOURCE_DIR	= ${CMAKE_CURRENT_SOURCE_DIR}")
message("src:	CMAKE_CURRENT_BINARY_DIR	= ${CMAKE_CURRENT_BINARY_DIR}")
```

> Reference  
[CMake](https://cmake.org/cmake/help/latest/command/message.html)


## CMake Variable

### 기본
CMake에서는 모든 변수를 문자열로 취급한다. 

#### 사용법
myVar이라는 변수가 있다고 하자.

변수에 저장된 값은 `${myVar}`을 사용하여서 얻을 수 있고, 문자열이나 변수가 필요한 곳이면 어디에서나 사용할 수 있다.

> Reference  
> [Blog.별준코딩](https://junstar92.tistory.com/206)

#### Scope

> Refernece  
> [Blog.별준코딩](https://junstar92.tistory.com/211)

### CMake 제공 변수 목록
[CMake.variables](https://cmake.org/cmake/help/latest/manual/cmake-variables.7.html?)

#### CMAKE_SOURCE_DIR 
가장 최상단의 source tree의 경로 즉, 최상단에 위차한 CMakeLists.txt의 경로이다.

#### CMAKE_BINARY_DIR 
가장 최상단의 build tree의 경로로 ${CMAKE_SOURCE_DIR}/bin이다.

#### CMAKE_CURRENT_SOURCE_DIR 
CMake에 의해서 현재 처리 중인 CMakeLists.txt가 존재하는 경로이다. 

이는 add_subdirectory()로 새로운 CMakeLists.txt가 수행될 때 업데이트되며 수행 중인 CMakeLists.txt가 종료되면 다시 이전 경로로 돌아가게 된다.

#### CMAKE_CURRENT_BINARY_DIR 
현재 수행 중인 CMakeLists.txt에 대응되는 빌드 디렉토리의 경로이다. 이 경로 역시 add_subdirectory()가 호출되어 새로운 CMakeLists.txt가 수행될 때 업데이트되며, 종료될 때 다시 이전 경로로 복구된다.

## CMake Property
### 목록
[CMake.property](https://cmake.org/cmake/help/latest/manual/cmake-properties.7.html?)

#### 참고
##### target_compile_option vs CXX_STANDARD
Build Target을 작성할때 작성자는 언제나 CXX_STANDARD를 명시합니다. 이는 target_compile_options함수로 /std:c++latest혹은 gnu++2a를 추가하지 않아도 자동으로 추가하도록 해줍니다. 이 Property의 최대 값은 CMake 버전에 따라서 결정됩니다.

> Reference  
> [Blog](https://gist.github.com/luncliff/6e2d4eb7ca29a0afd5b592f72b80cb5c)

##### `_<CONFIG>`
Property 중에 `_<CONFIG>`라고 되어 있는 부분은 DEBUG 혹은 RELEASE로 바꿔주면 된다.

문자열 내부에서 사용하고 싶은 경우 `$<CONFIG>`를 사용하면 된다.

###### Example
```
	ARCHIVE_OUTPUT_DIRECTORY_DEBUG		"${CMAKE_SOURCE_DIR}/lib"
	ARCHIVE_OUTPUT_NAME_DEBUG			"mssel_$<CONFIG>"
	ARCHIVE_OUTPUT_DIRECTORY_RELEASE	"${CMAKE_SOURCE_DIR}/lib"
	ARCHIVE_OUTPUT_NAME_RELEASE			"mssel_$<CONFIG>"
```

> Reference  
> [stackoverflow](https://stackoverflow.com/questions/68920524/cmake-library-outdir)

### get_target_property
기본 형태는 다음과 같다.
```
get_target_property(<VAR> target property)
```



#### Example
target의 이름이 mssel이라고 하자.
이 떄, target의 ARCHIVE_OUTPUT_DIRECTORY라는 property를 lib_dir이라는 변수로 가져오는 코드는 다음과 같다.
```
get_target_property(lib_dir mssel ARCHIVE_OUTPUT_DIRECTORY)
```

> Reference  
> [CMake](https://cmake.org/cmake/help/latest/command/get_target_property.html)


## 도움이 되는 문서
> [blog.모두코드](https://modoocode.com/332)  
> [blog.별준코딩](https://junstar92.tistory.com/category/CMake?page=1)  
> [blog - Cmake를 이용한 라이브러리](https://tttsss77.tistory.com/219)  
> [cgold](https://cgold.readthedocs.io/en/latest/overview/cmake-can.html)  
> [cmake 도움 문서](https://gist.github.com/luncliff/6e2d4eb7ca29a0afd5b592f72b80cb5c?permalink_comment_id=2831356)  
> [blog - cmake로 다른 library 빌드하는 과정](https://luckygg.tistory.com/376)  

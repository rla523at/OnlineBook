# STD



## source_location

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/utility/source_location)

## stacktrace

> Reference
> [cppreference](https://en.cppreference.com/w/cpp/header/stacktrace)  
> [blog](https://www.sandordargo.com/blog/2022/09/21/cpp23-stacktrace-library)  

## filesystem
```cpp
#include <iostream>$$
#include <filesystem>

int main() {
	std::filesystem::path p("RSC");
	std::cout << "current  path  : " << std::filesystem::current_path() << "\n";
	std::cout << "relative path  : " << p.relative_path() << "\n";
	std::cout << "absolute path  : " << std::filesystem::absolute(p) << "\n";
	std::cout << "canonical path : " << std::filesystem::canonical(p) << "\n";
}
// current  path  : "C:\\Users\\KimMinSeok\\source\\repos\\MS_Test\\GoogleTest"
// relative path  : "RSC"
// absolute path  : "C:\\Users\\KimMinSeok\\source\\repos\\MS_Test\\GoogleTest\\RSC"
// canonical path : "C:\\Users\\KimMinSeok\\source\\repos\\MS_Test\\GoogleTest\\RSC"
```

## Chrono
```cpp
int main() {
	std::vector<size_t> vec;

	const auto time_point1 = std::chrono::steady_clock::now();
	for (size_t i = 0; i < 10000000; ++i)
		vec.resize(i, i);
	const auto time_point2 = std::chrono::steady_clock::now();

	std::chrono::nanoseconds nanosec = time_point2 - time_point1;

	//기본 시간 단위는 암시적 형변환 안됨
	//std::chrono::microseconds microsec = time_point2 - time_point1;	//signed integer 55bits
	//std::chrono::milliseconds milisec = time_point2 - time_point1;	//signed integer 45bits
	//std::chrono::seconds sec = time_point2 - time_point1;				//signed integer 35bits

	//기본 시간 단위는 duration_cast로 형변환 해야됨
	const auto microsec = std::chrono::duration_cast<std::chrono::microseconds>(nanosec);	
	const auto millisec = std::chrono::duration_cast<std::chrono::milliseconds>(nanosec);	
	const auto sec		= std::chrono::duration_cast<std::chrono::seconds>(nanosec);		

	std::cout << "nanosec: \t" << nanosec.count() << "\n";
	std::cout << "microsec: \t" << microsec.count() << "\n";
	std::cout << "milisec: \t" << millisec.count() << "\n";
	std::cout << "sec: \t\t" << sec.count() << "\n";

	//사용자 정의 시간 단위는 암시적 형변한 됨
	std::chrono::duration<double, std::micro>	d_microsec	= time_point2 - time_point1;
	std::chrono::duration<double, std::milli>	d_millisec	= time_point2 - time_point1;
	std::chrono::duration<double>				d_sec		= time_point2 - time_point1;

	std::cout << "d_microsec: \t" << d_microsec.count() << "\n";
	std::cout << "d_milisec: \t" << d_millisec.count() << "\n";
	std::cout << "d_sec: \t\t" << d_sec.count() << "\n";
}
```

## getline
```cpp
#include <iostream>
#include <string>

int main() 
{
	std::string input_string;
	while (std::getline(std::cin, input_string))
	{		
		std::cout << input_string << "\n";
	}
	//Window에서 ctrl + z 를 입력하면 EOF가 입력됨.
	//이 경우 static_cast<bool>(std::cin)는 false가 됨
	return 0;
}
```

## cin
```cpp
#include <iostream>
#include <string>

int main() 
{
	size_t num = 0;
	std::cin >> num;	
}
```

### 주의사항1
모든 공백문자 (띄어쓰기나 엔터, 탭 등)을 입력시에 분리해서 입력받는다.

아래 예시 코드를 보자.

```cpp
#include <string>
#include <iostream>

using namespace std;

int main(void)
{	
	string str;
	cin >> str;
  	cout << str << endl;
  	cin >> str;
  	cout << str << endl;
  	cin >> str;
  	cout << str << endl;
}
```

입력이 `abc`인 경우, `abc`를 출력하고 두번 더 input을 받을 준비를 한다. 하지만 입력이 `a b c`인 경우, 각 줄에 a,b,c를 출력하고 끝난다. 왜냐하면 `a`,`b`,`c`를 각각 입력받아 총 3번 입력이 되었다고 인식하기 때문이다.

### 주의사항2
cin과 getline을 섞어 쓰면 예상하지 못한 버그가 발생할 수 있다.

아래 예시 코드를 보자
```cpp
#include <iostream>
#include <string>

int main()
{
	std::cout << "enter number : ";
	int number = 0;
	std::cin >> number;	
	//std::cin>>는 변수에 '\n'을 담지 않는다.
	//std::cin의 buffer에는 '\n'이 남아있음.
		
	std::cout << "enter name : ";
	std::string name;
	std::getline(std::cin, name);
	//새로 입력받지 않고 std::cin buffer에 남아있는 '\n'을 name에 할당함.
	
	std::cout << "\ninput: \n";
	std::cout << number << " " << name;

	return 0;
}
```

`std::cin >> number;`을 실행하고 나면 std::cin의 buffer에는 '\n'이 남아있다.

따라서 `std::getline(std::cin, name);`를 실행할 떄, 새로 입력받지 않고 std::cin buffer에 남아있는 '\n'을 name에 할당한다.

이와 같이 cin과 getline을 섞어 쓰면 버그가 발생한다.

cin은 담는 변수가 std::string이더라도 '\n'을 담지 않는다.
```cpp
#include <iostream>
#include <string>

int main()
{
	std::cout << "enter name : ";
	std::string name;
	std::cin >> name;
	//std::cin>>는 변수에 '\n'을 담지 않는다.
	//std::cin의 buffer에는 '\n'이 남아있음.

	std::cout << "enter family name : ";
	std::string family_name;
	std::getline(std::cin, family_name);
	//똑같은 문제 발생

	std::cout << "\ninput: \n";
	std::cout << name << " " << family_name;

	return 0;
}
```

## iomanip
```cpp
int main(){
	size_t b = 1;	
    std::cout << std::setw(9) << b  //기본으로 std::right
	std::cout << std::left;         //한번 설정하면 계속 유지
	std::cout << std::setw(9) << b << b << b << "\n"; //setw는 바로 다음 입력에만 영향을 줌	

    double d = 1.2345;
    std::fixed << std::setprecision(2) // 2자리 고정소수점으로 출력, 한번 설정으로 계속 유지
    std::cout << d << "\n";
	std::cout << std::defaultfloat << std::setprecision(6); //initial precision
	std::cout << d << "\n";
}
```

## Queue
FIFO(First in first out) container.

pop시 앞의 메모리를 낭비하거나, 모든 데이터를 옮기는 경우를 방지하기 위해 원형 큐를 사용해서 구현

## Set

### 멤버 함수
#### insert
``` cpp
std::set<size_t> s;
s.insert(3);
s.insert(4);
s.insert(3);    // nothing happen!
```

#### erase
```cpp
std::set<size_t> s = {1, 3};

const auto result1 = s.erase(2); // fail erase return size_t 0
const auto result2 = s.erase(3); // success erase return size_t 1
```
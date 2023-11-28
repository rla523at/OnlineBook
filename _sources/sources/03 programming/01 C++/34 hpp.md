# hpp
hpp를 이용해서 Template class의 정의와 구현부를 분리하게 되면, compiletime 의존성을 줄일 수 있다.

다음과 같은 예제코드에서는 `A<T>::function()`의 내용이 수정되도 Source2.cpp만 재컴파일 된다.

```cpp
//Header.h
#pragma once

template <typename T>
class A
{
public:
	void function(void);
};

//Header.hpp
#include "Header.h"

template<typename T>
void A<T>::function()
{
	//change!
};

//Header2.h
#pragma once
#include "Header.h"

class B
{	
public:
	void f(void);

private:
	A<int> _a;
};

//Source2.cpp
#include "Header2.h"

#include "Header.hpp"

void B::f(void)
{
	_a.function();
}

//Header3.h
#pragma once
#include "Header2.h"

class C : public B
{
public:
	void f2(void);
};

//Source3.cpp
#include "Header3.h"

void C::f2(void)
{
	this->f();
}

```
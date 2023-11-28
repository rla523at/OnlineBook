# Enum

## enum
아래 예시 코드를 보자.

```cpp
enum A
{
	Start
};

int main()
{
	auto val = Start;
	return 0;
}
```

enum의 경우 A라는 scope를 명시하지 않아도 된다.

따라서, 다음과 같은 경우 컴파일 Error가 발생한다.

```cpp
enum A
{
	Start
};

enum B
{
	Start
};

int main()
{
	auto val = Start; // Compile Error!
	return 0;
}
```

### 형변환
아래 예시 코드를 보자.

``` cpp
enum A
{
	Start
};

int main()
{
	int val = Start;
	return 0;
}

```

enum의 경우 int로 암시적 형변환이 가능하다.


## enum class
아래 예시 코드를 보자.
```cpp
enum class A
{
	Start
};

int main()
{
	auto val = Start;	//Error!
	return 0;
}
```

A가 enum class이기 때문에 A::Start 변수가 정의되어 있을 뿐, 더이상 Start라고 정의된 변수는 없다. 

즉, enum class의 경우 항상 scope를 명시해야 된다.

### 형변환
아래 예시 코드를 보자.
```cpp
enum class A
{
	Start
};

int main()
{
	//int val = A::Start; // Error!
	int val = static_cast<int>(A::Start);
	return 0;
}
```
enum class의 경우 암시적 형변환이 불가능하고 명시적 형변환만 가능하다.

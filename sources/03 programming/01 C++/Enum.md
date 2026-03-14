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

### Underlying Type
enum 은 colon(:) 뒤에 type 을 작성하여 underlying type 을 결정할 수 있다.

```cpp
enum A : unsigned char
{
	Start
};
```

만약 명시적으로 underlying type을 작성하지 않은 경우 int 형이 된다.

### 형변환
아래 예시 코드를 보자.

``` cpp
enum A
{
	Start = 1,
};

int main()
{
	int val = Start;
	return 0;
}
```

enum의 경우 int로 암시적 형변환이 가능하다.

그리고 enum 에서 정의 되지 않은 underlying type 의 변수 또한 enum 으로 형변환 될 수 있으며 데이터는 손실 되지 않는다. 즉, 다시 underlying type 으로 변환하면 그대로 원래 값이 나온다.

```cpp
enum A
{
	Start = 1,
};

int main()
{
	A enumA = static_cast<A>(10);
	int val = static_cast<int>(enumA); 
	return 0;
}
```


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

그리고 enum class 에서 정의 되지 않은 underlying type 의 변수 또한 enum class 로 형변환 될 수 있으며 데이터는 손실 되지 않는다. 즉, 다시 underlying type 으로 변환하면 그대로 원래 값이 나온다.

```cpp
enum class A
{
	Start = 1,
};

int main()
{
	A enumA = static_cast<A>(10);
	int val = static_cast<int>(enumA); 
	return 0;
}
```

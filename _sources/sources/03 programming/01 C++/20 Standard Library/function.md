# function
멤버 함수는 인자로 클래스의 객체를 암시적으로 받고 있다.

따라서, 멤버 함수에 대한 function 객체를 생성할 때는 이를 고려해야 한다.

```cpp
class A
{
public:
  void f(void) { std::cout << "f";}
  void constf(void) const { std::cout << "constf";}
}

void g(void) { std::cout << "g";}

int main(void){
  std::function<void(A&)> test1 = &A::f;
  std::function<void(const A&)> test2 = &A::constf;
  std::function<void(void)> test3 = &g;
}
```

test1의 경우 `std::function<void(const A&)> test1 = &A::f;` 면 컴파일 오류가 발생한다. 왜냐하면 함수내에서 멤버변수를 바꿀 수 있기 때문에 const 객체가 들어올 수 없다. 하지만 test2의 경우 `std::function<void(A&)> test2 = &A::constf;` 여도 정상적으로 컴파일 된다.

그리고 멤버함수의 경우 mem_fn 을 이용해서 function 객체를 생성할 수 있다.

```cpp
auto test4 = std::mem_fn(&A::f);
```

> Reference   
> [cppreference - mem_fn](https://en.cppreference.com/w/cpp/utility/functional/mem_fn)  
> [모두의코드 - 씹어먹는 C ++ - <14. 함수를 객체로! (C++ std::function, std::mem_fn, std::bind)>](https://modoocode.com/254)  

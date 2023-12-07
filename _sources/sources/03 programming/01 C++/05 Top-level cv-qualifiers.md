# Top-level cv-qualifiers

`T* u`의 타입은 pointer to T이다. 이 때, 타입은 2개의 계층으로 이루어져 있으며 첫번째 계층은 pointer 그리고 두번째 계층은 T이다.

그리고 C++에서는 cv-qualifier가 다른 계층에 각각 나타날 수 있다. 

예를 들어 `const T* u`의 타입은 pointer to const T이다. 즉, 여기서는 두번째 계층에 cv-qualifier가 나타난 경우이다.

그리고 reference는 top-level cv-qualifiers를 갖지 않는다.([standard](https://eel.is/c++draft/basic.type.qualifier), [cppreference](https://en.cppreference.com/w/cpp/language/reference))

> Reference  
> [Dan saks - Top-Level cv-Qualifiers in Function Parameters](http://www.dansaks.com/articles/2000-02%20Top-Level%20cv-Qualifiers%20in%20Function%20Parameters.pdf)  
> [stackoverflow - where-is-the-definition-of-top-level-cv-qualifiers-in-the-c11-standard](https://stackoverflow.com/questions/24676824/where-is-the-definition-of-top-level-cv-qualifiers-in-the-c11-standard)  
> [blog - references-dont-have-top-level-cv-qualifiers](https://blog.knatten.org/2023/03/17/references-dont-have-top-level-cv-qualifiers/)
> [standard - basic.type.qualifier](https://eel.is/c++draft/basic.type.qualifier)  
> [cpprefenrece - reference](https://en.cppreference.com/w/cpp/language/reference)
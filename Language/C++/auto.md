# auto

## auto in C

  자동 변수라는 의미로, 변수가 생성된 뒤 자동으로 사라집니다.  
  일반적으로 main 또는 함수 내부에서 선언한 모든 변수가 auto 변수입니다.

- 예제 코드 

```c
#include <stdio.h>
int main(){
  auto int num1 = 10; // int num1 = 10 과 같은 의미
  printf("%d\n", num1);
  return 0;
}
```  

## auto in C++:

  Before :   
  멤버 포인터, 함수 포인터, 템플릿 형식을 초기화 표현식 으로 선언할 경우 작성이 어렵거나 오타의 가능성이 높다.

- 예제 코드
```c
class Math
{
public:
  int Sum(int left, int right)
  {
    return left + right;
  }
};

int Sum(int a, int b)
{
  return a+b;
}

int main(){
  // 함수 포인터의 타입
  int (*SumFunc1)(int, int) = Sum;
  
  // 멤버 함수 포인터 타입
  Math math;
  int (Math:: * MathSum1)(int, int) = &Math::Sum;
  (math.*MathSum1)(5, 5);
  
  std::map<int, int> mapContainer;
  // 템플릿 형식의 타입
  std::map<int, int>::iterator it1 = mapContainer.begin();
}
```
  After :  
  Auto를 사용하여 변수 선언 및 초기화를 하면 코드 작성 및 실수 방지에 유리하다.

- 예제 코드
```c
int main(){
  // 함수 포인터의 타입을 auto를 사용
  atuo *SumFunc1 = Sum;
  
  // 멤버 함수 포인터 타입
  Math math;
  auto * MathSum1 = &Math::Sum;
  (math.*MathSum1)(5, 5);
  
  std::map<int, int> mapContainer;
  // 템플릿 형식의 타입을 auto를 사용
  auto it1 = mapContainer.begin();
}
```

  컴파일러가 타입을 정확히 알아낼 수 있는 경우 auto로 표현할 수 있다.
  - 예제 코드(출처 : https://modoocode.com/222)
  ```c
#include <iostream>
#include <typeinfo>

int sum(int a, int b) { return a + b; }

class SomeClass {
  int data;

 public:
  SomeClass(int d) : data(d) {}
  SomeClass(const SomeClass& s) : data(s.data) {}
};

int main() {
  auto c = sum(1, 2);  // sum의 반환형이 int이므로 추정 가능
  auto num = 1.0 + 2.0;  // double 간의 연산이므로 추정 가능

  SomeClass some(10);
  auto some2 = some;

  auto some3(10);  //10은 int이기 때문에 컴파일러 입장에서는 int로 추정

  std::cout << "c 의 타입은? :: " << typeid(c).name() << std::endl;
  std::cout << "num 의 타입은? :: " << typeid(num).name() << std::endl;
  std::cout << "some2 의 타입은? :: " << typeid(some2).name() << std::endl;
  std::cout << "some3 의 타입은? :: " << typeid(some3).name() << std::endl;
}
  ```

  매개변수 불가, 구조체, 클래스 멤버변수 불가
  : 함수나 구조체, 클래스 등은 컴파일러가 애당초 필요한 메모리가 얼마나 되는지 미리 알고 할당해야 합니다. 따라서 auto를 쓰게 되면 적절한 공간 계산이 불가하기 때문에 이와 같이는 사용할 수 없습니다.

- 예제 코드
```c
void add(auto a, auto b){}
struct{
  auto b;
}
class Person{
  auto name;
}
```

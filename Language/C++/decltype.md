# decltype

## Before : 
  템플릿으로 auto 반환형 함수에 사용할 경우, 후행반환에 어떤 자료형을 표시해야 하는지 모호할 수 있다.

  - 예제 코드
```c++
templae <typename T1, typename T2>
auto add(T1 t1, T2 t2) {
  return t1 + t2;
}
```

템플릿으로 일반화 했기 때문에 t1과 t2의 자료형이 어떻게 될 지 모른다.

---------------------------------------------------------------------------------------

## After : 
  decltype을 통해 자료형을 알아내면, 템플릿을 통한 auto 반환형 함수의 구현이 가능하다.

  - 예제 코드
```c++
templae <typename T1, typename T2>
auto add(T1 t1, T2 t2) -> decltype(t1+t2) {
  return t1 + t2;
}
```
  t1이 double, t2가 int일 경우, 둘을 더하고 decltype을 통해 반환형을 double로 만들 수 있다.

-------------------------------------------------------------------------------------

decltype은 함수와는 다르게, 타입을 알고자 하는 식의 타입으로 치환합니다.
```c++
decltype(/* 타입을 알고자 하는 식*/)

#include <iostream>

struct A{
  double d;
};

int main(){
  int a = 3;
  decltype(a) b = 2; // int
  
  int& r_a = a;
  decltype(r_a) r_b = b; // int&
  
  int&& x = 3;
  decltype(x) y = 2; // int&&
  
  A* aa;
  decltype(aa->d) dd = 0.1; // double
}
```

  Auto와는 다르게, 변수의 자료형으로 치환합니다.  
  즉, 우변에 있는 값의 타입으로 추정하는 것이 아니라, 확정적으로 자료형을 선언하는 것과 마찬가지입니다.

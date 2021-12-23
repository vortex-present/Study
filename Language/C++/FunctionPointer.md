# Function Pointer

  ### 다음과 같이 switch나 if-else와 같은 조건문으로 간단한 사칙연산을 처리할 수 있다.

- 예제 코드
```c++
#include <iostream>

using namespace std;

int main() {
	char oper;
	double a, b, ans;
	cin >> oper >> a >> b;
	switch (oper)
	{
	case '+': ans = a + b; break;
	case '-': ans = a - b; break;
	case '*': ans = a * b; break;
	case '/': ans = a / b; break;
	default: cout << "사칙연산(+, -, *, /)만을 지원합니다."; return 0;
	}
	ans = calc(a, b);
	cout << "사칙 연산의 결과는 " << ans << "입니다.";
	return 0;
}
```
--------------------------------------------------------------------------
  ### 사칙연산에 맞는 함수를 정의한 후, 조건문에서 해당하는 연산을 함수포인터에 대입해도 동일한 결과가 도출된다.
- 예제 코드
``` c++
#include <iostream>

using namespace std;

double add(double a, double b) { return a + b; }
double sub(double a, double b) { return a - b; }
double mul(double a, double b) { return a * b; }
double div(double a, double b) { return a / b; }

int main() {
	double (*calc)(double, double) = NULL;
	char oper;
	double a, b, ans;
	cin >> oper >> a >> b;
	switch (oper)
	{
	case '+': calc = add; break;
	case '-': calc = sub; break;
	case '*': calc = mul; break;
	case '/': calc = div; break;
	default: cout << "사칙연산(+, -, *, /)만을 지원합니다."; return 0;
	}
	ans = calc(a, b);
	cout << "사칙 연산의 결과는 " << ans << "입니다.";
	return 0;
}
```

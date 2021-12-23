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
	double (*calc)(double, double) = NULL; // 반환 타입, 변수 이름, 매개변수 타입
	char oper;
	double a, b, ans;
	cin >> oper >> a >> b;
	switch (oper)
	{
	// 포인터에 원하는 함수를 대입
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
------------------------------------------------------------------------
   ### 함수 선언 후, 함수 포인터 배열에 저장한다면 index로 더 빠르게 해당 함수에 접근할 수 있다.
- 예제 코드(함수 포인터 미사용)
```c++
#include <iostream>

using namespace std;

double add(double a, double b) { return a + b; }
double sub(double a, double b) { return a - b; }
double mul(double a, double b) { return a * b; }
double div(double a, double b) { return a / b; }
double (*calc[])(double, double) = {add, sub, mul, div};

int main() {
	int oper;
	double a, b, ans;
	cin >> oper >> a >> b;
	if(oper < 0 || oper > 3){
		cout << "0 : 덧셈, 1 : 뺄셈, 2 : 곱셈, 3 : 나눗셈";
		return 0;
	}
	ans = calc[oper](a, b);
	cout << "사칙 연산의 결과는 " << ans << "입니다.";
	return 0;
}
```
--------------------------------------------------------------
	### 함수 포인터를 매개 변수로 사용하면 더 유연하게 코드를 작성할 수 있다.
- 예제 코드
```c++
#include <iostream>
#define UP		1
#define DOWN	2

using namespace std;

void simple_sort(int* arr, int n, int cmp) {
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n - 1 - i; j++) {
			if (cmp == UP) {
				if (arr[j] > arr[j + 1])
					arr[j] ^= arr[j + 1] ^= arr[j] ^= arr[j + 1];
			}
			else if (cmp == DOWN) {
				if(arr[j]<arr[j+1])
					arr[j] ^= arr[j + 1] ^= arr[j] ^= arr[j + 1];
			}
		}
	}
}

void sort_print(int* arr, int n) {
	for (int i = 0; i < n; i++)
		cout << arr[i] << " ";
	cout << endl;
}

int main() {
	int arr[5] = { 10, 5, 41, 100, 2 };
	simple_sort(arr, 5, DOWN);
	sort_print(arr, 5);
	simple_sort(arr, 5, UP);
	sort_print(arr, 5);
	return 0;
}
```
- 예제 코드(함수 포인터 사용)
```c++
#include <iostream>

using namespace std;
typedef bool(*Cmp)(int, int);
bool Up(int x, int y) { return x > y; }
bool Down(int x, int y) { return x < y; }

void simple_sort(int* arr, int n, Cmp cmp) {
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n - 1 - i; j++) {
			if (cmp(arr[j], arr[j + 1])) {
				arr[j] ^= arr[j + 1] ^= arr[j] ^= arr[j + 1];
			}
		}
	}
}

void sort_print(int* arr, int n) {
	for (int i = 0; i < n; i++)
		cout << arr[i] << " ";
	cout << endl;
}

int main() {
	int arr[5] = { 10, 5, 41, 100, 2 };
	simple_sort(arr, 5, Down);
	sort_print(arr, 5);
	simple_sort(arr, 5, Up);
	sort_print(arr, 5);
	return 0;
}
```
	함수 포인터를 사용하여 조건에 따라 달라지는 함수를 작성하는 것이 용이해진다.

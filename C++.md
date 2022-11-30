# **1. C++ 언어의 특징**

• **C언어와의 호환성** : 기존에 작성된 C 프로그램을 그대로 사용할 수 있도록 C언어의 문법적 체계를 그대로 계승함

• **객체 지향** : 소프트웨어의 재사용을 통해 소프트웨어 생산성을 높이기 위해 객체 지향 개념을 도입함

• **타입 체크** : 타입 체크를 엄격히 하여 실행 시간 오류의 가능성을 줄이고 디버깅을 도움

• **효율성 저하 최소화** : 멤버 함수에 인라인 함수를 도입하는 등 함수 호출로 인한 시간 저하를 막음

# **2. bool 자료형**
다음과 같이 bool 자료형을 선언이 가능하다.
```cpp
bool a = true;
bool b = false;
bool c = 32 > 29;
bool d = a != false;
bool e = (a && b + a || b) && c;
```

사용시, 다른 변수와 같이 조건을 대체할 수 있다.

1 Bit만 저장 가능하기에 조건 값을 저장하기에 적합하다.
```cpp
if(a) { something }
while(a) { something }
for(; a; ;) { something }
```

특히, 조건이나 0, 1을 저장해야 하는 변수를 만들 때 효율적으로 메모리를 사용할 수 있으며, 가독성이 좋은 장점이 있다.
```cpp
// bool 사용 안함
int a = 30, b = 50;
int c = a > b;
if(c == 1) someFunc(c);

// bool 사용
int a = 30, b = 50;
bool c = a > b;
if(c) someFunc(c);
```

# **3. auto 변수**
자바 18, 코틀린, js 등에 있는 var와 유사하다. 변수의 자료형을 적지 않더라도 선언시 대입한 값을 담을 수 있는 자료형 중 __**메모리를 최소로 하는 자료형**__으로 자료형을 자동 설정한다.
```cpp
auto a = 10; // int
auto b = 0; // int
auto c = true; // bool
auto d = 2.5f; // float
auto e = .92f; // float
auto f = .16; // double
auto g = 6.12; // double
auto h = 'k'; // char
auto i = "hello"; // const char*
auto j = {32, 6, 2, 16}; // initializer_list<int>
auto k = {6.2, 17.7, 1.623, .64294}; // initializer_list<double>
auto l = new int[52]; // int*
auto m = new bool[17]; // bool*

// 구조체 또는 클래스 사용시
auto stack1 = new Stack(10); // Stack*
auto stack2 = Stack(20); // Stack
```

다만, auto 형식이라고 해서 마음대로 형 변환은 되지 않는다.
```cpp
auto a = 30;
auto b = 5f;
cout << a / b << endl;          // 6
cout << a * b << endl;          // 150
cout << b * a << endl;          // 150
cout << b * (float) a << endl;  // 150
cout << b / a << endl;          // 0.166667
a = 8.6f;                       // a = (int) 8.6f;
cout << a << endl;              // 8
```

# **4. 범위 지정 for-loop**
다음과 같이 3가지의 방식으로 작성이 가능하다.
```cpp
int array[] = {1, 2, 3, 4, 5};

for(int value: array) cout << value << ", ";
// '1, 2, 3, 4, 5, '

for(int &value: array) cout << value << ", ";
// '1, 2, 3, 4, 5, '

for(const int &value: array) cout << value << ", ";
// '1, 2, 3, 4, 5, '
```

예시를 들어보자. 먼저 배열을 선언해뒀다.
```cpp
int array[] = {1, 2, 3, 4, 5};
```

> 첫 번째 방법으로, 파라미터에 전달하는 방식인 Call-By-Value와 비슷하다.
> for문 안에서 배열을 복사해 value에 각 index의 값을 대입할 뿐이다.
> 그러므로, 수정해도 값은 변하지 않는다.
> ```cpp
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> // '1, 2, 3, 4, 5, '
> 
> for(int value: array) value++;
> 
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> // '1, 2, 3, 4, 5, '
> ```

> 두 번째 방법으로, 파라미터에 전달하는 방식인 Call-By-Reference와 비슷하다.
> for문 안에서 배열의 각 index의 주소를 value에 복사한다.
> 그러므로, 수정하면 값이 바뀐다. 주소인데도 불구하고 *을 쓰지 않아도 된다.
> ```cpp
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> // '1, 2, 3, 4, 5, '
> 
> for(int &value: array) value++;
> 
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> // '2, 3, 4, 5, 6, '
> ```

> 세 번째 방법으로, 파라미터에 전달하는 방식인 Call-By-Reference와 비슷하다.
> for문 안에서 배열 전체를 직접 참조한다.
> 이때는 아무런 복사도 일어나지 않는 대신, 값을 수정해줄 수 없다. 정말로 Read-Only가 되는 것이다.
> 그러므로, 값을 변경하려고 시도하면 오류가 발생하고 컴파일 조차 되지 않는다.
> ```cpp
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> // '1, 2, 3, 4, 5, '
> 
> for(const int &value: array) value++; // 오류 발생
> 
> for(int i = 0; i < 5; i++) cout << array[i] << ", ";
> ```

세 방법 모두 auto로 작성해도 된다.
```cpp
for(int value: array) cout << value << ", ";
for(int &value: array) cout << value << ", ";
for(const int &value: array) cout << value << ", ";

// 동일하다
for(auto value: array) cout << value << ", ";
for(auto &value: array) cout << value << ", ";
for(const auto &value: array) cout << value << ", ";
```

# **5. 정적 배열과 동적 배열**
```cpp
int[] constArray = {1, 3, 5, 7, 9};
int i = 5;
int[] nonConstArray = new int[i];
```
> constArray는 정적 배열이고, nonConstArray는 동적 배열이다.
> 
> constArray는 스코프를 벗어나기 전까지 항상 5개 크기의 int형 배열을 가지고 있다.
> 
> constArray의 크기는 어떤 방법으로도 바꿔줄 수 없다, 이를 **정적 배열**이라고 한다.

> nonConstArray는 똑같이 5개 크기의 int형 배열을 가지고 있지만, 이는 주소를 반환해준다.
> 
> 즉, i의 값에 따라서 nonConstArray의 크기는 바뀔 수 있다. 이를 **동적 배열**이라고 한다.

# **6. Template Function**
자료의 형태가 어떤 것이든지 간에 자동으로 해당 자료형으로 매개변수의 자료형이 바뀐다.
다만, 함수 이름 옆에 < > 으로 자료형을 명시해주면 어떤 자료형을 대입하든 해당 자료형이 들어온 것으로 인지하고 동작한다.
```cpp
template <typename T>
T sum(T a, T b) {
    return a + b;
}

int main() {
    cout << sum(1, 3) << endl;          // 4
    cout << sum(5.2, 8.9) << endl;      // 14.2
    cout << sum<int>(5.2, 8.9) << endl; // 13
    cout << sum(9.0f, -2.6f) << endl;   // 6.4
}
```

# **7. 어떤 주어진 자료의 n번째 숫자**
EZ 하죠?
```cpp
#include <cmath>

int getDigit(int a, int index) {
  return a / pow(10, index) % 10;
}
```

# **8. BFS, DFS의 자료구조**
BFS, DFS 모두 트리를 기반으로 하지만, 탐색시에는 재귀함수를 쓸 수도 있고, 스택을 쓰는 방법도 있다.

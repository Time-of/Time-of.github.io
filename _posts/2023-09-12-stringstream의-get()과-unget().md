---
categories: [C++, 문자열]
tags: [C++, 문자열, string]
use_math: true
---
# unget()
```cpp
std::string someString("1D2T#10S");
```  
위와 같은 문자열을 분해하려고 한다.  
분해하는데, 형식은 '숫자' + '영문자' + '특수문자'의 형식으로 분해해야 한다.  
그런데, '특수문자'는 있을수도, 없을 수도 있다.  
이런 상황이 분명히 존재한다.  
<br>

```cpp
std::string someString("1D2T#10S");

std::stringstream ss;

ss << someString;

int num;
char character;
char special;

ss >> num >> character >> special;

// 1D2, special에 '2'가 들어가버렸다
std::cout << num << character << special << "\n";
```  
순서대로 꺼낸다면 `1D` → `2T#` → `10S`의 순서대로 꺼내야 할 것이다만, 위 코드처럼 '잘못' 꺼냈다가는 코드가 읽기 어렵게 쓰일 수가 있다.  
<br>
`stringstream`에서는 '한 칸 되돌리기' 기능이 존재하는데, 이게 바로 `unget()`이다.  
```cpp
std::string someString("1D2T#10S");
std::stringstream ss;
ss << someString;
int num;
char character;
char special;

// 1, D, 2
ss >> num >> character >> special;

// special이 알파벳이거나 숫자라면
if (isalpha(special) || isdigit(special))
{
	// stringstream에 한 칸 되돌린다
	// 참고로 special에는 그대로 남아있음
	ss.unget();
}

// special에 잘못 들어간 '2'가 stringstream에 남아있다
// 따라서 2, T, #이 차례대로 들어간다
ss >> num >> character >> special;
```  
<br>

### unget() 사용 시 주의할 점
```cpp
std::string someString2("123D%");
std::stringstream ss;
ss << someString2;

int num;
char character;
char special;

// num에 123을 저장한다
ss >> num;

// 정수형으로 빼낸 123을 되돌리면 무슨 일이 일어날까?
ss.unget();

ss >> character >> special;

// 123, 3, D 출력
// character에 '3'이, special에 'D'가 들어간다
std::cout << num << ", " << character << ", " << special << "\n";
```  
`unget()`으로 되돌리기 시, 정수형의 맨 끝부분만 돌아온다는 점을 꼭 기억하도록 하자.  
<br>

# get()
```cpp
std::string someString2("123A@");
std::stringstream ss;

ss << someString2;

int num;
char character;
char special;

// num에 123을 저장한다
ss >> num;

character = ss.get();
special = ss.get();

// 123, A, @ 출력
// character에 'A'가, special에 '@'가 들어간다
std::cout << num << ", " << character << ", " << special << "\n";
```  
`get()`은 `char`형으로 현재 값을 반환하면서, 커서를 하나씩 우측으로 이동시킨다.  
<br>

```cpp
// num에 123이 저장되지 않는다
// num에 저장된 값은 49로, char형 값인 '1'.
num = ss.get();
```  
반환값에 주의하며 사용하도록 하자.  


---
categories: [C++, 문자열]
tags: [C++, 문자열, string]
use_math: true
---
# 파싱(Parsing)
파싱이란, 문장의 구성 성분을 분해하여 재조립해 원하는 데이터를 빼내는 일련의 과정.  
문자열은 단순히 `"Hello World"`같이 문자만 다루지 않으며, `"2023 09 07 THURSDAY"`, `"3x + 9y - 13 = 7"`과 같이 복합적인 성분으로 이루어져 있을 때도 많다.  
그리고 우리는 이 문자열에서 원하는 요소만 골라먹어야 한다.  
<br>
C++에서는 문자열에서 필요한 자료형에 맞는 정보를 꺼낼 수 있는 `stringstream`을 제공한다.  
<br>

본문 내용의 일부는 [링크](https://marketsplash.com/tutorials/cpp/cplusplus-stringstream/)의 내용을 정리하며 공부한 내용입니다.

# 헤더 파일
```cpp
#include <sstream>
```
<br>

# stringstream
### 개요
`stringstream`은 문자열과 함께 사용하는 입/출력 스트림 클래스다.  
문자열을 공백과 line break(`\n`)를 기준으로 자료형에 맞는 정보를 꺼낼 때 사용한다.  
문자열 객체를 스트림으로써 다루게 해 주고, 입/출력 스트림과 마찬가지로 문자열에서 추출/삽입 작업을 수행하게 해 준다.  
`stringstream`은 데이터 관리, 성능 향상, 코드 간소화 등 많은 기능을 제공하는 도구이므로, 잘 알아두도록 하자.
<br>

### 그래서 stream이 뭔데?
![mermaid-diagram-2023-07-23-155003](https://github.com/Time-of/Time-of.github.io/assets/83389425/97ccb8df-72a2-4079-bafa-7c6900b8519c)
사실 `#include <iostream>`을 밥먹듯이 쓰면서도, 스트림이 뭔지에 대해서는 고찰해보지 않았다.  
스트림이란, **간단히 말해 입/출력 흐름이 될 수 있는 데이터의 흐름**이다.
- 입력 스트림 (`cin`)
	- 키보드 등의 입력 장치로부터 입력된 데이터를 순서대로 프로그램에 전달하는 객체
	- 스트림 버퍼가 존재, Enter 키를 누른 순간 입력 스트림 버퍼를 통째로 프로그램에 전달
- 출력 스트림 (`cout`)
	- 프로그램의 출력 데이터를 모니터 등의 출력 장치로 순서대로 내보내는 객체
	- 버퍼가 꽉 차거나 `\n`이 도착할 때 출력

<br>

## stringstream 생성하기
본제로 돌아와서, `stringstream` 객체를 생성해보자.
```cpp
std::stringstream ss;
```
다른 객체들과 마찬가지로, 매우 간단하게 만들 수 있다.  

```cpp
ss.str("Hello World");
```
`str("문자열");`을 사용하여 초기화할 수 있다.  
초기화하는 경우 기존에 들어있던 문자열은 지워진다.  
<br>

### 삽입하기
`stringstream`에 **삽입**은, `cout`과 마찬가지로 `<<` 연산자를 사용할 수 있다.  
```cpp
std::stringstream ss;
// "Hello World"라는 문자열이 stringstream 객체에 삽입된다
ss << "Hello World";
```

<br>

### 추출하기
`stringstream`에서 **추출**은, `>>` 연산자를 사용할 수 있다. 추출하고 나면 `stringstream`에서 추출된 문자열은 제거된다.  
```cpp
std::stringstream ss;
// "Hello World"라는 문자열이 stringstream 객체에 삽입된다
ss << "Hello World";
	
std::string str1;
// str1은 이제 "Hello"가 된다
// ss에는 이제 "World"만이 남아있다
ss >> str1;
```
추출 연산자(`>>`)는 공백 또는 `\n`을 만나기 전까지 읽는다.  
따라서 위 코드에서는 `str1`에 추출한 결과 `str1`에는 `"Hello"`만 추출되었다.
<br>
```cpp
std::stringstream ss;
// "100"은 int가 아니라 문자열
ss << "100";
	
std::string str1;
ss >> str1;

// str1에는 "100"이 들어간다.
std::cout << str1;
```
```cpp
std::stringstream ss;
// "100"은 int가 아니라 문자열
ss << "100";
	
int integer1;
ss >> integer1;

std::cout << integer1;
```
또한, 문자열을 알맞은 자료형으로 바꾸어 추출하는 것 역시 가능하다.
<br>

### 지우기
`clear()`을 호출하여 `stringstream`을 비울 수 있다.

<br>

## stringstream 사용하기
### 다양한 타입으로 문장 구성하기
`stringstream`을 사용한다면, 다른 데이터 타입을 부드럽게 연결할 수 있다.  
자료형이 뒤죽박죽인 문장을 작성하고자 할 때 특히 유용하게 사용할 수 있다.  
문자열과 숫자 자료형이 뒤섞인 문장을 작성해보자:  
```cpp
float HP = 91.6f;
int Strength = 3;

std::stringstream ss;
ss << "Character's HP is " << HP << ", Strength is " << Strength;

std::string str1 = ss.str();

// 'Character's HP is 91.6, Strength is 3'이 str1에 모두 담긴다
std::cout << str1 << "\n";
```
`stringstream`에 입력 연산자 `<<`를 사용해 여러 타입의 데이터를 `to_string()` 없이 담아낼 수 있다.  
`ss.str()`을 사용해 `stringstream`에 담긴 모든 내용을 문자열로 변환할 수 있다.  
<br>
`stringstream`을 `std::cout`을 사용하듯이 사용하여 다양한 자료형에 대해 깔끔한 문장을 구성할 수 있다는 점을 알아보았다.
<br>

### 문자열 분해하기
문자열을 다양한 타입으로 구성할 수 있었다면, 다양한 타입으로 나눠버릴 수도 있다.  
```cpp
std::string date = "2023 09 07 THURSDAY";

std::stringstream ss;

ss.str(date);

int year, month, day;
std::string daysOfWeek;

ss >> year >> month >> day >> daysOfWeek;

std::cout << year << " " << month << " " << day << "\n";
std::cout << "요일: " << daysOfWeek << "\n";
```
문자열 `date`에서 연도, 월, 일, 요일을 타입별로 나눌 수 있다.  
코딩테스트에서도 매우 유용하게 쓰인다.  
<br>
```cpp
// ,(Comma) 단위로 나뉜 문장도 분해할 수 있다
std::string DropInfo = "Enemy_Puppet,Wood,0.1f";

std::stringstream ss;
ss << DropInfo;

std::string EnemyName, DropItem;
float DropChance;

// getline()을 사용하여 공백 또는 '\n'을 대신하여 구분자로 쓰일 문자를 지정할 수 있다.
std::getline(ss, EnemyName, ',');
std::getline(ss, DropItem, ',');
ss >> DropChance; // DropChance는 0.1이 된다.
```
`std::getline()`을 사용해 공백이나 `\n` 대신 구분자로 쓰일 문자를 직접 지정하여 추출할 수 있다.
문자열에만 추출할 수 있다.
<br>

### 포맷 형식의 입력 읽기
때로는 데이터가 특정한 포맷(형식)으로 제공되기도 한다.  
특히 게임 관련 직종에서는 CSV(콤마 단위로 구분된 값)을 많이 접하게 된다.  
이런 경우, `stringstream`을 사용하면 쉽게 값을 읽을 수 있다.  
```cpp
// ,(Comma) 단위로 나뉜 문장도 분해할 수 있다
std::string EnemyInfo = "Enemy_Puppet,12.3f,5,15";

std::stringstream ss;
ss << EnemyInfo;

std::string EnemyName;
float EnemyHealth;
int EnemyStrength;
int EnemyDefense;

// 버릴 문자
char Comma;

// EnemyName: Enemy_Puppet
// EnemyHealth: 12.3f
// EnemyStrength: 5
// EnemyDefense: 15
ss >> EnemyName >> Comma >> EnemyHealth >> Comma >> EnemyStrength >> Comma >> EnemyDefense;
```
꼭 콤마 단위가 아니고, `2023/09/07` 등 형식이 지정된 인풋을 쉽게 읽을 수 있다.  
<br>
이렇듯, `stringstream`을 이해하고 활용하면 보다 깔끔하고 효율적인 코드를 구성할 수 있다.  

<br>

### 입출력 작업 함께 진행
`stringstream`의 강점 중 하나는, 동일한 문자열에서 입력 및 출력 연산을 동시에 수행 가능하다는 점이다.  
아래 예시를 보자:  
```cpp
std::stringstream ss("100");
	
int num;
ss >> num; // num은 이제 100이 된다.
num *= 3;
ss.str("");
ss.clear();
ss << num;
std::string Result = ss.str();

// Result는 "300"
std::cout << Result;
```
문자열에서 정수를 추출하고, 정수에 연산을 가한 뒤, 다시 문자열에 삽입하였다.  
<br>

### 문자열 조작하기
```cpp
std::stringstream ss("Axe,Hammer,Sword");
	
std::string Weapon;

while (std::getline(ss, Weapon, ','))
{
	// 각각의 문자열로 작업을 수행한다
	std::cout << Weapon << "을 창고에 넣었다!\n";
}
```
반복문과 함께 사용하여 `,`로 구분된 각각의 문자열에 대해 작업을 수행할 수 있다.  

<br>

## 오류 처리하기
```cpp
std::stringstream ss("Hello World");
	
// 오류 시뮬레이션
ss.setstate(std::ios_base::badbit);

int num;

ss >> num;

if (ss.bad())
{
	std::cout << "오류 발생!";
}
```
위 코드를 실행하면 `"오류 발생!"`이 출력된다.  
**badbit**는 스트림에서 더 이상 연산을 수행할 수 없는 오류 발생 시 stringstream에 의해 설정된다.  
<br>

## 성능 고려 사항
`stringstream`은 기능이 다양하고 사용하기 편리하나, 성능에 미치는 영향을 이해하는 것이 중요하다.  
`stringstream` 객체를 생성, 사용, 소멸하는 데 드는 비용은 특히 성능이 중요한 경우 프로그램의 효율성에 영향을 줄 수 있다.  
<br>
### 오버헤드
`stringstream` 객체 생성 및 소멸에는 동적 메모리 할당 및 해제가 필요하므로, 약간의 오버헤드가 발생한다.  
루프문 내에서 객체를 생성하려 한다면, 이 때문에 성능에 영향을 줄 수 있다.  
```cpp
// 반드시 '지양'할 것.
// 매 반복 시마다 오버헤드가 발생한다
for (const string& str : strings)
{
	std::stringstream ss;
	ss < str;
	// ...
}
```
<br>

### 문자열 연산 효율
`stringstream`은 많은 문자열 연산을 단순화하나, 항상 가장 효율적인 선택은 아니다.  
간단한 연결의 경우 문자열에 대한 직접적인 연산이 더 빠를 수 있다.  
```cpp
std::stringstream ss;
ss << "Hello " << "World";
// 단순 연산이 더 빠를 수 있다
std::string str1 = "Hello" + "World";
```
<br>

### 재사용하기
`stringstream`은 생성 및 소멸 시 약간의 오버헤드가 발생한다.  
객체를 생성/소멸하는 대신에, 초기화를 진행하여 재사용할 수 있다:  
```cpp
std::stringstream ss;
for (int i = 0; i < 10000; ++i)
{
	ss.str("");
	ss.clear();
	ss < i;
	// ...
}
```
이렇게 함으로 매 반복에서 `stringstream`이 재사용되므로 생성 및 삭제로부터 오는 오버헤드를 피할 수 있다.  
<br>



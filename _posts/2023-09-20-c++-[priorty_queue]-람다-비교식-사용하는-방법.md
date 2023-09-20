---
categories: [C++, STL]
tags: [C++, STL]
use_math: true
---
[stl - C++ priority_queue with lambda comparator error - Stack Overflow](https://stackoverflow.com/questions/5807735/c-priority-queue-with-lambda-comparator-error)
<br>

# 람다 식으로 custom comparator를 써보자
```cpp
auto cmp = [&](int a, int b) -> bool
    {
        return a > b;
    };
    
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
```
람다 식을 바로 넣으면 사용이 안되고, 따로 함수 객체로 만들어서 `decltype(cmp)`를 템플릿에 넣어 사용한다.  
`decltype`은 값에 맞는 타입을 '추출'해내는, `auto`의 쌍둥이 같은 존재.  
- `auto`는 값에 맞는 타입을 '추론'한다.  

람다 객체를 `decltype`을 사용하여 템플릿 인자로 넘겨버리는 것이다.  
<br>
참고로 `priority_queue`의 생성자 파라미터로도 `cmp` 함수 객체를 넘겨야 한다.  
<br>

# 참고사항
`priority_queue`의 정렬 순서는 `<algorithm>`의 `sort()`와는 반대 순서로 나열된다.  
`sort()`에서 `greater<int>()`가 내림차순이었다면, `priority_queue`에서 `greater<int>()`는 오름차순.  
그 이유는 **컨테이너로 `vector`를 사용하기 때문에, `vector`의 뒤쪽이 우선순위가 높도록 정렬되기 때문**이다.  
<br>
실제로 `pop()`함수를 열어보면 알 수 있다.  
```cpp
void pop() {
    _STD pop_heap(c.begin(), c.end(), comp);
    c.pop_back();
}
```
`c`가 container  
가장 뒤쪽을 빼내도록 구현되어 있음을 알 수 있다.  
일반적인 큐가 앞쪽에서 빼내기 때문에 헷갈릴 만 하다.  


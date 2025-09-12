### `noexcept` 키워드: 예외를 던지지 않는다는 약속

`noexcept`는 **함수가 예외를 발생시키지 않음을 컴파일러에게 알려주는 약속**입니다.

---

```
#include <iostream>

// 이 함수는 예외를 던지지 않습니다.
void safe_function() noexcept {
    std::cout << "This is a safe function." << std::endl;
    // 예외를 던지는 코드가 없습니다.
}

// 이 함수는 예외를 던질 수 있습니다.
void unsafe_function() {
    throw std::runtime_error("This is an unsafe function.");
}

int main() {
    try {
        safe_function();
        unsafe_function(); // 여기서 예외 발생
    } catch (const std::exception& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }
    return 0;
}
```

#### 성능 최적화: 예외 처리 비용 제거

예외가 발생할 가능성이 있는 함수는 컴파일러가 추가적인 정보를 생성해야 합니다. 
이 정보는 예외 발생 시 스택을 안전하게 풀고 객체의 소멸자를 올바르게 호출하는 데 사용됩니다.

`noexcept`는 컴파일러에게 "이 함수는 예외가 없으니, 안전을 위한 모든 예외 처리 코드를 생략해도 좋다"고 알려주는 역할을 합니다. 코드 크기가 줄어들고 실행 속도가 빨라집니다.

#### 안정성 보장: 예측 가능한 프로그램 종료

`noexcept` 함수에서 예외가 발생하면, C++ 런타임은 예외를 처리하기 위한 복잡한 스택 해제 과정 없이 즉시 `std::terminate()`를 호출하여 프로그램을 종료시킵니다.

 "이 함수는 예외가 발생할 수 없다"는 개발자의 중요한 약속이 깨졌기 때문입니다. 
 예외가 발생했을 때 프로그램의 상태가 예측 불가능해지는 것보다, 차라리 안전하게 프로그램을 종료시키는 것이 더 나은 선택일 수 있습니다.
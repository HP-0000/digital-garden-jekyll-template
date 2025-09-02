---
---


---

### 1. `typedef`

`typedef`는 타입에 새로운 별명(alias)을 붙이는 데 사용됩니다. 복잡한 타입을 간단하게 만들거나, 코드의 가독성을 높일 때 유용합니다.

C++

```
// 예시: std::vector<int>에 IntVector라는 새로운 이름을 부여합니다.
typedef std::vector<int> IntVector;

IntVector myVec; // 이제 IntVector를 std::vector<int>처럼 사용할 수 있습니다.

// 주의: typedef는 템플릿 별명을 만들 수 없습니다.
// 아래 코드는 컴파일 오류를 발생시킵니다.
// template <typename T>
// typedef std::vector<T> TVector;
```

**핵심:** 단순 타입 별명을 만들 때 사용하며, C++11 이후에는 `using`으로 대체되었습니다.

---
### 2. `using` 

`using`은 `typedef`의 모든 기능을 포함하며, **템플릿 별명(Template Aliases)을 만드는 기능이 추가된** 현대적인 타입 별명 문법입니다.

`using`을 사용하면 템플릿을 사용하여 새로운 타입 별명을 정의할 수 있습니다.

C++

```
// 예시 1: typedef와 동일하게 단순 타입 별명을 만듭니다.
using IntVector = std::vector<int>;

IntVector myVec;

// 예시 2: 템플릿 별명을 만듭니다. (using의 가장 큰 장점)
template <typename T>
using SmartPointer = std::unique_ptr<T>;

SmartPointer<int> myIntPtr = std::make_unique<int>(42);
SmartPointer<std::string> myStrPtr = std::make_unique<std::string>("hello");
```

**핵심:** `typedef`의 상위 호환이며, **템플릿 별명**을 만들 수 있어 현대 C++에서 가장 많이 사용되는 타입 별명 문법입니다.

---

### 3. `typename`

`typename`은 **템플릿 안에서 중첩된 이름이 타입임을 컴파일러에게 명시적으로 알려주는 역할**을 합니다.

템플릿 매개변수(`T`) 안에 있는 `T::InnerType`과 같은 이름은 컴파일러가 타입인지 변수인지 구별하기 어렵습니다. 이때 `typename`을 사용해 "이것은 타입입니다"라고 알려줍니다.

C++

```
template <typename T>
void doSomething(T arg) {
    // T::InnerType이 타입임을 알려주지 않으면 컴파일러가 혼란스러워합니다.
    // typename을 사용하여 "T::InnerType은 타입입니다"라고 명시합니다.
    typename T::InnerType myObject;
}
```

**핵심:** 템플릿 코드를 작성할 때, 템플릿 매개변수 안에 있는 종속적인 타입 이름을 가리킬 때 사용합니다.

### 요약

|키워드|주요 기능|주요 사용처|
|---|---|---|
|**`typedef`**|타입에 별명 부여|C++03 스타일 코드, 단순 타입 별명|
|**`typename`**|템플릿 안에서 중첩된 이름이 타입임을 명시|템플릿 메타 프로그래밍|
|**`using`**|타입에 별명 부여 (`typedef` 확장)|C++11 이상, **템플릿 별명** 포함|
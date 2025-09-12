
`shared_ptr`은 여러 포인터가 하나의 객체를 **공동으로 소유**하며, **참조 횟수**를 통해 객체 수명을 자동으로 관리하는 스마트 포인터입니다.

### `unique_ptr`과 비교하여 `shared_ptr`을 쓰는 이유

**`unique_ptr`**은 **단독 소유권**을 갖는 반면, **`shared_ptr`**은 **공동 소유권**을 가집니다.

- **`unique_ptr`**: 한 객체를 오직 하나의 포인터만 소유합니다. 포인터 간 소유권 이전은 가능하지만(move), 복사할 수 없습니다. 이 때문에 소유권 관계가 명확하고, `shared_ptr`보다 가볍고 빠르다는 장점이 있습니다.
    
- **`shared_ptr`**: 여러 포인터가 동일한 객체를 공유해야 할 때 사용합니다. 예를 들어, 여러 모듈이나 클래스에서 공통 데이터나 자원에 접근해야 할 경우에 유용합니다. 복사를 통해 쉽게 소유권을 공유할 수 있으며, 마지막 `shared_ptr`이 사라질 때 객체가 자동 해제됩니다.

---

### `shared_ptr` 사용 시 주의사항

`shared_ptr`은 메모리 관리를 자동화하지만, **순환 참조(circular reference)**라는 심각한 문제를 일으킬 수 있습니다.

#### 순환 참조 문제

두 객체가 서로를 `shared_ptr`로 가리킬 때 발생하며, 이는 메모리 누수(memory leak)로 이어집니다. 서로의 참조 횟수가 1 이상으로 유지되어 객체가 영원히 소멸되지 않습니다.

가장 흔한 순환 참조 형태 중 하나는 **자기 참조**입니다. 객체 내부에 자기 자신을 가리키는 `shared_ptr` 멤버를 두는 경우입니다.

C++

```
#include <iostream>
#include <memory>

class Node {
public:
    std::shared_ptr<Node> self_reference;
    ~Node() {
        std::cout << "Node 객체 소멸됨" << std::endl;
    }
};

int main() {
    auto node = std::make_shared<Node>();
    node->self_reference = node; // 순환 참조 발생
    // node와 node->self_reference가 서로를 가리키므로 참조 횟수가 2가 됩니다.
    return 0; // "Node 객체 소멸됨" 메시지가 출력되지 않고 메모리 누수 발생
}
```

위 예제에서 `main` 함수가 종료될 때 `node` 변수는 소멸되지만, `Node` 객체 내부의 `self_reference`가 여전히 자신을 가리키고 있어 참조 횟수가 1로 남습니다. 
이 때문에 소멸자가 호출되지 않습니다.

#### 해결책: `weak_ptr` 사용

[[weak_ptr]]



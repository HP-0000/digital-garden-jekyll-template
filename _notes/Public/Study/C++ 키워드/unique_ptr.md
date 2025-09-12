
### `std::move`
[[좌측값 참조 , 우측값 참조]]
**좌측값을 `static_cast`를 통해 우측값 참조로 변환**한다.

`unique_ptr` 은 `std::move`를 사용해 소유권을 명확하게 **이동**한다.
```
std::unique_ptr<int> unique_func()
{
	return std::unique_ptr<int>{new int(10.0)};
} 

int main(void) {
	auto ptr = unique_func(); 
	return 0;
}

```
다만 함수 반환 시, Return 되는 로컬 객체는 R-value 로 인식된다.
즉 반환 되는 Unique_Ptr은 우측 값이 되어 이동이 발생하기 때문에 
`std::move` 없이 그냥 반환하면 된다.

[[shared_ptr]]
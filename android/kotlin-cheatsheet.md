### internal

### apply()

### with()
- null 값이 아닌 경우 함수의 호출을 막을 방법이 없음.
- 인자로 전달되는 객체가 null이 아닌 게 확실 경우 사용 권장.
```
with(rv_task_list) {
    adapter = this@MainActivity.adapter
}
```

### lateinit
- 호출 시점에 초기화.
- 항상 null이 아닌 프로퍼티로 간주됨. 초기화하지 않았을 때 컴파일 수준에서 확인할 수 없음.
- primitive type에는 불가능함.
- setter/getter properties 정의가 불가능.

### lazy
- 호출 시점에 by lazy {} 에 정의에 의해 초기화를 진행. 마지막 줄에 초기화값 정의.
- 호출 시점에 초기화하지만 val이기 때문에 초기화 변경 불가능.
- val에만 사용 가능.
- 싱글톤 클래스의 구현을 프로퍼티에 적용한 형태.
- 초기화 데이터가 자주 변경되는 경우가 아니라면 lazy가 선호됨.

### Extension Functions
```
// Context 클래스에 toast 함수 추가
fun Context.toast(message: CharSequence) {
    // this는 Context 인스턴스!
    Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
}

// 사용 예
context.toast("네트워크를 확인해 주세요.")

// 자바는 함수형 인터페이스를 람다로 표현하는 방식이지만 코틀린은 고차 함수를 지원해 람다 자체를 타입으로 지정 가능
fun SQLiteDatabase.inTransaction(block: (SQLiteDatabase) -> Unit) {
    beginTransaction()
    try {
        block(this)
        setTransactionSuccessful()
    } finally {
        endTransaction()
    }
}

// 사용 예
db.inTransaction {
    // it은 코틀린이 만들어주는 임시 변수
    it.delete("members", "first_name = ?", arrayOf("도연"))
}
```
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

### object

- 싱글톤 객체.
- 다른 class를 상속하거나 interface를 구현 가능.

### companion object

- java static과 같은 효과.
- class 내부 private로 접근해야 하는 factory method 구현 시 유용함.
```
// 생성자의 접근 제한자가 private이므로 외부에선 접근할 수 없음.
class User private constructor(val name: String, val registerTime: Long) {

    companion object {

        // companion object는 클래스 내부에 존재하므로
        // private로 선언된 생성자에 접근할 수 있음.
        fun create(name: String) : User {
            return User(name, System.currentTimeMillis())
        }
    }
}
```

### inline, noninline

- 고차함수에 전달한 람다는 컴파일 과정에서 익명 클래스로 변환됨.
- 익명 클래스를 사용하는 코드를 호출할 때마다 매번 새로운 객체가 생성되므로 여러번 호출되는 경우 성능 저하가 일어남.
- inline함수는 private를 사용하여 함수를 정의할 수 없음.
- inline으로 처리되지 않아야 하는 매개변수에는 noninline 키워드 추가.
```
fun callFunction() {
    doSomething { print("문자열 출력!") }
}

// convert to Java
public void callingFunction() {
    doSomething(new Function() {
        @Override
        public void invoke() {
            System.out.print("문자열 출력!);
        }
    })
}

inline func doSomethig(body: () -> Unit) {
    body()
}
fun callFunction() {
    doSomething { print("문자열 출력!") }
}

// convert to Java
public void callingFunction() {
    System.out.print("문자열 출력!")
}

inline fun doSomethig(
    inlineBody: () -> Unit,
    noninline notInlinedBody: () -> Unit) {
        ....
    }
)
```
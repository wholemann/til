코틀린에서 전역 변수는 초기화가 필요하다.

그런데 해당 클래스에서 반드시 매번 사용되는 변수가 아니라면
오히려 메모리 손해일 수 있다.

그래서 늦은 초기화 2개를 제공한다.

## Late-Initialized properties

```
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```

### lateinit 조건
lateinit은 꼭 변수를 부르기 전에 초기화 시켜야 하는데 아래와 같은 조건을 가지고 있다.

- var(mutable)에서만 사용이 가능하다
- var이기 때문에 언제든 초기화를 변경할 수 있다.
- null을 통한 초기화를 할 수 없다.
- 초기화를 하기 전에는 변수에 접근할 수 없다.
    - lateinit property subject has not been initialized
- 변수에 대한 setter/getter properties 정의가 불가능하다.
- lateinit은 모든 변수가 가능한 건 아니고, primitive type에서는 활용이 불가능하다(Int, Double 등)

### lateinit 초기화 확인하기
```
class SampleActivity {

	private lateinit var sampleAdapter: SampleAdapter

	override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_sample_main)

		// 부르는 시점 초기화
    sampleAdapter = SampleAdapter(ImageLoaderAdapterViewModel(this@SampleMainActivity, 3))

		if (::sampleAdapter.isInitialized) {
			sampleAdapter.addItem()
			sampleAdapter.notifyDataSetChanged()
		}
	}
}
```


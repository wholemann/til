## Dirtiness test
### Business logic dirtiness test (for Activities)
1. R.Layout.* (layout files)
2. R.id.* (View's IDs)
3. 테스트 항목1과 항목2에서 0점이 아닌 어떤 클래스 또는 인터페이스에 대한 의존성을 갖는 경우

이 테스트는 이행규칙(transitive)이다. 당신의 Activities가 UI의 상세한 구현을 몰라야 할 뿐 아니라 액티비티에서 참조하는 클래스들 또한 UI 구현 부분을 몰라야 한다.
그러므로, 단순히 dirty code 를 액티비티에서 초기화하는 어떤 helper 클래스에 다 때려 넣어선 안 된다.

### UI logic dirtiness test (for clases that encapsulate UI logic)
1. Activity에 대한 의존성
2. 테스트 항목1에서 0점이 아닌 어떤 클래스 또는 인터페이스에 대한 의존성을 갖는 경우

이 테스트 또한 이행규칙(transitive)이다. encapsulate UI logic과 Activities 간의 의존성 체인이 있어선 안 된다.
안타깝게도, Context에 대한 의존성은 dirtiness point라 할 수 없다. Context를 UI 클래스에 제공하지 않으면 
View를 인플레이팅할 방법이 없기 때문이다. (Context는 킹갓오브젝트임 ㅇㅇ)

이 테스트에서 0점을 항상 목표로 하라. 물론 완벽하게 0점을 하긴 어렵지만 앱의 dirtiness 포인트가 확실히 올라가는 걸 알게 된다. 또한 그것들을 냅두는 이유도 명확해진다.


> 이글은 [Engineering guide to writing correct User Stories](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#clarifying-roles)을 번역한 것입니다.  
최대한 한국말에 맞게 의역을 해보았습니다. 번역으로 인해 의미가 흐려질 경우 실제 원어를 그대로 썼으며, 추가 설명을 위해  각주를 달아놓았습니다.  
부족한 번역이지만 읽어주시고, 피드백 부탁드립니다.

![Logo](https://thepracticaldev.s3.amazonaws.com/i/3p0vlvdavfjlwr3neiw8.png)

애자일을 하는 사람들은 유저 스토리 작성에 집착한다. 유저 스토리 작성이 정말 강력한 도구이긴 하다. 하지만 많은 사람들이 잘못 작성하고 있다.

예를 들어보면:
```
As a user
I want to receive issue webhooks from Gitlab
So that I can list all current tasks
```

올바른 유저 스토리 같아 보이지만, 사실은 많은 문제점을 내포하고 있다. 당신이 적어도 8가지 실수를 발견하지 못했다면 이 글은 당신에게 유용할 것이다.

이 글은 크게 3가지로 구분되어 있다:
  1. 유저 스토리 기본 형식을 개선
  2. 유저 스토리를 검증할 수 있도록 BDD를 적용한 유저 스토리 재작성
  3. 유저 스토리와 테스트, 소스코드, 문서를 연결

모든 부분이 흥미롭진 않겠지만, 모두 이해하는 것이 중요하다.

# 문제 발견 및 해결

다들 알겠지만 우리의 요구 사항은 정확하고, 모호하지 않으며, 완전하고, 일관성이 있고, 우선순위가 있으며, 검증가능하고, 수정할 수 있고, 추적 가능해야 한다. 물론 처음엔 요구사항처럼 보이진 않을 수도 있지만 말이다. 

유저 스토리는 위에 열거된 특성 중 일부가 없는 경향이 있다. 그 부분을 고쳐야 한다.

## 일관된 언어 사용하기

"receive issue webhooks" 과 "list all current tasks"는 어떻게 연결되는가? "tasks" 와 "issues" 는 같은 것인가 아닌가? 둘은 완전 다른 것들이거나 아니면 단어 선택이 잘못된 것이다. 우린 그걸 어떻게 알 수 있을까?

바로 용어 사전집을 통해서다. 모든 프로젝트는 앞으로 유비쿼터스 언어[^1]가 될 특정용어를 정의하는 것으로 시작해야 한다. 애초에 어떻게 이 용어집을 만들까? 우리는 도메인 전문가들에게 물어본다. 우리가 새로운 용어를 만나면: 모든 도메인 전문가들이 이 용어를 정확하고 비슷하게 이해하도록 해야한다. 또한 우리는 같은 용어라도 상황과 맥락에 따라서 다르게 이해될 수 있다는 사실에 주의해야 한다.

위의 사례에서 도메인 전문가에게 자문을 구한 후 우리는 "task" 와 "issue" 가 같은 것임을 알게 됐다고 해보자. 이제 잘못된 용어를 제거해야될 때다.
```
As a user
I want to receive issue webhooks from Gitlab
+++So that I can list all current issues
---So that I can list all current tasks
```
훌륭하다. Entity[^2]에 대해서 같은 용어를 쓰는 것은 우리의 요구사항을 더 명확하고 일관되게 만든다.

## 사용자는 당신만을 위한 제품을 원하지 않는다

마지막 줄을 수정하고 나서야 사용자의 목적은 "현재 모든 이슈를 나열하는 것" 이라는 걸 알게 됐다. 왜 사용자는 현재 모든 이슈가 나열되길 원하는가? 그게 사용자에게 무슨 의미가 있는가? 사실 어떤 사용자도 그걸 원하지 않는다. 이 요구사항은 정확하지 않다.

이건 요구사항 기술에 있어서 매우 중요한 문제를 나타내는 부분이다. 우리는 사용자의 목적과 우리의 목적을 혼동하는 경우가 많다. 우리의 목표가 사용자들을 기분 좋게 하는 것인만큼, 우선 사용자들에게 집중해야 된다. 그들의 욕구가 우리의 욕구보다 중요하다. 그래서 우리는 요구사항에 사용자의 욕구를 명시해야된다.  

사용자가 원하는 것을 어떻게 알 수 있을까? 다시 말하지만, 우린 알 수 없다. 반드시 실사용자에게 자문을 구해야 한다. 자문이 여의치 않으면 우리 스스로 가설이라도 세워야 한다.
```
As a user
I want to receive issue webhooks from Gitlab
+++So that I can overview and track the issues' progress
---So that I can list all current issues
```
피드백을 수집해보니 사용자들은 프로젝트의 진행 정도를 알고 싶어한다는 걸 알게 됐다. 그들은 이슈가 나열되길 원하는 게 아니었다. 그렇기 때문에 우리는 써드 파티 서비스로부터 이러한 사용자 피드백 정보를 받고 보관해야 되는 것이다.

## 기술에 관한 세부사항을 제거하기

"receive issue webhooks" 을 문자 그대로 원하는 사람을 만난 적이 있는가? 사실 누구도 그걸 원치 않는다. 이 경우에도 우리는 서로 다른 두 가지 관심사를 혼동한 것이다.

사용자의 목적과 그걸 달성시켜주는 기술적인 방법은 명확하게 구분해야 된다. 그리고 "to receive issue webhooks" 는 기술적인 구현에 대한 내용이다. 당장 내일 웹훅 대신 웹소켓, 푸쉬알람 또는 그 외의 기술로 변경될 수 있다. 그리고 기술의 변경 때문에 사용자의 목적이 변하진 않을 것이다.
```
As a user
+++I want to have up-to-date information about Gitlab issues
---I want to receive issue webhooks from Gitlab
So that I can overview and track the issues' progress
```
보이는가? 필요한 정보만 남았고, 구현 세부 정보는 없어졌다.

# 역할을 명확히 하기

문맥상으로 확실히 알 수 있는 부분은 우리가 개발자와 관련된 도구를 다루고 있다는 것이다. 우리는 Gitlab과 이슈 관리를 이용한다. 따라서 주니어 개발자, 중견 개발자, 시니어 개발자 등의 다양한 사용자를 갖게 될 것이라고 생각하는 건 어렵지 않다.

그래서 우리는 역할의 정의에 도달한다. 모든 프로젝트는 다양한 유형의 사용자를 갖는다. 비록 그것이 명시적인 유형이 아니라고 생각하더라도 말이다. 이러한 역할은 제품이 사용되는 이유에 따라 달라질 수 있다. 그리고 이러한 역할들은 우리가 프로젝트에 대한 용어를 정의하는 것과 같은 방식으로 정의되어야 한다.

위의 유저 스토리에서 우리가 말하는 사용자는 어떤 종류인가? 주니어 개발자는 프로젝트 관리자와 아키텍트와 동일한 방식으로 진행 진행 상황을 요약하고 추적할 것인가? 물론 그렇지 않다.
```
+++As an architect
---As a user
I want to have up-to-date information about Gitlab issues
So that I can overview and track the issues progress
```
서로 다른 사용자 역할마다 서로 다른 유저 스토리로 분리할 수 있다. 그리고 이것은 우리가 제공하는 기능들과 우리가 이러한 기능들을 누구에게 전달하는지도 세밀하게 조정해준다.

# 유저 스토리 확장하기

이 단순한 <역할이나 페르소나>로서, 나는 <목표/필요>를 원하기 때문에, <왜>는 간단하고 동시에 강력하기 때문에 위대하다. 그것은 우리에게 완벽한 의사소통의 방법을 제공한다. 그러나, 다음 형식엔 우리가 적어도 알아야 할 몇 가지 단점이 있다.

## 유저 스토리를 검증할 수 있도록 만들기

우리에게 주어진 유저 스토리가 여전히 가진 문제점은 그것이 검증될 수 없다는 것이다. 이 스토리가 사용자들에게 효과가 있는지 어떻게 확신할 수 있을까? 아직 확신할 수 없다.  


유저 스토리와 테스트 사이에 명확한 매핑이 없다. 유저 스토리를 테스트로 쓸 수 있다면 정말 멋질 것이다.  


근데, 그건 가능하다! 우리에겐 [Behavior-driven development](https://en.wikipedia.org/wiki/Behavior-driven_development) 와 `gherkin` 언어가 있다. [애초에 BDD가 만들어진 것도 바로 그 때문이다](https://cucumber.io/blog/the-worlds-most-misunderstood-collaboration-tool/). 우리가 유저 스토리를 검증할 수 있도록 해주는 `gherkin` 형식으로 다시 쓸 수 있다는 걸 의미한다.
```
Feature: Tracking issues' progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  Scenario: new valid issue webhook is received
    Given issue webhook is valid
    When it is received
    Then a new issue is created
```
이제 이 유저 스토리는 검증이 가능하다. 말 그대로 테스트로 사용할 수 있고, 그 상태를 추적할 수 있다. 게다가 이제 우리는 고수준의 요구사항과 구현 세부 사항 간의 매핑을 통해 요구사항을 정확히 충족시킬 방법을 알 수 있다. 우리는 비지니스 요구사항을 구현 세부사항으로 대체하지 않고 보완한다.

## 불완전함을 발견하기

일단 유저 스토리를 쓰기 위해 `gherkin` 사용했을 때 유저 스토리에 대한 시나리오를 쓰기 시작했다. 그리고 같은 유저 스토리에 대한 몇 가지 시나리오가 있을 수 있다는 것을 알게 되었다.

우리가 만든 첫 번째 시나리오를 살펴보자. "new valid issue webhook is received" 이다. 잠깐, 우리가 잘못된 웹훅을 받으면 어떻게 될까? 이 문제를 접어둬야 할까 말아야 할까? 추가 작업을 해야될지도 모른다.

이 경우 무엇이 잘못될 수 있고 무엇을 해야 하는지에 대한 정보를 [Gitlab의 문서]((https://docs.gitlab.com/ee/user/project/integrations/webhooks.html))에서 참고하자.
알고 보니 우리가 다루어야 할 두 가지 다른 케이스가 있다. 첫째, Gitlab이 실수로 쓰레기를 보낸 경우다. 두 번째는 인증 토큰이 일치하지 않는 것이다.

![Gitlab webhooks](https://thepracticaldev.s3.amazonaws.com/i/iglagw03ngpwu3fo88i4.png)

이제 이 유저 스토리를 완성하기 위해 두 가지 시나리오를 추가할 수 있다. 
```
Feature: Tracking issues progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  Scenario: new valid issue webhook is received
    Given issue webhook is valid
    And issue webhook is authenticated
    When it is received
    Then a new issue is created

  Scenario: new invalid issue webhook is received
    Given issue webhook is not valid
    When it is received
    Then no issue is created

  Scenario: new valid unauthenticated issue webhook is received
    Given issue webhook is valid
    And issue webhook is not authenticated
    When it is received
    Then no issue is created
    And webhook data is saved for future investigation
```
나는 이렇게 간단한 유저 스토리가 매우 복잡한 것처럼 느껴지는 게 좋다. 왜냐하면 내부적인 복잡성을 우리에게 드러내기 때문이다. 그리고 커져가는 복잡성에 맞게 개발 프로세스를 조정할 수 있다.

## 유저 스토리에 순위 부여하기

지금은 "이슈 진행 상황 요약하고 추적하는 게" 아키텍트에게 얼마나 중요한지 명확하지 않다. 다른 유저 스토리보다 더 중요한가? 좀 복잡해 보이기 때문에 대신 좀 더 쉽고 더 중요한 일을 할 수 있지 않을까?

순위와 우선순위는 어떤 제품에서든 중요하며, 무시할 수 없다. 요구사항을 작성하는 유일한 방법으로 유저 스토리를 사용한다 해도 말이다. 요구사항의 우선순위를 정하는 방법은 여러 가지가 있지만 [MoSCow](https://wemake.services/meta/rsdp/requirements-analysis/#requirements-prioritization) 방법을 준수할 것을 권장한다. 이 간단한 방법은 네 가지 주요 카테고리를 기반으로 한다. `most`, `should`, `could` 그리고 `won't` 다. 그리고 문서 어딘가에 프로젝트에 있는 유저 스토리의 우선순위를 정한 별도의 표가 있을 것임을 암시한다.

그리고 다시, 우리는 사용자들에게 각각의 기능이 얼마나 중요한지 물어봐야 한다.

제품과 함께 일하는 여러 아키텍트와 몇 차례 대화를 나눈 결과, 다음 항목들은 반드시 해야되는 것임을 알았다.

| <center>Feature</center> | Priority
:------- | :--------:
Authenticated users must be able to send private messages | Must
Architects must track issuesÎ progress | Must
There should be a notification about incoming private message | Must
Multiple message providers could be supported | Must
Encrypted private messages won’t be supported	 | Must
따라서 우선 순위가 적용되도록 유저 스토리의 이름을 수정한 기능은 다음과 같다.
```
Feature: Architects must track issues' progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  ...
```
우리는 심지어 유저 스토리와 위의 표를 연결시킬 수도 있다. 순위가 매겨진 요구사항 표에서 유저 스토리가 있는 기능 파일로 하이퍼링크를 사용하라. 이 방법으로 우리는 위의 기능이 가장 우선 순위가 높기 때문에 첫 번째로 개발될 것임을 확신할 수 있다.

# 배운 모든 걸 연결해보기

적절한 관리 없이는 유저 스토리, 테스트, 소스 코드 및 문서들로 당신의 머리가 복잡해질 것이다. 프로젝트가 지속적으로 성장하면, 어플리케이션의 어느 부분이 비지니스 유즈케이스를 담당하는지 알 수 없다. 이를 극복하기 위해 요구사항, 소스 코드, 테스트, 문서 등 모든 것을 함께 연결해야 한다. 우리의 목표는 다음과 같은 것으로 끝나는 것이다.

![Linking everything together](https://thepracticaldev.s3.amazonaws.com/i/j1pko4hb4s1ua9kmehv9.png)

파이썬을 사용하여 그 원리를 설명해보겠다.
유즈케이스는 앱이 수행할 수 있는 고유의 고수준 행동 집합으로 정의할 수 있다.(이건 클린 아키텍쳐의 관점과 상당히 비슷해 보인다).
보통 `usecases`라고 불리는 패키지를 정의하고 모든 것을 안에 넣기 때문에 기존의 모든 유즈케이스를 한꺼번에 간과하기 쉽다. 각 파일에는 다음과 같은 간단한 클래스(또는 함수)가 들어 있다.
```
class CreateNewIssueFromWebhook(object):
    """
    Creates new :term:`issue` from the incoming webhook payloads.

    .. literalinclude:: path/to/your/bdd/user-story/file
      :language: gherkin

    .. versionadded:: 0.2.0
    """

    def __call__(self, webhook_payload: 'WebhookPayload') -> 'Issue':
        # Do something ...
```
도메인 로직을 문서화하기 위해 테스트에 사용하는 것과 동일한 파일을 포함시켜야 하는데 이를 위해 `sphinx` 와 [`literalinclude` 명령](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-literalinclude)을 사용한다.
또한 용어집을 사용하여 `issue` 가 임의의 단어가 아니고 우리가 이 프로젝트에서 사용하는 특정 용어임을 나타낸다.

이렇게 하면 테스트, 코드, 그리고 문서들이 가능한 한 연결될 것이다. 그리고 앞으로 덜 신경써도 될 것이다. 이 과정을 자동화할 수 있으며, `usecases/` 내부의 모든 클래스가 그들의 문서에 `.. literalinclude` 명령을 포함하고 있는지 점검할 수 있다.

이 클래스를 사용하여 유저 스토리를 테스트할 수도 있다. 이 방법으로 요구사항, 테스트와 유저 스토리를 구현한 실제 도메인 로직을 연결할 수 있다.

끝났다!

# 결론

이 가이드는 당신이 사용자의 필요에 집중한 더 나은 유저 스토리 작성을 도와줄 것이며, 소스 코드를 깨끗하게 유지해주고, 다른(그러나 비슷한) 목적들을 위해 재사용할 수 있게 도와줄 것이다.

[^1]: 도메인 전문가와 개발자가 공통으로 사용하는 언어를 말한다.

[^2]: 서로 구별되는 하나하나의 대상. 주로 개체를 의미한다.

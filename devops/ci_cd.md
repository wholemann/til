### Continuous integration

- 지속적인 통합
- 깨지지 않는, 연속적인, 꾸준한
- '테스트할 항목, 여러 이슈 등 쌓이는 걸 나중으로 미루지 말자' 이게 핵심
- 이를 위해서 Git&Github, DevOps, TDD 가 필요한 것
- 정적 분석(실제 실행없이 분석하는 것)

### BuildMachine

- Watch ->(Pull)-> BuildMachine
- Build -> Test (CI)
- Docker (CD Deploy 전)

### Read-Only Master

- 기존의 CI는 한명만 깨져도 나머지 인원이 기다렸어야 했음
- 그러다 보니 눈치보여서 Test 코드 주석처리하고 CI를 진행하는 등 부작용 발생
- 커밋이 줄고 같이 일하는 걸 피하게 됨
- 그래서 각자 브랜치로 CI하고 문제가 없으면 Merger를 이용하여 Master Branch에 Merge
- Merge 전에 Pull Request 날려서 코드 리뷰도 하고 등등

### Docker Image

- environment는 Development, Staging, Production 3가지가 있음
- Development는 자주 변경 가능, Development는 로컬일 수도 어떤 브랜치 일 수도 있음
- Development 단계에서 문제가 없다고 완전히 검증됐을 때 Staging으로
- Staging 서버 이미지는 Production 서버 이미지와 완전 같아야 함
- 만약 다르면 잘못 구성한 것
- Staging 서버 이미지에서 테스트 결과 이상 없으면 바로 Production으로

### 정적 분석 도구

- codeclimate
- sonarqube
- java static analysis, javascript static analysis 등 검색



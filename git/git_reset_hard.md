### 특정 시점으로 돌아가야 할 때

- 특히 commit history를 보고 특정 시점으로 돌아가서 소스볼 떄 유용
- 아니면 해당 시점 이후의 개선을 직접 해볼 수도 있음

```
git checkout -b exercise-1
git log
git reset --hard (commit identifier)
```
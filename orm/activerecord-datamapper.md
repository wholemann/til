### Active Record
- Domain 객체(모델) 안에서 insert, update 등 CRUD를 함.
- Domain 로직이 복잡하지 않은 CRUD를 위한 좋은 선택.
- 빠르고 간편한 생산에 초점.

---

### Data Mapper
- Domain 객체와 Mapper과 분리되도록 구현하여 DB를 Domain 객체에서 격리.
- Mapper에서 CRUD를 처리.
- 비지니스 로직이 매우 복잡한 경우 유지보수 편리. Domain clean.
- 성능이 필요하면 ORM 쿼리를 최적화하기보단 DataMapper와 순수 SQL로 해결.
# ORM
## `select_related` vs `prefetch_related`
### `select_related`
> 객치가 역참조하는 single object(one-to-one or many-to-one)이거나, 정참조 foreign key일 때 사용
> DB 단에서 inner join 으로 쿼리 수행

### `prefetch_related`
> 객체가 정참조 multiple objects(many-to-many or one-to-many)이거나, 역참조 Foreign Key일 때 사용
> 각 관계 별로 DB 쿼리를 수행하고, 파이썬 단에서 조인을 수행
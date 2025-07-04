查询语句是快照读，是通过MVCC实现的，所以不会加行级锁
更新和删除操作都会加行级锁，且锁的类型都是独占锁

# 是怎么加行级锁的
****
加锁的对象是索引，加锁的基本单位是next-key lock，但是在一些场景下会退化成记录锁或间隙锁

## 唯一索引等值查询
用唯一索引进行等值查询的时候，查询的记录不存在，加锁的规则也会不同
- 存在，在索引树上定位到这一条记录后，索引中的next-key lock会退化成记录锁
- 不存在，会退化成间隙锁

唯一索引等职查询并且查询记录存在得情景下，仅靠记录所也能避免幻读问题

## 唯一索引范围查询
会对每一个扫描到的索引加next-key锁，如果遇到下面这些情况，会退化成记录锁或者是间隙锁
- 针对大于等于得范围查询，因为存在等值查询条件，如果等值查询得记录是存在于表中，那么该记录得索引中得next-key锁会退化成记录锁
- 针对小于或者小于等于得范围查询，要看记录是否存在于表中
	- 扫描条件值记录不在表中，扫描到终止范围查询得记录时，该记录得索引得next-key会退化成间隙锁，其它扫描到的记录，都是在这些记录得索引上加next-key锁
	- 扫描条件值记录在表中，如果是小于条件得范围查询，扫描到终止范围查询得记录时，该记录得索引next-key会退化成间隙锁，其它扫描到的记录，都是在这些记录得索引上加next-key锁，如果小于等于条件得范围查询，扫描到终止范围查询得记录时，该记录得索引不会退化成间隙锁，其它扫描到的记录，都是在这些记录得索引上加next-key锁

## 非唯一索引等值查询
这时存在两个索引，因为主键索引肯定唯一得，非唯一只能是出现了二级索引。索引在加锁时，两个索引都会加锁，但是对主键索引加锁得时候，只有满足查询条件得记录才会对他们得主键索引加锁
针对非唯一索引等值查询慢查询的记录存不存在，加锁得规则也会不同
- 当查询的记录存在时，直到扫描到第一个不符合条件的二级索引记录就停止扫描，然后在扫描的过程中，对扫描到的二级索引记录假的时next-key锁，而对于第一个不符合条件的二级索引记录，该二级索引的next-key会退化成间隙锁，同时在符合查询条件的记录的主键索引上加记录锁
- 当查询的记录不存在时，扫描到第一条不符合条件的二级索引记录，该二级索引的next-key锁会退化成间隙锁，因为不存在满足查询条件的记录，所以不会对主键索引加锁

![[Pasted image 20250520143913.png]]
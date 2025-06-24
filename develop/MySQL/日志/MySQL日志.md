![[Pasted image 20250520162533.png]]
# 为什么需要undo log
****
是一种用于撤销回退的日志，在事务没提交之前，MySQL会先记录更新前的数据到undo log日志文件里面，当事务回滚时，可以利用undo log来进行回滚
![[Pasted image 20250520164816.png]]
![[Pasted image 20250520170233.png]]
****
# 为什么需要Buffer Pool
![[Pasted image 20250520195417.png]]
# 为什么需要redo log
防止断电导致数据丢失的问题，当有一条记录需要更新的时候，会先更新内存，然后对这个页的修改以redo log形式记录下来。WAL技术指MySQL的写操作不是立刻写道磁盘上，而是先写日志，然后在合适得时间再写道磁盘上

# 为什么需要binlog
binlog是记录了数据库表结构变更和数据库修改的日志
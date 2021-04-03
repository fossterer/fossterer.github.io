# Oracle 12 deletes all records when sub-query fails

A surprising thing happened this week! An engineer in our team brought to our attention that the reason a delete query has been removing everything from a table during batch job executions is because of a sub-query that is supposed to fail, not just return 0 records, but does not. A quirk!

Here's how it goes:

When a column doesn't exist in an Oracle table, sure a query over it throws exception.

````sql
select id from table1 -- Here 'id' is not a valid column at all in `table1`
````

But, if it is wrapped as a sub-query inside another query, apparently the entire WHERE clause ceases to disappear.

````sql
delete from table2 where id in (select id from table1) -- table2 has column 'id'
````

The above query against expectations runs successfully.

In short, as `select id from table1` is an exception-throwing operation, the statement effectively became here - `delete from table2`;

## Quirks of sub-query handling in Oracle

To begin with, it is not intuitive.
For example,

1. A query using JOINs such as `select * from table2 t2 join table1 t1 on t2.id = t1.id;` does throw exception as expected

2. While reading more about this, I came across [this non-primary source](https://stackoverflow.com/questions/24746087/oracle-select-query-with-inner-select-query-error) on another quirk. This not allowing ORDER BY inside a sub-query *simply because* it would have no effect is something that does not seem consistent with the above practice of not throwing an exception when something is non-processable

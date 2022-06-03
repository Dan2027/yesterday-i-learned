# Any SQL

- [Primary key (PK) vs Unique key (UK)](https://stackoverflow.com/questions/9565996/difference-between-primary-key-and-unique-key): they both have a unique constraint, but the indexing strategy is different. A PK can never be null, whereas a UK will accept null... [depending on implementation, sometimes more than one](https://www.enterprisedb.com/postgres-tutorials/postgresql-unique-constraint-null-allowing-only-one-null).
- [`OUTER JOIN ON TRUE`](https://stackoverflow.com/questions/21520048/what-means-table-a-left-outer-join-table-b-on-true) is a technique to get yourself a `CROSS JOIN` (null values allowed on either table) without saying `CROSS JOIN`.
- SQL is more than 40 years old and is still used today, not because it's great, but because [it just works, 90% of the time](http://blog.sqlizer.io/posts/sql-43/), makes [RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system) and SQL solved problems in computing.
- `TRUNCATE some table` differs from `DELETE * FROM some table` in that ~~the `TRUNCATE` statement does not leave behind transaction logs, and thus cannot be rolled back.~~ [`TRUNCATE` requires the `TRUNCATE` privilege, fires `ON TRUNCATE` triggers rather than `ON DELETE`](https://www.postgresql.org/docs/9.1/sql-truncate.html), and does so immediately, rather than a `DELETE`, which only marks rows as deleted, to actually be deleted later by a background process. (Here's a trivia you will never use again)
- You don't need to select any database to `SELECT 1;`. This is a poor man's way of checking if the database connection is working.
- "Technically, PRIMARY KEY is merely a combination of UNIQUE and NOT NULL".
- Turns out inverting values is pretty easy, even without intermediate values. `UPDATE table SET col = (CASE WHEN col = 0 THEN 1 ELSE 0 END);`
- `SELECT (SELECT ...) as resultColumnName;` creates a [temp table](https://leetcode.com/problems/second-highest-salary/), which guarantees the select to return a row of `null` instead of no rows.
- You can't [have `WHERE` clauses in a `GROUP BY` query](https://leetcode.com/problems/duplicate-emails/), but you _can_ turn that into a temp table and have a `WHERE` clause filter on that. Note: **temp tables must have an alias**, i.e. `SELECT (SELECT ...) as alias`.
- `%` is a thing, so selecting odd-numbered IDs is really `WHERE id % 2 = 1` or something like that.
- If a foreign key field is [unique and nullable at the same time](https://stackoverflow.com/questions/7573590/can-a-foreign-key-be-null-and-or-duplicate), it can contain at most one `NULL`. If you really want to avoid that null, and can afford having an M2M relationship rather than an O2M relationship, you can [use an intersection table](https://softwareengineering.stackexchange.com/questions/335284/disadvantages-of-using-a-nullable-foreign-key-instead-of-creating-an-intersectio), where an intermediary table links up two other tables.
- [Sharding](https://en.wikipedia.org/wiki/Shard_%28database_architecture%29) is putting different rows in the "same" table in different database nodes. Horizontal [partitioning](https://en.wikipedia.org/wiki/Partition_%28database%29) puts different rows in different tables in the same database.
- If you want to `UPDATE` something with values from another tables, those tables need to be declared [using `FROM tablename` after the `SET` statement](http://www.postgresqltutorial.com/postgresql-update-join/).
- `<> NUL`, `!= NULL`, ... is actually `IS NOT NULL`. [Sometimes it isn't though](https://stackoverflow.com/a/9581790). `null` is never equal to `null`; `WHERE something = NULL` is never true.
- [Some expert](https://www.cybertec-postgresql.com/en/a-beginners-guide-to-postgresqls-update-and-autovacuum/) suggests inserting (and no updating) partitioned tables, where the most recent one is the active record. When it comes to cleaning up, you drop the oldest table. Not sure how this works with foreign keys, or unique constraints.
- [DDL changes the data structures, e.g. `CREATE TABLE`, `ALTER TABLE`, ...](https://stackoverflow.com/a/2578207/1558430) DML manipulates the data itself, e.g. `SELECT`, `INSERT`, ... `TRUNCATE` is DDL, while `DELETE` is DML.
- A ["columnar database"](https://searchdatamanagement.techtarget.com/definition/columnar-database) is just a database with rows and columns transposed. Word has it that aggregation is much faster to do (becausse if your columns are rows, a single "row" contains all your values). It is not clear how that's any different from selecting a single column from your everyday [RDBMS](https://en.wikipedia.org/wiki/Relational_database), with or without vertical partitioning.
- MySQL and Oracle don't support DDL in transactions.
- There is a name for "just prefetch the table and do the select in memory": [**N + 1 Problem**](https://stackoverflow.com/a/97253/1558430).
- Try not to have any indexes that you don't plan to use. They increase write times for obvious reasons.
- ["VERY bad things can happen when you exhaust your disk space ... don't run out of disk space"](https://dba.stackexchange.com/questions/187044/disk-space-unreleased-after-cleaning-up-rows-from-pg-table) - Evan
- If you shard your database into multiple instances in a cluster, and the shard depends on some kind of non-random primary key, then be careful with it, because [it may very well end up with just a few of your instances working very hard](https://cloud.google.com/spanner/docs/schema-design#primary-key-prevent-hotspots) if the primary key distribution is not uniform. This is called hotspotting.
- Oracle and Postgres take `/* C-style comments */`, too.

# MySQL

- To install MySQL server, client, and python adapters: `sudo apt-get install mysql-common mysql-client-4.4 libmysqlclent-dev mysql-server-5.5`
- To start/stop/restart MySQL server: `(/etc/init.d/mysql OR /etc/init.d/mysqld) start/srestart`
- [Tables are stored in `/var/lib/mysql`](http://forums.mysql.com/read.php?10,239450,239465#msg-239465), and, if you are using InnoDB, the file is actually called `ibdata`.

- Create a database: [`CREATE DATABASE ebdb;`](http://dev.mysql.com/doc/refman/5.0/en/create-database.html)
  (after which you can run `python manage.py syncdb` if that happens to be your thing)
- Select a database: `USE ebdb;` (which isn't `SELECT ebdb`, for some reason)
- Delete a database: [`DROP DATABASE ebdb;`](http://dev.mysql.com/doc/refman/5.0/en/drop-database.html)
- Show list of tables: `SHOW TABLES;`

- Create an index: **TODO**

- Show tables by schema: `SELECT table_name, engine FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = '(dbname)';`
- Show table columns: `SHOW COLUMNS FROM table_name;`
- Show table info: `DESCRIBE table_name;`
- Change table engine: [`ALTER TABLE table_name ENGINE=InnoDB;`](http://dev.mysql.com/doc/refman/5.5/en/converting-tables-to-innodb.html)
- Show table engines (and other info): `SHOW TABLE STATUS;`
- Show the SQL needed to create an existing table: [`SHOW CREATE TABLE table_name;`](http://stackoverflow.com/a/5615783/1558430)
- Show last `InnoDB` foreign key error: [`SHOW ENGINE INNODB STATUS;`](http://stackoverflow.com/a/179501/1558430)
- `SET unique_checks=0;` and `SET foreign_key_checks = 0;` does not help you drop a foreign key constraint.

- Create a user: `CREATE USER 'ebroot'@'localhost' [IDENTIFIED BY 'some_pass'];` (different from `CREATE USER 'ebroot';` and `CREATE USER 'ebroot'@'*';`)
- Create a user using a pre-hashed password: `CREATE USER 'ebroot'@'localhost' IDENTIFIED BY PASSWORD 'abcdef123456';`
- Give a user privileges: `GRANT ALL ON ebdb.* TO 'ebroot'@'localhost';`

- MySQL [does not](http://stackoverflow.com/a/10474104/1558430) support transactional schema alters at any time.
- Amazon [does not](https://forums.aws.amazon.com/message.jspa?messageID=153017#) allow SSH into RDS instances.
- [Why is MySQL's default collation latin1_swedish_ci?](http://stackoverflow.com/questions/6769901/why-is-mysqls-default-collation-latin1-swedish-ci)
- [No `FULL (OUTER) JOIN` in MySQL?](https://stackoverflow.com/questions/4796872/how-to-do-a-full-outer-join-in-mysql#4796911) WTF? (`LEFT JOIN` works as expected.)
- It's not that DDL statements don't work in a transaction, but [an implied `COMMIT` is run whenever you run DDL statements](https://stackoverflow.com/a/22806756/1558430), so any DML statements run before that DDL statement (like `INSERT; ALTER TABLE;`) will have been committed as well.
- [AUTO_INCREMENT keys are not reset](https://stackoverflow.com/questions/449346/mysql-auto-increment-does-not-rollback) after a transaction is rolled back. This is useful because you don't want a transaction to fail just because two transactions are inserting an automatically-numbered row each.

# PostgreSQL

- Get client version: `psql --version`
- [Get server version](https://stackoverflow.com/a/13733884/1558430): `pg_config --version`, or `SELECT version();`
- Logging into `psql`: `psql dbname username`
- Logging into `psql` to a specific DB: `psql -h somedb.com dbname username`
- [`~/.pgpass`](https://stackoverflow.com/a/16791494/1558430) is activated only if you also specify the username and database name.
- Backup a database: [`pg_dumpall > outfile`](http://www.postgresql.org/docs/9.1/static/backup-dump.html#BACKUP-DUMP-ALL) OR `pg_dump specific_db > outfile`
- [Transfer/Migrate a database to another postgres version](http://www.postgresql.org/docs/9.0/static/migration.html): `pg_dumpall -p 5432 | psql -d postgres -p 6543` (old port to new port)
- Compress a backup: `pg_dump dbname | gzip > outfile.gz`
- Restore a backup: `psql -f outfile postgres` (the keyword `postgres` here is necessary only if you load into a [large cluster](http://www.postgresql.org/docs/9.1/static/backup-dump.html#BACKUP-DUMP-ALL))
- Restore a compressed backup: `gunzip -c outfile.gz | psql dbname`
- Delete a database: (`sudo su - owner_unix_user`), then run `dropdb dbname -Uowner_unix_user -W`. Alternatively, in psql, switch to a database (`\c postgres;`), then `DROP DATABASE dbname;` to delete.
- [Postgres does not take performance hits from string lengths.](http://www.postgresql.org/docs/8.2/static/datatype-character.html) Putting it in reverse, it also means it cannot be sped up by shortening strings.
- [`varying` string type has no length limit](http://stackoverflow.com/questions/2904991/postgresql-character-varying-length-limit)
- `\c`: show the current user and database. `\c dbname` also switches to that database.
- `\d`: list tables (and indexes!)
- [`\d+ tablename`](http://stackoverflow.com/a/109334/1558430): describe the table.
- `\l`: list databases. Note that these commands (called "meta commands") [do not end with a semicolon](https://stackoverflow.com/a/1517692/1558430).
- Create an index: `create index concurrently if not exists index_name on table_name (ordered_fields);` Creating an index non-concurrently will block out writes, but not reads.
- Drop an index: `drop index concurrently if exists index_name;`
- Rename an index: `alter index old_index_name rename to new_index_name;`
- Monitoring psql: `sudo tail -n 50 -f /var/log/postgresql/postgresql-9.1-main.log`
- Setting monitoring flags: [`log_min_duration_statement = 0`, and `log_statement = all`](http://stackoverflow.com/a/12670828/1558430)
- Running a SQL file: [`psql -U username -d myDataBase -a -f myInsertFile`](http://stackoverflow.com/a/12085561/1558430)
- Re-index a table: `REINDEX TABLE tablename;` (doesn't work if the index is broken, which is _retarded_
  `REINDEX TABLE tablename force;` doesn't work.
- [Read the docs](https://wiki.postgresql.org/wiki/Things_to_find_out_about_when_moving_from_MySQL_to_PostgreSQL). Postgres strings must be enclosed with single quotes. Double quotes only work for system identifiers.
- You can choose the type of index to build. The default is B tree.
- According to [this pgcon video](https://www.pgcon.org/2016/schedule/events/934.en.html), GIN indices are good for full text search, and GiST indices are good for full text search, and ranges in general (not just geospatial stuff).
- [Want to write a bank app? Don't read-modify-update.](http://blog.2ndquadrant.com/postgresql-anti-patterns-read-modify-write-cycles/) Potential workarounds for race conditions include `INSERT` journals (inserting deltas, e.g. `insert... values (1)` for having one extra dollar), doing calculated `UPDATE`s (e.g. `update... set value = value + 1` for bumping up balance by 1), row locking with `SELECT... FOR UPDATE` (which waits if the row is already being read in another transaction), `BEGIN ISOLATION LEVEL SERIALIZABLE` (which aborts if another transaction is already updating the same row), or manage your own `version` column that limits what your `UPDATE` queries match (manually not recommended).
  [`LIKE '%s'`](https://www.w3schools.com/sql/sql_like.asp) means "ending with s". It is not a string substitution marker. To find in any position, use `LIKE '%s%'`. To find starting with something, use `LIKE 's%'`.
- `ORDER BY 1, 2` would order by column 1, then column 2.
- [`-Infinity`](https://stackoverflow.com/questions/19686635/earliest-timestamp-supported-in-postgresql) is a valid timestamp.
- You can find your config files by just running [`SHOW config_file;`](https://stackoverflow.com/a/3603162/1558430)
- `->'foo'` is selecting a key inside a JSON object, as JSON (i.e. `{"foo": {...}}`). `->>'foo'` is selecting the same key, except as a string (i.e. that same example would get you `"{...}"`).
- Selecting `->2` gives you the second (not "2th") item in a json list. Selecting [`->'2'`](https://www.postgresql.org/docs/9.3/static/functions-json.html) gives you the value of a json object's `2` key.
- There is no good way to [monitor your indexing progress](https://dba.stackexchange.com/questions/11329/monitoring-progress-of-index-construction-in-postgresql). The closest you have is a giant query [here](https://dba.stackexchange.com/a/161992/41651).
- [`timestamp without time zone AT TIME ZONE zone`](https://www.postgresql.org/docs/9.6/static/functions-datetime.html#FUNCTIONS-DATETIME-ZONECONVERT) obviously gives you a timestamp _with_ time zone. `timestamp with time zone AT TIME ZONE zone` obviously gives you a timestamp _without_ time zone. Doing `at timezone utc at timezone utc at timezone utc...` also [switches between UTC and not UTC](https://twitter.com/garybernhardt/status/1011388486190968832), depending on how many times you repeat it. [By design.](https://www.postgresql.org/message-id/CAKFQuwYeHxefXOWmF_fXOM%3DMfR%3DQOz%3DUas-HNz5_fA%3DR-koUfw%40mail.gmail.com) Obviously.
- Apart from [preserving key order](https://www.postgresql.org/docs/9.4/static/datatype-json.html), there is [no real advantage to storing JSON as `JSON`](https://www.sisense.com/blog/postgres-vs-mongodb-for-storing-json-data/) rather than `JSONB`. Other `JSON` perks include: preserving whitespace. [Possibly faster writes](https://docs.djangoproject.com/en/1.10/ref/contrib/postgres/fields/#django.contrib.postgres.fields.JSONField).
- JSONB has a hard [1GB size limit](https://dba.stackexchange.com/a/286357/41651).
- See what queries are running: `SELECT pid, now() - xact_start AS running_time, usename, substr(query, 0, 120) as query_str FROM pg_stat_activity WHERE query <> 'DISCARD ALL' ORDER BY running_time DESC, datname, query_start, client_addr;`
- [Kill connections by pid](https://stackoverflow.com/a/5109190/1558430): `SELECT pg_cancel_backend(pid);`, or `SELECT pg_terminate_backend(pid) FROM pg_stat_activity;`. [The first one is less mean than the second](https://serverfault.com/a/35344/523754): [cancel active queries, terminate idle sessions](https://community.pivotal.io/s/article/How-to-Cancel-Running-Queries-or-Idle-Sessions). If neither work, you can SIGQUIT the whole server in an emergency.
- [Selecting the nth row](https://stackoverflow.com/a/16777/1558430) with a non-standard query: `LIMIT y OFFSET n`
- Did you know you can [inherit table schemas](https://www.postgresql.org/docs/9.2/ddl-inherit.html)?
- [The auto-incrementing `SERIAL` type only goes up to 2 billion ish.](https://www.postgresql.org/docs/9.1/datatype-numeric.html)
- Index creation is locking, not `CONCURRENTLY` by default, because [concurrent indexes need more work](https://www.postgresql.org/docs/9.1/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY), are significantly slower, and are much more prone to error arising from transactions. Regular index builds can take place inside a transaction, but concurrent ones cannot.
- There are three kinds of "views": [temporary views](https://www.postgresql.org/docs/9.4/sql-createview.html), [materialized views](http://www.postgresqltutorial.com/postgresql-materialized-views/), and ["new tables"](https://www.postgresql.org/docs/8.1/sql-createtableas.html). [Materialized views](https://www.postgresql.org/docs/9.4/sql-creatematerializedview.html) saves the data at the time the query is run, and calling `REFRESH MATERIALIZED VIEW` locks the entire table. Use `CONCURRENTLY`. In the case of `CREATE TABLE ... AS SELECT ...`, the data is not changed after that initial population.
- Adding a [`DEFERRABLE` constraint](https://hashrocket.com/blog/posts/deferring-database-constraints) allows that same constraint to be changed within a transaction. A `DEFERRABLE` constraint can also be `DEFERRABLE INITIALLY IMMEDIATE`, which means the constraint will be enforced right after that statement, or `DEFERRABLE INITIALLY DEFERRED`, which means the constraint will not check anything until the transaction is committed.
- To update only some of the objects you select, use a subselect: `UPDATE your_table ... WHERE (conditions) AND your_table.id = ANY(SELECT id from your_table ORDER BY random() WHERE (same conditions outside) LIMIT 10000)`. Now you only update 10000 of your rows.
- You can [restrict a column's contents to a set of enumerated values](https://stackoverflow.com/questions/7250939/in-postgres-how-do-you-restrict-possible-values-for-a-particular-column) using `ALTER TABLE foo ADD CONSTRAINT check_name CHECK (column IN ('bar', 'baz'));`.
- Make the current _session's' process_ sleep with [`pg_sleep(seconds)`](https://www.postgresql.org/docs/9.0/functions-datetime.html).
- [Filter through JSON arrays](https://stackoverflow.com/questions/22736742/query-for-array-elements-inside-json-type) with either `json_array_elements` or `jsonb_array_elements`. You *need* to alias the array field as a table, i.e. `SELECT * FROM table, jsonb_array_elements(json_column->'items_array') column_alias WHERE column_alias...`
- To get how much space your tables use, run [this frankenstein query](https://wiki.postgresql.org/wiki/Disk_Usage), or `SELECT relname AS table_name, c.reltuples AS row_estimate, pg_total_relation_size(c.oid) AS total_bytes FROM pg_class c WHERE relkind = 'r' ORDER BY total_bytes DESC;`.
- **Select text only after converting it to `TEXT`**. `SELECT 'Foo'` is actually of unknown type. `SELECT 'Foo'::TEXT` returns a column with type `text` (test with `pg_typeof('Foo'::TEXT)`).
- If you try to shrink/decrease a `varchar` column's length (for example from `varchar(100)` to `varchar(1)`), all the data needs to fit in the new column. Postgres will NOT truncate the column for you: `ERROR:  value too long for type character varying(1)`. You can do a `USING` to limit the length of data as you alter: `ALTER TABLE foo ALTER COLUMN bar TYPE varchar(1) USING bar::varchar(1);`.
* [`WITH` expressions](https://www.postgresql.org/docs/10/queries-with.html) (common table expressions) allow you do declare basically anything inside the `WITH` block, and use it in the query that follows: `WITH regional_sales AS (SELECT...) SELECT ... FROM regional_sales`. Word has it that [CTEs are quite expensive](https://hakibenita.medium.com/be-careful-with-cte-in-postgresql-fca5e24d2119) if used incorrectly.
- `ALTER TABLE ALTER COLUMN SET DEFAULT` only affects rows that are inserted/updated, not existing ones.
- `ALTER TABLE ADD COLUMN ... DEFAULT` **does** affect existing rows, writing that default value into every row, **if** the postgres version is 10 or under.
- Postgres [in particular](https://stackoverflow.com/a/20154382/1558430) allows multiple NULLs in a nullable, yet UNIQUE column. Other databases might not have the same behaviour.
- [`ANALYZE foo;`](https://www.postgresql.org/docs/9.3/sql-analyze.html) updates the row estimates (`reltuples`) for that table.
- The [`RETURNING id`](https://stackoverflow.com/a/2944481/1558430) in `INSERT INTO persons (lastname,firstname) VALUES ('Smith', 'John') RETURNING id;` helps you know what you just inserted. Otherwise you will have to assume the last thing you inserted was the one, which is not true if there are multiple writers.

## Performance

- When in doubt: use `EXPLAIN ANALYZE (your query here);` to help you out. If it doesn't work: [look at the full list of things to do](https://wiki.postgresql.org/wiki/SlowQueryQuestions).
- [Tool for EXPLAIN ANALYZE](https://tatiyants.com/pev/): first prefix your query with `EXPLAIN (ANALYZE, COSTS, VERBOSE, BUFFERS, FORMAT JSON)`, and then run `psql -h host db usuer -A -f some_query.sql`.
- `EXPLAIN` returns whatever the engine _thinks_ about a query. `EXPLAIN ANALYZE` actually runs the query, which requires data in actual scale.
- Selecting an indexed column, but while using a function-wrapped parameter (e.g. `WHERE SOMETHING(ROW) == 1`) disables the index.
- [There are function-based indices](http://use-the-index-luke.com/sql/where-clause/functions) but it is discouraged for their own performance reasons.
- Multi-column indices are position-dependent; `CREATE INDEX tbl_idx ON tbl (a, b)` is different from `CREATE INDEX tbl_idx ON tbl (b, a)`, where selecting by `b` requires the second index.
- Continuing from the point above, if the first column in a multi-column index (^ i.e. `a`) is selected as a range, the subsequent indices (i.e. `b`) are useless in the same query.
  **Make sure the first column in a multi-column index is selected exactly.**
- Getting the create table SQL for a table: [see guide](http://stackoverflow.com/a/16154183/1558430)
- [`SELECT COUNT(*) FROM tbl` is slow](https://wiki.postgresql.org/wiki/Slow_Counting); use only with indexed `WHERE` queries instead.
- `SELECT field1, field2, field3, ...`, even if the list of fields includes all fields in the table, is [more likely to be faster than `SELECT *`](http://stackoverflow.com/a/65532/1558430), being more likely to use the index.
- [Creating a partial index on a boolean field](http://stackoverflow.com/questions/42972726/postgres-sql-create-index-for-boolean-column) is useful if only a few of the records have either true or false.
- ["An index computed on `upper(col)` would allow the clause `WHERE upper(col) = 'JIM'` to use an index."](https://www.postgresql.org/docs/9.1/static/sql-createindex.html)
- Postgres transparently breaks up large field values into multiple rows for performance reasons. They call this ["TOAST"](https://www.postgresql.org/docs/8.3/static/storage-toast.html).
- You can query two like tables at the same time using `UNION`: `select (same columns) from table1 UNION select (same columns) from table2`. If you can handle duplicate rows, use [`UNION ALL`](https://stackoverflow.com/questions/49925/what-is-the-difference-between-union-and-union-all), which is faster (if bandwidth is free).
- **Attempting to [remove nullable or add `NOT NULL` to a field during a zero downtime deployment](https://gist.github.com/majackson/493c3d6d4476914ca9da63f84247407b#notes-on-adding-not-null-columns-in-very-large-tables) will cost a lot of time.**
- `VACUUM ANALYZE VERBOSE;` is almost free to run. Run it every so often (it vacuums all tables). There is also autovacuum that automates the automation away from you.
- `VACUUM FULL` before postgres 9 had an issue with [increasing index sizes rather than compacting them](https://wiki.postgresql.org/wiki/VACUUM_FULL), making performance lower than before. That same page says use `VACUUM`/autovacuum instead, and use `VACUUM FULL` only when you need to free up disk space.
- SSDs and HDDs have different `random_page_cost` (default 4) and `seq_page_cost` values (default 1). For SSDs, `random_page_cost` may well be set to 1, which often affects how the query planner decides how to make a query.
- An ordered index (one where you specify `field_name DESC`) [hardly matters](https://dba.stackexchange.com/a/39599) except if you perform range queries over multiple columns.
- Non-material views are [as fast as the query you put inside it](https://dba.stackexchange.com/a/151220).
- Check timing with `\timing` or `\timing on`. Though if you type `\timing on;` (with the semicolon), it thinks `on;` is not a boolean and keeps it off.
- Echo with `\echo` (lol)
- Avoid ["cross joins"](https://www.w3resource.com/PostgreSQL/postgresql-cross-join.php) or cartesian product joins, aka `SELECT ... FROM more,than,one,table`, which produces a massive queryset of size `more x than x one x table`. Nevertheless, in some cases, a cross join might reference an index that an inner join does not, ending up being faster.
- Table locks obtained with `LOCK` are [released only when the transaction ends](https://www.postgresql.org/docs/9.0/sql-lock.html).
- Limit postgres connection count to 200-400 no matter the database size.
- The source table (the table having the foreign key to another table) [does not require an index on the foreign key](https://www.cybertec-postgresql.com/en/index-your-foreign-key/). If you query by that field a lot, you should make an index there.
- A subquery without saying `LIMIT 1` instantly ruins the performance of the outer query.
- [An index is automatically created for all primary keys and unique constraints](https://stackoverflow.com/a/970605/1558430), but [not](https://stackoverflow.com/a/48793428/1558430) for foreign keys... unless [you use Django](https://stackoverflow.com/a/6010542/1558430). which makes an index for foreign keys as well.
- While [foreign data wrapper (`postgres_fdw`)](https://www.postgresql.org/docs/9.5/postgres-fdw.html) helps you retrieve data across different databases, doing a join on them is costly (because "semi-joins" need to be sent to another system, said someone on the internet). [Do a subquery where possible](https://thoughtbot.com/blog/postgres-foreign-data-wrapper).
- [We all add indexes until the database is slow because of them](https://blog.crunchydata.com/blog/five-tips-for-a-healthier-postgres-database-in-the-new-year). Do some profiling, and only keep the ones you need.
- PgBouncer's job is to limit the number of connections to the database. Since [postgres connection management is (even?) worse than MySQL's](https://news.ycombinator.com/item?id=29455852), you should use pgBouncer in any self-respecting environment.

## Troubleshooting

### Corrupted index

This is what a corrupted index looks like:

```
db=> select * from assets_tile where id=43;
 id | ...
----+-----
 43 | ...
(1 row)
db=> reindex table assets_tile;
ERROR:  could not create unique index "assets_tile_pkey"
DETAIL:  Key (id)=(43) is duplicated.
```

### New databases don't have extensions installed

Postgres has 'templates', so running `psql -d template1 -c 'CREATE EXTENSION...'` adds the CREATE EXTENSION line to the list of queries to run when creating a database.

### Permission denied (while reading an `.sql`)

`sudo -u someone psql -f filename` reads the file as someone instead of you. If that someone cannot access the file but you can, pipe the file in as yourself:

`sudo -u someone psql < filename`

### My DB is blowing up

[Check your disk space usage](https://wiki-bsse.ethz.ch/display/ITDOC/Check+size+of+tables+and+objects+in+PostgreSQL+database) with

    SELECT
        pg_database.datname,
        pg_size_pretty(pg_database_size(pg_database.datname)) AS size
        FROM pg_database;

### `No operator matches the given name and argument type(s). You might need to add explicit type casts.`

Do a cast then:

    SELECT
        *
        FROM tblPoop
        WHERE tblPoop.volume::varchar LIKE '9001%';

# MongoDB

> "MongoDB is the core piece of architectural rot in every single teetering and broken data platform I've worked with."

MongoDB is actually NoSQL, so it shouldn't be in this file.

- Install: `sudo apt-get install mongodb`
- [To have MongoDB return all records](https://engineering.meteor.com/mongodb-queries-dont-always-return-all-matching-documents-654b6594a827#.emoh9pv03), you must either:
  - have an index on a field, and search for exactly that field, or
  - query with all keys when you build a multi-field index, or
  - never update anything. MongoDB misses documents if you update some while you read the same table (if they can be so called).
- Well don't actually connect to mongodb directly, use an ORM: [`npm install mongodb mongoose`](https://www.npmjs.com/package/mongoose)

# SQLite3

- Open database: `attach "some_file_name.db" as db;` or `.open db`
- Show tables: `.tables`
- Show columns for a table: [`PRAGMA table_info(table_name);`](https://stackoverflow.com/a/948204/1558430) (text table) or `.schema tablename` (SQL to create that table).
- Select from a table with column names: `.headers on`
- Close database (or quit the dumb shell): `.exit`

# PL/SQL

- I don't think [PL/SQL](https://www.guru99.com/sql-vs-pl-sql.html) is like SQL at all... [it's like a third-generation programming language that you can write an entire program with](https://docs.oracle.com/cd/A97630_01/appdev.920/a96624/01_oview.htm#7106). **You don't even interact with the DB with PL/SQL**. You can have SQL inside PL/SQL though.

# MSSQL

- `SELECT ... LIMIT n` is [`SELECT TOP(n) ...`](https://stackoverflow.com/questions/603724/how-to-implement-limit-with-sql-server) in MSSQL, because no reason.
- You need `WITH (NOLOCK)` everywhere in SQL Server because [the default transaction isolation level is READ COMMITTED](https://www.sqlshack.com/understanding-impact-clr-strict-security-configuration-setting-sql-server-2017/), which means your reads will fail if someone else is modifying it.
- On the other hand, you should really use `WITH (NOLOCK)` only if there are performance issues, because [you will be reading uncommitted data, which can be rolled back by another transaction](https://www.sqlservercentral.com/articles/the-effect-of-nolock-on-performance). You can get objects that cease to exist the next second.

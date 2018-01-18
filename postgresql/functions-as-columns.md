# Functions as Columns

Postgres allows _virtual columns_ by creating a function that receives a row as an argument, allowing some columns to be dynamic and calculated only when the query to that _"column"_ is performed, without the need for a view just for calculated columns.

Example, for the table bellow

```sql
CREATE TABLE users (
  user_id serial PRIMARY KEY,
  name varchar,
  registration varchar
);

INSERT INTO users(name, registration) VALUES
  ('john', '1234-5'),
  ('alice', '12-345'),
  ('bob', '36872'),
  ('jean', '551-34');

SELECT * FROM users;

/*
 user_id | name  | registration
---------+-------+--------------
       1 | john  | 1234-5
       2 | alice | 12-345
       3 | bob   | 36872
       4 | jean  | 551-34
*/
```

You could have a function that, for example, removed the `"-"` from the registration column

```sql
CREATE OR REPLACE FUNCTION normalized_registration(users)
RETURNS text AS $$
  SELECT replace($1.registration, '-', '')
$$ STABLE LANGUAGE SQL;

SELECT *, normalized_registration(users) FROM users;

/*
 user_id | name  | registration | normalized_registration
---------+-------+---------------------------------------
       1 | john  | 1234-5       | 12345
       2 | alice | 12-345       | 12345
       3 | bob   | 36872        | 36872
       4 | jean  | 551-34       | 55134
*/
```

And even use it for anything you would use a regular column, like aggergations for example

```sql
SELECT
  users.normalized_registration,
  string_agg(name, ',') as names
FROM users
GROUP BY users.normalized_registration
HAVING count(*) > 1;

/*
 registration |   names
--------------+------------
 12345        | john,alice
*/
```

Also, this is possible because *attribute notation* (users.name) and *function notation* (name(users)) are equivalent in Postgres.

## Reference

* <http://bernardoamc.github.io/sql/2015/05/11/postgres-virtual-columns/>
* <https://dba.stackexchange.com/questions/166792/postgresql-generated-columns>

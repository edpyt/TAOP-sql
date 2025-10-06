# The Art of PostgreSQL

Thanks for buying The Art of PostgreSQL! The edition you selected offers a
database dump of the datasets used in the book, allowing for easy replay of
the SQL queries.

This document shows how to restore the database. The core of it is as simple
as using the `psql` command. The database has some dependencies though…

## Extensions needed

The catalog query published at my blog was used to generate the list of
tables which have dependencies towards extensions in the “taop” database:

  https://tapoueh.org/blog/2019/11/list-postgresql-tables-using-extensions/

Here's the result:

~~~
┌─────────┬───────────┬─────────┬─────────┬──────────┐
│ extname │   type    │ schema  │  table  │  column  │
├─────────┼───────────┼─────────┼─────────┼──────────┤
│ ip4r    │ ipaddress │ tweet   │ visitor │ ipaddr   │
│ ip4r    │ ip4r      │ geolite │ blocks  │ iprange  │
│ hll     │ hll       │ tweet   │ uniques │ visitors │
│ hstore  │ hstore    │ moma    │ audit   │ after    │
│ hstore  │ hstore    │ moma    │ audit   │ before   │
└─────────┴───────────┴─────────┴─────────┴──────────┘
(5 rows)
~~~

Make sure to install those extensions when loading the SQL scripts for one
of the schemas listed above.

## Restoring the PostgreSQL database, one schema at a time

The zipfile `TheArtOfPostgreSQL-database-sql.zip` contains one file per
schema. Each of them can be restored with the following command line:

~~~
$ psql -p 54312 -1 -d taop -f xxx.sql
~~~

For the files that are named like one of the schema listed in the result of
the query above, please make sure that you did install the necessary listed
extension first.

The `hstore` extension is part of the Postgres core distribution, it's a
contrib. The ip4r and hyperloglog extensions are found on GitHub and how to
install them depend on the system you're using. Working in a debian virtual
machine (or container) might make things easier for you there.

  - https://github.com/RhodiumToad/ip4r
  - https://github.com/citusdata/postgresql-hll

## psql configuration

You can install the `psqlrc` file to try it:

~~~
$ cp -i psqlrc ~/.psqlrc
~~~

## Conclusion

Your journey into *The Art of PostgreSQL* is now starting.

> _Knowledge is of no value unless you put it into practice._

> ***Anton Chekhov***

Writing code is fun. Have fun writing SQL!


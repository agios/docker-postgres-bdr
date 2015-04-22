# Docker image for postgres 9.4 with BDR support

Based on Debian Jessie. Includes a patched postgres with support for [BDR](http://bdr-project.org/)

Image heavily borrowed from the [official image](https://github.com/docker-library/postgres) for postgres.
Look there for further usage instructions.

## Quick setup

Create a database on each node, eg named bdrdemo. Then, connect to the
database on the first node and run:

```SQL
CREATE EXTENSION IF NOT EXISTS btree_gist;
CREATE EXTENSION IF NOT EXISTS bdr;

SELECT bdr.bdr_group_create(
  local_node_name := 'node01',
  node_external_dsn := 'host=node01.host port=5432 dbname=bdrdemo'
);
```

Then, on all other nodes run:

```SQL
CREATE EXTENSION IF NOT EXISTS btree_gist;
CREATE EXTENSION IF NOT EXISTS bdr;

SELECT bdr.bdr_group_join(
  local_node_name := 'nodeXX',
  node_external_dsn := 'host=nodeXX.host port=5432 dbname=bdrdemo',
  join_using_dsn := 'host=node01.host port=5432 dbname=bdrdemo'
);
```

(Make sure to replace node names and hosts with appropriate values)

## docker run

```
% docker compose up -d
```

※ -d --detach オプションでバックグラウンド起動

## docker bash

```
% docker compose exec postgres /bin/bash
```

## initial SQL

postgresql のサンプル SQL を流し込む

```
pg_restore -U admin -d dvdrental dvdrental.tar
```

## postgres login

```
% psql -U admin dvdrental
```

## ER diagram

![ER](ER-diagram.jpg)

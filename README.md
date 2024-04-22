
# json files

## config.json for sql tap
``` json
{

    "host": "127.0.0.1",
  
    "port": "3306",
  
    "user": "root",
  
    "password": "root"
  }
```
## properties.json for sql tap
``` json
{
    "streams": [
      {
        "tap_stream_id": "app-a",
        "table_name": "a",
        "schema": {
          "properties": {
            "id": {
              "inclusion": "available",
              "minimum": -2147483648,
              "maximum": 2147483647,
              "type": ["null", "integer"]
            }
          },
          "type": "object"
        },
        "stream": "a",
        "metadata": [
          {
            "breadcrumb": [],
            "metadata": {
              "row-count": 3,
              "table-key-properties": [],
              "database-name": "app",
              "selected-by-default": false,
              "is-view": false,
              "selected": true,
              "replication-method": "FULL_TABLE"
            }
          },
          {
            "breadcrumb": ["properties", "id"],
            "metadata": {
              "selected-by-default": true,
              "sql-datatype": "int(11)",
              "selected": true
            }
          }
        ]
      }
    ]
  }
```

## config.json for postgre sql

``` json
{

    "postgres_host": "10.0.0.11",
  
    "postgres_port": 5432,
  
    "postgres_database": "app",
  
    "postgres_username": "root",
  
    "postgres_password": "1234",
  
    "postgres_schema": "mytapname"
  
  }
```
# note 
## python venv is compulsary for this to work


# important links
``` url
https://blog.panoply.io/etl-with-singer-a-tutorial

```

``` url
https://meltano.com/
```

``` url
https://docs.meltano.com/getting-started/part1
```

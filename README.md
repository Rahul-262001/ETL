
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
* properties.json must be a dictionary not a list in the
* ***official documentation of singer there they have given a list but in actual it is a dictionary***
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
* The **database** name is **app**
* The table name is **a**
* The table a has one column named **id**  

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
# Cssommands

## Create a virtual environment for your tap
``` cmd
python3 -m venv mysql_tap
```
## Activate your virtual environment
``` cmd
source mysql_tap/bin/activate
```
## Install the Singer package for the MySQL tap
``` cmd
pip install tap-mysql
```
## Run the tap in discovery mode to create a catalog.json file
``` cmd
tap-mysql --config config.json --discover > catalog.json
```
## Run the tap
``` cmd
tap-mysql --config config.json --properties properties.json
```

## Set up the PostgreSQL target
``` cmd
python3 -m venv postgresql_target
```
``` cmd
source postgresql_target/bin/activate
```
``` cmd
pip install target-postgres
```
## Transfer data from MySQL to PostgreSQL database
``` cmd
tap-mysql --config mysql_tap/bin/config.json --properties mysql_tap/bin/properties.json | postgresql_target/bin/target-postgres -c postgresql_target/bin/config.json >> state.json
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

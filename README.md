# introduction
* *Here i am trying to send the content of the database from mysql to postgre sql using singer ETL took*
![image](https://github.com/Rahul-262001/ETL/blob/main/image.png)

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
* ***Note***
* * In the official documentation it is suggested to use a list  converted into json but doing so causes a error
  * to fix that we have to convert that list into a dictionary adding { "streams": at the beginning and closing it at the end.
  * for example
  * ``` json
    {
    "streams": ["list content"]
    }
    ```   
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
# Commands

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
### Sample output
``` json
{'streams': [{'tap_stream_id': 'example_db-animals', 'table_name': 'animals', 'schema': {'type': 'object', 'properties': {'name': {'inclusion': 'available', 'type': ['null', 'string'], 'maxLength': 255}, 'id': {'inclusion': 'automatic', 'minimum': -2147483648, 'maximum': 2147483647, 'type': ['null', 'integer']}, 'likes_getting_petted': {'inclusion': 'available', 'type': ['null', 'boolean']}}}, 'metadata': [{'breadcrumb': [], 'metadata': {'row-count': 2000, 'table-key-properties': ['id'], 'database-name': 'example_db', 'selected-by-default': False, 'is-view': False, 'selected': True, 'replication-method': 'FULL_TABLE'}}, {'breadcrumb': ['properties', 'id'], 'metadata': {'sql-datatype': 'int(11)', 'selected-by-default': True, 'selected': True}}, {'breadcrumb': ['properties', 'name'], 'metadata': {'sql-datatype': 'varchar(255)', 'selected-by-default': True, 'selected': True}}, {'breadcrumb': ['properties', 'likes_getting_petted'], 'metadata': {'sql-datatype': 'tinyint(1)', 'selected-by-default': True, 'selected': True}}], 'stream': 'animals'}]}
{"type": "STATE", "value": {"currently_syncing": "example_db-animals"}}
{"type": "SCHEMA", "stream": "animals", "schema": {"properties": {"name": {"inclusion": "available", "maxLength": 255, "type": ["null", "string"]}, "id": {"inclusion": "available", "minimum": -2147483648, "maximum": 2147483647, "type": ["null", "integer"]}, "likes_getting_petted": {"inclusion": "available", "minimum": -128, "maximum": 127, "type": ["null", "integer"]}}, "type": "object"}, "key_properties": ["id"]}
{"type": "ACTIVATE_VERSION", "stream": "animals", "version": 1713768569189}
{"type": "RECORD", "stream": "animals", "record": {"name": "aardvark", "id": 1, "likes_getting_petted": 0}, "version": 1713768569189, "time_extracted": "2024-04-22T06:49:29.221681Z"}
{"type": "RECORD", "stream": "animals", "record": {"name": "bear", "id": 2, "likes_getting_petted": 0}, "version": 1713768569189, "time_extracted": "2024-04-22T06:49:29.221681Z"}
{"type": "RECORD", "stream": "animals", "record": {"name": "cow", "id": 3, "likes_getting_petted": 1}, "version": 1713768569189, "time_extracted": "2024-04-22T06:49:29.221681Z"}
{"type": "STATE", "value": {"currently_syncing": "example_db-animals", "bookmarks": {"example_db-animals": {"max_pk_values": {"id": 3}, "last_pk_fetched": {"id": 3}}}}}
{"type": "ACTIVATE_VERSION", "stream": "animals", "version": 1713768569189}
{"type": "STATE", "value": {"currently_syncing": "example_db-animals", "bookmarks": {"example_db-animals": {"initial_full_table_complete": true}}}}
{"type": "STATE", "value": {"currently_syncing": null, "bookmarks": {"example_db-animals": {"initial_full_table_complete": true}}}}

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


> **note** ## python venv is compulsary for this to work

# Python code
``` python
# to use the above in python
cmd = ["tap-mysql", "--config", "mysql_config.json", "--properties", "properties.json"]
result = subprocess.run(cmd, capture_output=True, text=True)
print(result.stdout)
# please modify further to and use result.stdout 
```
***to get the x509 certificate, openssl is required to generate the certificate because the mysql inbuilt certificate has been depricated***


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

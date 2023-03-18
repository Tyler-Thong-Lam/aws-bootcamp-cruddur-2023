# Week 4 — Postgres and RDS

Connecting to PSQL via psql client tool to use host flag for localhost 

```sh

psql -Upostgres --host localhost

```

Common PSQL commands

```sql

\x on -- expanded display when looking at data
\q -- Quit PSQL
\l -- List all databases
\c database_name -- Connect to a specific database
\dt -- List all tables in the current database
\d table_name -- Describe a specific table
\du -- List all users and their roles
\dn -- List all schemas in the current database
CREATE DATABASE database_name; -- Create a new database
DROP DATABASE database_name; -- Delete a database
CREATE TABLE table_name (column1 datatype1, column2 datatype2, ...); -- Create a new table
DROP TABLE table_name; -- Delete a table
SELECT column1, column2, ... FROM table_name WHERE condition; -- Select data from a table
INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...); -- Insert data into a table
UPDATE table_name SET column1 = value1, column2 = value2, ... WHERE condition; -- Update data in a table
DELETE FROM table_name WHERE condition; -- Delete data from a table

```

## Provision RDS Instance - Setup Postgres via AWS CLI

```
aws rds create-db-instance \
  --db-instance-identifier cruddur-db-instance \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --engine-version  14.6 \
  --master-username root \
  --master-user-password huEE33z2Qvl383 \
  --allocated-storage 20 \
  --availability-zone ca-central-1a \
  --backup-retention-period 0 \
  --port 5432 \
  --no-multi-az \
  --db-name cruddur \
  --storage-type gp2 \
  --publicly-accessible \
  --storage-encrypted \
  --enable-performance-insights \
  --performance-insights-retention-period 7 \
  --no-deletion-protection
```

With this setup, it will take us 15 minutes to create the instance

Besides that,  if you would like to save the credit or tight on budget, you can temporarily stop the instace from the AWS console 

[Insert Image]()

## Create Database

Create folder ``` backend-flask/db ```

Create file ``` backend-flask/db/schema.sql ```

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

DROP TABLE IF EXISTS public.users;
DROP TABLE IF EXISTS public.activities;
CREATE TABLE public.users (
  uuid UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  display_name text,
  handle text,
  cognito_user_id text,
  created_at TIMESTAMP default current_timestamp NOT NULL
);
CREATE TABLE public.activities (
  uuid UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_uuid UUID NOT NULL,
  message text NOT NULL,
  replies_count integer DEFAULT 0,
  reposts_count integer DEFAULT 0,
  likes_count integer DEFAULT 0,
  reply_to_activity_uuid integer,
  expires_at TIMESTAMP,
  created_at TIMESTAMP default current_timestamp NOT NULL
); 
```

Create a seed file 

```backend-flask/db/seed.sql```

``` sql
INSERT INTO public.users (display_name, handle, cognito_user_id)
VALUES
  ('Andrew Brown', 'andrewbrown' ,'MOCK'),
  ('Andrew Bayko', 'bayko' ,'MOCK');

INSERT INTO public.activities (user_uuid, message, expires_at)
VALUES
  (
    (SELECT uuid from public.users WHERE users.handle = 'andrewbrown' LIMIT 1),
    'This was imported as seed data!',
    current_timestamp + interval '10 day'
  )
```

## Bash Scripts

Create db-create file ``` backend-flask/bin/db-create```

``` bash

#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-CREATE"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"


NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "create database cruddur;"

```

Create db-connect file ``` backend-flask/bin/db-connect```

```bash
#! /usr/bin/bash

GREEN='\033[0;32m'
RED='\033[0;31m'
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-CONNECT"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

if [ "$1" = "prod" ]; then
    printf "${GREEN}It's PRODUCTION!${NO_COLOR}\n"
    CON_URL=$PROD_CONNECTION_URL
else
    printf "${RED}NOT PRODUCTION!${NO_COLOR}\n"
    CON_URL=$CONNECTION_URL
fi

psql $CON_URL
```

Create db-drop file ``` backend-flask/bin/db-drop```

```bash

#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-DROP"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "DROP DATABASE cruddur;"
```

Create db-schema-load file ``` backend-flask/bin/db-schema-load```

```bash

#!/usr/bin/bash

GREEN='\033[0;32m'
RED='\033[0;31m'
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-SCHEMA-LOADED"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

schema_path="$(realpath .)/db/schema.sql"
echo $schema_path

if [ "$1" = "prod" ]; then
    printf "${GREEN}It's PRODUCTION!${NO_COLOR}\n"
    CON_URL=$PROD_CONNECTION_URL
else
    printf "${RED}NOT PRODUCTION!${NO_COLOR}\n"
    CON_URL=$CONNECTION_URL
fi

psql $CON_URL cruddur < $schema_path

```

Create db-seed file ``` backend-flask/bin/db-seed```

```bash
#!/usr/bin/bash

GREEN='\033[0;32m'
RED='\033[0;31m'
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-SEED"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

seed_path="$(realpath .)/db/seed.sql"
echo $seed_path

if [ "$1" = "prod" ]; then
    printf "${GREEN}It's PRODUCTION!${NO_COLOR}\n"
    CON_URL=$PROD_CONNECTION_URL
else
    printf "${RED}NOT PRODUCTION!${NO_COLOR}\n"
    CON_URL=$CONNECTION_URL
fi

psql $CON_URL cruddur < $seed_path
```

Create db-setup file ``` backend-flask/bin/db-setup```

```bash
#! /usr/bin/bash

GREEN='\033[0;32m'
RED='\033[0;31m'
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-SETUP"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

bin_path="$(realpath .)/bin"
 
source "$bin_path/db-drop"
source "$bin_path/db-create"
source "$bin_path/db-schema-load"
source "$bin_path/db-seed"
```

Create db-session file ``` backend-flask/bin/db-session```

```bash
#! /usr/bin/bash

GREEN='\033[0;32m'
RED='\033[0;31m'
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="DB-SESSIONS"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

if [ "$1" = "prod" ]; then
    printf "${GREEN}It's PRODUCTION!${NO_COLOR}\n"
    CON_URL=$PROD_CONNECTION_URL
else
    printf "${RED}NOT PRODUCTION!${NO_COLOR}\n"
    CON_URL=$CONNECTION_URL
fi

NO_DB_URL=$(sed 's/\/cruddur//g' <<<"$CON_URL")
psql $NO_DB_URL -c "select pid as process_id, \
       usename as user,  \
       datname as db, \
       client_addr, \
       application_name as app,\
       state \
from pg_stat_activity;"
```

Create rds-update-sg-rule file ``` backend-flask/binrds-update-sg-rule```

```bash
#!/usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="RDS-UPDATE-SG-RULE"
printf "${CYAN}== ${LABEL} ==${NO_COLOR}\n"

counter=0
while ! aws ec2 describe-instances &> /dev/null
do
    printf "AWS CLI is not installed yet, waiting for ${counter} seconds...\n"
    sleep 1
    ((counter++))
done

aws ec2 modify-security-group-rules \
    --group-id $DB_SG_ID \
    --security-group-rules "SecurityGroupRuleId=$DB_SG_RULE_ID,SecurityGroupRule={Description=GITPOD,IpProtocol=tcp,FromPort=5432,ToPort=5432,CidrIpv4=$GITPOD_IP/32}"

```

[Insert Image of rds-update-sg-rule]()


## Installing Postgres Client

Setting the env var in backend-flask application

```python
  backend-flask:
    environment:
      CONNECTION_URL: "${CONNECTION_URL}"   
```

Then we will add psycopg to text 

```
psycopg[binary]
psycopg[pool]
```

```txt
pip install -r requirements.txt
```

## DB Object and Connection pool

We will add ```db.py``` to ```backend-flask/lib```

in ```db.py```

```py
from psycopg_pool import ConnectionPool
import os
import re
import sys
from flask import current_app as app

class Db:
  def __init__(self):
    self.init_pool()

  def template(self,*args):
    pathing = list((app.root_path,'db','sql',) + args)
    pathing[-1] = pathing[-1] + ".sql"

    template_path = os.path.join(*pathing)

    green = '\033[92m'
    no_color = '\033[0m'
    print("\n")
    print(f'{green} Load SQL Template: {template_path} {no_color}')

    with open(template_path, 'r') as f:
      template_content = f.read()
    return template_content

  def init_pool(self):
    connection_url = os.getenv("CONNECTION_URL")
    self.pool = ConnectionPool(connection_url)
  # we want to commit data such as an insert
  # be sure to check for RETURNING in all uppercases
  def print_params(self,params):
    blue = '\033[94m'
    no_color = '\033[0m'
    print(f'{blue} SQL Params:{no_color}')
    for key, value in params.items():
      print(key, ":", value)

  def print_sql(self,title,sql):
    cyan = '\033[96m'
    no_color = '\033[0m'
    print(f'{cyan} SQL STATEMENT-[{title}]------{no_color}')
    print(sql)
  def query_commit(self,sql,params={}):
    self.print_sql('commit with returning',sql)

    pattern = r"\bRETURNING\b"
    is_returning_id = re.search(pattern, sql)

    try:
      with self.pool.connection() as conn:
        cur =  conn.cursor()
        cur.execute(sql,params)
        if is_returning_id:
          returning_id = cur.fetchone()[0]
        conn.commit() 
        if is_returning_id:
          return returning_id
    except Exception as err:
      self.print_sql_err(err)
  # when we want to return a json object
  def query_array_json(self,sql,params={}):
    self.print_sql('array',sql)

    wrapped_sql = self.query_wrap_array(sql)
    with self.pool.connection() as conn:
      with conn.cursor() as cur:
        cur.execute(wrapped_sql,params)
        json = cur.fetchone()
        return json[0]
        
  # When we want to return an array of json objects

  def query_object_json(self,sql,params={}):

    self.print_sql('json',sql)
    self.print_params(params)
    wrapped_sql = self.query_wrap_object(sql)

    with self.pool.connection() as conn:
      with conn.cursor() as cur:
        cur.execute(wrapped_sql,params)
        json = cur.fetchone()
        if json == None:
          "{}"
        else:
          return json[0]
  def query_wrap_object(self,template):
    sql = f"""
    (SELECT COALESCE(row_to_json(object_row),'{{}}'::json) FROM (
    {template}
    ) object_row);
    """
    return sql
  def query_wrap_array(self,template):
    sql = f"""
    (SELECT COALESCE(array_to_json(array_agg(row_to_json(array_row))),'[]'::json) FROM (
    {template}
    ) array_row);
    """
    return sql
  def print_sql_err(self,err):
    # get details about the exception
    err_type, err_obj, traceback = sys.exc_info()

    # get the line number when exception occured
    line_num = traceback.tb_lineno

    # print the connect() error
    print ("\npsycopg ERROR:", err, "on line number:", line_num)
    print ("psycopg traceback:", traceback, "-- type:", err_type)

    # print the pgcode and pgerror exceptions
    print ("pgerror:", err.pgerror)
    print ("pgcode:", err.pgcode, "\n")

db = Db()
```

Then we will fully update in ```home_activities.py```

```py
from datetime import datetime, timedelta, timezone
from opentelemetry import trace
from lib.db import pool,query_wrap_array
tracer = trace.get_tracer("home.activities.here")
 
class HomeActivities:
  def run(logger,cognito_user_id=None):
    # logger.info('home-activities-cloudwatch')
    # with tracer.start_as_current_span("home-page-mock-data"):
    #   span = trace.get_current_span()
    # now = datetime.now(timezone.utc).astimezone()
    #   span.set_attribute("app.now", now.isoformat())  
    #   span.set_attribute("user.id", "We have not User id defined in flask")
    
    sql=query_wrap_array( """
    SELECT
        activities.uuid,
        users.display_name,
        users.handle,
        activities.message,
        activities.replies_count,
        activities.reposts_count,
        activities.likes_count,
        activities.reply_to_activity_uuid,
        activities.expires_at,
        activities.created_at
      FROM public.activities
      LEFT JOIN public.users ON users.uuid = activities.user_uuid
      ORDER BY activities.created_at DESC
    """)
    with pool.connection() as conn:
      with conn.cursor() as cur:
        cur.execute(sql)
        # this will return a tuple
        # the first field being the data
        json = cur.fetchone()
       
      return json[0]
      return results
```

## Connection RDS by GITPOD

```py
GITPOD_IP=$(curl ifconfig.me)

export GITPOD_IP=$(curl ifconfig.me)

gp env GITPOD_IP=$(curl ifconfig.me)
```

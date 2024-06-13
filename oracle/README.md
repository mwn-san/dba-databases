<div align="center">
  
# Oracle Database 12c Enterprise Edition

</div>

### Getting started on docker container:

```
# CREATE USER DIRECTORY DATA & DOCKER COMPOSE FILE FOR ORACLE DATABASE.

sudo useradd oracle
password oracle
su - oracle
mkdir -p /home/oracle/data
mkdir -p /home/oracle/archive_log
touch oracle-12c-enterprise.yaml
vi oracle-12c-enterprise.yaml

# CREATE CONTAINER & MONITOR LOG ORACLE DATABASE.

docker-compose -f oracle-12c-enterprise.yaml up -d
watch docker logs oracle-12c-enterprise -f
```
<div align="center">

## Below for the Cheat Sheet

</div>

#### EXEC DOCKER CONTAINER
```
docker exec -ti oracle-12c-enterprise bash
````
````
lsnrctl start
lsnrctl status
````
````
sqlplus / as sysdba
````
```
#### SHOW ACTIVE CONNECTION
```
```
SHOW CON_NAME;
```
#### SHOW PLUGGABLE DATABASE & CHANGE SESSION PDB
```
SELECT NAME, OPEN_MODE FROM V$PDBS;
ALTER SESSION SET CONTAINER = ORCLPDB1;
```
#### CREATE USER 
```
ALTER SESSION SET "_ORACLE_SCRIPT"=true;
CREATE USER MWN IDENTIFIED BY 12345678;
GRANT CONNECT, RESOURCE TO MWN;
GRANT ALL PRIVILEGES TO MWN;
GRANT DBA TO MWN;
```
#### SHOW LIST AVAILABLITY USERNAME
```
SELECT username FROM all_users;
```
#### CONFIG ADD SUPPLEMENTAL LOG TO DATABASE
```
ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
```
#### SHOW ACTIVE DATABASE
```
SELECT NAME FROM V$DATABASE;
```
#### CONFIG ARHIVE LOG
```
ALTER SYSTEM SET LOG_ARCHIVE_FORMAT='%t_%s_%r.dbf' SCOPE=SPFILE;
ALTER SYSTEM SET LOG_ARCHIVE_FORMAT='arch_%t_%s_%r.dbf' SCOPE=SPFILE;
ALTER SYSTEM SET log_archive_dest_1='LOCATION=/opt/oracle/archive_log' SCOPE=SPFILE;
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;
ARCHIVE LOG LIST;
SELECT DEST_ID, STATUS, TYPE, DESTINATION
FROM V$ARCHIVE_DEST
WHERE STATUS = 'VALID';
ALTER SYSTEM ARCHIVE LOG CURRENT;
ALTER SYSTEM SWITCH LOGFILE;
```
#### CONFIG LOB OPEARTION
```
ALTER SYSTEM SET "_disable_directory_link_check"=TRUE
             COMMENT='  - Directory SymLink Desupport- Jun 05, 2024 by MWN'
             SCOPE=SPFILE;
ALTER SYSTEM SET "_kolfuseslf"=TRUE
             COMMENT='  - Directory SymLink Desupport- Jun 05, 2024 by MWN'
             SCOPE=SPFILE;
STARTUP FORCE
ALTER PLUGGABLE DATABASE ALL OPEN;
```

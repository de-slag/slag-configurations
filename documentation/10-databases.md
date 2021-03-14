# Databases

## table of contents
1. [Naming Strategy](#naming-strategy)
2. [Creation](#creation)

## Naming Strategy

12-digits, parted in 4 areas

### Stage (1 digit)

* D: Development
* T: Test
* I: Integration
* P: Production

### Application short term (3 digit)

restriction: only letters

example: DMO for a demonstration application

### Business Context (2 digit)

i.e. a customer short term

restriction: only letter, except default value '00'

### Number (3 digit)

a hexadecimal numbering

### Examples:

T_DMO_00_100 - 256th db of an demo application without business context at test stage
D_REP_AC_007 - 7th db of a reporting application with 'ACME Corp.' as business context at development stage

## Creation

Default: MariaDB

### Server

create docker container:

    docker pull mariadb:10.5.9
    docker run --publish 3306:3306  --name some-mariadb -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mariadb:10.5.9

### Database

    create database SCHEMA_NAME;
    grant all privileges on SCHEMA_NAME.* TO 'SCHEMA_NAME'@'%' identified by 'PASSWORD';
    flush privileges;


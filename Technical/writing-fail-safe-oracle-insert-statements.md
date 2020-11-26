# Writing Fail-safe Oracle Insert Statements

Did you know that you can isolate PL/SQL statements that can raise exceptions while declaring just once the exceptions used across them?

````sql
--------------------------------------------------------
--  DDL for ALTER TABLEs
--------------------------------------------------------
-- Add a new column to existing TABLES
declare
    column_exists exception;
    pragma exception_init (column_exists , -01430);
begin
    begin
        execute immediate 'ALTER TABLE PLANETS ADD PLANET_NAME VARCHAR2(50=
) default '||chr(39)||'UNNAMED'||chr(39)||' not null';
        exception when column_exists then dbms_output.put_line('Exception =
caught: Column exists already');
    end;
    begin
        execute immediate 'ALTER TABLE SATELLITES ADD SATELLITE_NAME VARCH=
AR2(50) default '||chr(39)||'UNNAMED'||chr(39)||' not null';
        exception when column_exists then dbms_output.put_line('Exception =
caught: Column exists already');
    end;
    begin
        execute immediate 'ALTER TABLE STARS ADD STAR_NAME VARCHAR2(50) de=
fault '||chr(39)||'UNNAMED'||chr(39)||' not null';
        exception when column_exists then dbms_output.put_line('Exception =
caught: Column exists already');
    end;
    begin
        execute immediate 'ALTER TABLE CONSTELLATIONS ADD CONSTELLATION_NA=
ME VARCHAR2(50) default '||chr(39)||'UNNAMED'||chr(39)||' not null';
        exception when column_exists then dbms_output.put_line('Exception =
caught: Column exists already');
    end;
end;
````

In this beautiful piece above, we handle each statement on its own and so do we handle their exceptions individually. That's the sweetness of BEGIN-BEGIN-END-END construct

Even if we were to hand this script to a database change management system such as Liquibase after having made the changes to database inadvertently (during development), there won't be any exceptions as we handle them inside the script as shown

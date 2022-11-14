# postgresql 使用记录
 
 
## 查询获取字段名，字段类型，长度，主键，非空，自增，默认值，描述
+ 使用sql查询
  ```sql
  select
    c.relname as 表名,
    a.attname as 列名,
    (case
      when a.attnotnull = true then true
      else false end) as 非空,
    (case
      when (
      select
        count(pg_constraint.*)
      from
        pg_constraint
      inner join pg_class on
        pg_constraint.conrelid = pg_class.oid
      inner join pg_attribute on
        pg_attribute.attrelid = pg_class.oid
        and pg_attribute.attnum = any(pg_constraint.conkey)
      inner join pg_type on
        pg_type.oid = pg_attribute.atttypid
      where
        pg_class.relname = c.relname
        and pg_constraint.contype = 'p'
        and pg_attribute.attname = a.attname) > 0 then true
      else false end) as 主键,
    concat_ws('', t.typname) as 字段类型,
    (case
      when a.attlen > 0 then a.attlen
      when t.typname='bit' then a.atttypmod
      else a.atttypmod - 4 end) as 长度,
     col.is_identity	as 自增,
     col.column_default	as 默认值,
    (select description from pg_description where objoid = a.attrelid
    and objsubid = a.attnum) as 备注
  from
    pg_class c,
    pg_attribute a ,
    pg_type t,
    information_schema.columns as col
  where
    c.relname = '表名'
    and a.attnum>0
    and a.attrelid = c.oid
    and a.atttypid = t.oid
    and col.table_name=c.relname and col.column_name=a.attname
  order by
    c.relname desc,
    a.attnum asc

  ```

+ 命令行查询
  ```
  \d tablename
  ```

## 查询函数的详细sql
+ 命令行查询
  ```
  \sf 函数名
  ```
+ sql查询
  ```sql 
   select prosrc from pg_proc where proname='function_name'
  ```

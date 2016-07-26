# 07月21日

##关键词

mysql替换数据

##

修改唯一性约束

alter table table_name drop index `uk_name`;

alter table table_name add unique key `new_uk_name` (`col1`,`col2`);

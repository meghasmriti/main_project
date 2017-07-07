# main_project
REGISTER /usr/local/pig/lib/piggybank.jar;

DEFINE XPath org.apache.pig.piggybank.evaluation.xml.XPath();

A = LOAD '/flume_import/StatewiseDistrictwisePhysicalProgress.xml' using org.apache.pig.piggybank.storage.XMLLoader
('row') as (x:chararray);

 bpl = FOREACH A GENERATE XPath(x, 'row/State_Name') AS name, XPath(x, 'row/District_Name') AS dname, XPath(x, 'row/Project_Objectives_IHHL_BPL') AS obj_bpl,  XPath(x, 'row/Project_Performance-IHHL_BPL') AS per_bpl;


result =  FILTER bpl by obj_bpl == per_bpl;

final = FOREACH result generate dname, obj_bpl, per_bpl; 
																																					

STORE final INTO '/proj2' USING PigStorage(';');

sqoop export --connect jdbc:mysql://localhost/b1 --username 'root' -P --table 'district' --export-dir '/proj2' --input-fields-terminated-by ';' -m 1

sudo service mysqld start

mysql -u root

use b1

create table district ( district varchar(20), obj_bpl varchar(20), per_bpl varchar(20) );



2)


proj2_80%.png
proj2_UDF

store filter_data INTO '/proj2_80' USING PigStorage(';');

sqoop export --connect jdbc:mysql://localhost/b1 --username 'root' -P --table 'per_80' --export-dir '/proj2_80' --input-fields-terminated-by ';' -m 1
sudo service mysqld start

mysql -u root

use b1

create table per_80 ( district varchar(20), obj_bpl varchar(20), per_bpl varchar(20) );

# MySQL

## Resolvendo problemas com o Definer:
1. Change the DEFINER
This is possibly easiest to do when initially importing your database objects, by removing any DEFINER statements from the dump.
Changing the definer later is a more little tricky:
How to change the definer for views
Run this SQL to generate the necessary ALTER statements

SELECT CONCAT("ALTER DEFINER=`youruser`@`host` VIEW ", 
table_name, " AS ", view_definition, ";") 
FROM information_schema.views 
WHERE table_schema='your-database-name'; 
Copy and run the ALTER statements 
How to change the definer for stored procedures
Example:

UPDATE `mysql`.`proc` p SET definer = 'user@%' WHERE definer='root@%'

Be careful, because this will change all the definers for all databases.

2. Create the missing user
If you've found following error while using MySQL database:
The user specified as a definer ('someuser'@'%') does not exist`
Then you can solve it by using following :

GRANT ALL ON *.* TO 'someuser'@'%' IDENTIFIED BY 'complex-password';
FLUSH PRIVILEGES;

From http://www.lynnnayko.com/2010/07/mysql-user-specified-as-definer-root.html
This worked like a charm - you only have to change someuser to the name of the missing user. On a local dev server, you might typically just use root.
Also consider whether you actually need to grant the user ALL permissions or whether they could do with less.
____________________________________________________________________________________________________
## Conectar

mysql -u USER -h ADRESS -p


## Dump
mysqldump (é uma aplicação fora do mysql - não é pra entrar no mysql antes);

Exportar:
mysqldump -u [USERNAME] -p [DATABASE-NAME] > dumpfilename.sql

ex: mysqldump -u root -p ENTERPRISE > dumpQA-michelin-ads-12-12-2019.sql

Importar:
COMANDO TESTADO:
mysql -u USUARIO -p SENHA DATABASE < FILE.sql

https://chartio.com/resources/tutorials/importing-from-and-exporting-to-files-using-the-mysql-command-line/

____________________________________________________________________________________________________
## Solução de Problemas:

Error related to only_full_group_by when executing a query in MySql:
https://stackoverflow.com/a/35729681/10291492

You can try to disable the only_full_group_by setting by executing the following:

set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
set session sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';


____________________________________________________________________________________________________
## Exemplo de Query com Inner Join:

select ds_login, nm_person, nm_surname, nm_role from person, person_application_role, application_role
	where person.id_person = person_application_role.id_person
	and person_application_role.id_application_role = application_role.id_application_role
	and application_role.id_application_role in (88,89,90,94,95,96,97,103,122)
ORDER BY 2

____________________________________________________________________________________________________
## MySQL on CentOS 7

start:
sudo systemctl start mysqld

shutdown:
mysqld
_______________________________________________________________________________
## MySQL - How to temporarily disable a foreign key constraint?

To disable foreign key constraints when you want to truncate a table:
SET FOREIGN_KEY_CHECKS=0;

and remember to enable it when you’re done:
SET FOREIGN_KEY_CHECKS=1;
_______________________________________________________________________________
## FAZER DISTINÇÃO DE ACENTOS NA QUERY
COLLATE utf8_bin

EXEMPLO:
select COUNT(*) from dock_trip where id_dock_cargo_type = 'EXPORTAÇÃO' COLLATE utf8_bin
_______________________________________________________________________________
## Abrir acesso externo:
iptables -I INPUT -i eth0 -p tcp --destination-port 3306 -j ACCEPT

E descomentar a linha bind-address = 127.0.0.1 em:
/etc/mysql/mysql.conf.d/mysqld.cnf

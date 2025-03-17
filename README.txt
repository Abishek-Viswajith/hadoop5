$HADOOP_HOME/sbin/start-dfs.sh
$HADOOP_HOME/sbin/start-yarn.sh

pig -x mapreduce

echo -e "1,John,45000\n2,Alice,55000\n3,Bob,60000\n4,Mary,70000" > employees.txt

hdfs dfs -mkdir /pigdata
hdfs dfs -put employees.txt /pigdata/

employees = LOAD '/pigdata/employees.txt' USING PigStorage(',') 
            AS (id:int, name:chararray, salary:int);

high_salary = FILTER employees BY salary > 50000;

DUMP high_salary;

quit;

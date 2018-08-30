# spark.apache.connection.jdbc-postgresql

Solución conectar spark apache 2.3.1 con jdbc driver postgresql

# Fuente en python (etl.py):

from pyspark.shell import spark 
from pyspark.sql.functions import *
from pyspark.sql import SparkSession
spark2 = SparkSession.builder.getOrCreate()
url = 'jdbc:postgresql://url:5432/baseDatos'
properties = {'user': 'root', 'password': '1234567'}
df = spark2.read.jdbc(url=url, table='city', properties=properties)
df.show()

# Ejecucion 
python etl.py

# Error
/ __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.3.1
      /_/

Using Python version 2.7.12 (default, Dec  4 2017 14:50:18)
SparkSession available as 'spark'.
Traceback (most recent call last):
  File "etl.py", line 8, in <module>
    df = spark2.read.jdbc(url=url, table='city', properties=properties)
  File "/home/gonzalo/.local/lib/python2.7/site-packages/pyspark/sql/readwriter.py", line 527, in jdbc
    return self._df(self._jreader.jdbc(url, table, jprop))
  File "/home/gonzalo/.local/lib/python2.7/site-packages/py4j/java_gateway.py", line 1257, in __call__
    answer, self.gateway_client, self.target_id, self.name)
  File "/home/gonzalo/.local/lib/python2.7/site-packages/pyspark/sql/utils.py", line 63, in deco
    return f(*a, **kw)
  File "/home/gonzalo/.local/lib/python2.7/site-packages/py4j/protocol.py", line 328, in get_return_value
    format(target_id, ".", name), value)
py4j.protocol.Py4JJavaError: An error occurred while calling o32.jdbc.
: java.sql.SQLException: No suitable driver
	at java.sql.DriverManager.getDriver(DriverManager.java:315)
	at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions$$anonfun$7.apply(JDBCOptions.scala:85)
	at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions$$anonfun$7.apply(JDBCOptions.scala:85)
	at scala.Option.getOrElse(Option.scala:121)
	at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions.<init>(JDBCOptions.scala:84)
	at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions.<init>(JDBCOptions.scala:35)
	at org.apache.spark.sql.execution.datasources.jdbc.JdbcRelationProvider.createRelation(JdbcRelationProvider.scala:34)
	at org.apache.spark.sql.execution.datasources.DataSource.resolveRelation(DataSource.scala:340)
	at org.apache.spark.sql.DataFrameReader.loadV1Source(DataFrameReader.scala:239)
	at org.apache.spark.sql.DataFrameReader.load(DataFrameReader.scala:227)
	at org.apache.spark.sql.DataFrameReader.load(DataFrameReader.scala:164)
	at org.apache.spark.sql.DataFrameReader.jdbc(DataFrameReader.scala:254)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at py4j.reflection.MethodInvoker.invoke(MethodInvoker.java:244)
	at py4j.reflection.ReflectionEngine.invoke(ReflectionEngine.java:357)
	at py4j.Gateway.invoke(Gateway.java:282)
	at py4j.commands.AbstractCommand.invokeMethod(AbstractCommand.java:132)
	at py4j.commands.CallCommand.execute(CallCommand.java:79)
	at py4j.GatewayConnection.run(GatewayConnection.java:238)
	at java.lang.Thread.run(Thread.java:748)
  
  # =========== Solución ==================
  # Descargar el driver de Postgres
  https://jdbc.postgresql.org/download.html 
  En mi caso descargue: https://jdbc.postgresql.org/download/postgresql-42.2.5.jar 
  # Copiar el driver a la ruta /.local/lib/python2.7/site-packages/pyspark/jars
  cp /home/gonzalo/.local/lib/postgresql-42.2.5.jar /home/gonzalo/.local/lib/python2.7/site-packages/pyspark/jars/
 
  # Configuracion de mi maquina
  Ubuntu 16.04 LTS
  Python 2.7.12
  Spark 2.3.1

  




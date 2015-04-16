<head><title>Extensions : RDBMS Datatypes</title></head>

## Extensions : RDBMS Datastore Types
![Plugin](../images/nucleus_plugin.gif)

When persisting a class to an RDBMS datastore there is a _mapping_ process from class/field to table/column. The most common thing to configure is the 
[JavaTypeMapping](rdbms_java_types.html) so that it maps between the java type and the column(s) in the desired way. We can also configure the 
mapping to the datastore type. So, for example, we define what JDBC types we can map a particular Java type to. 
By "datastore type" we mean the JDBC type, such as BLOB, INT, VARCHAR etc. 
DataNucleus provides datastore mappings for the vast majority of JDBC types and the handlings will almost always be adequate. 
What you could do though is make a particular Java type persistable using a different JDBC type if you wished.


### Interface

To define your own datastore mapping you need to implement _org.datanucleus.store.rdbms.mapping.datastore.DatastoreMapping_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/mapping/datastore/DatastoreMapping.html).

So you can define how to convert the datastore column value to/from common Java types.
Look at [datanucleus-rdbms](https://github.com/datanucleus/datanucleus-rdbms/tree/master/src/main/java/org/datanucleus/store/rdbms/mapping/datastore)
for examples. Note that your XXXDatastoreMapping should have a single constructor taking in 
_JavaTypeMapping mapping, StoreManager storeMgr, Column col_.


### Plugin Specification

To give an example of what the plugin specification looks like

	<?xml version="1.0"?>
	<plugin id="mydomain.myplugins" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store.rdbms.datastore_mapping">
        	<mapping java-type="java.lang.Character" rdbms-mapping-class="org.datanucleus.store.rdbms.mapping.datastore.CharRDBMSMapping" 
                jdbc-type="CHAR" sql-type="CHAR" default="true"/>
        	<mapping java-type="java.lang.Character" rdbms-mapping-class="org.datanucleus.store.rdbms.mapping.datastore.IntegerRDBMSMapping" 
                jdbc-type="INTEGER" sql-type="INT" default="false"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).
So in this definition we have defined that a field of type "Character" can be mapped to JDBC type of CHAR or INTEGER (with CHAR the default)


### Missing JDBC definitions

While DataNucleus attempts to provide all useful JDBC type mappings out of the box, occasionally
you may come across one that is not defined, even though we provide an RDBMSMapping class for that
JDBC type. For example, if you got this error message

	JDBC type XXX declared for field "org.datanucleus.test.MyClass.yyyField" of java type java.lang.YYY cant be mapped for this datastore.


This means that while the JDBC driver may support this JDBC type, we haven't defined it for the java type "YYY" in the _plugin.xml_ file. 
All that you need to do is add a _plugin.xml_

	<?xml version="1.0"?>
	<plugin id="mydomain.myplugins" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store.rdbms.datastore_mapping">
        	<mapping java-type="java.lang.YYY" rdbms-mapping-class="org.datanucleus.store.rdbms.mapping.XXXRDBMSMapping" 
            	jdbc-type="XXX" sql-type="XXX" default="false"/>
    	</extension>
	</plugin>

together with MANIFEST.MF to your application, and it will be supported. Obviously if it is of
a type that you think we ought to support out-of-the-box then raise a JIRA on project "NUCRDBMS"
with your plugin.xml definition and mention the datastore it was needed for.

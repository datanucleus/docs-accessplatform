<head><title>Extensions : Connection Pooling</title></head>

## Extensions : Connection Pooling
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the pooling of connections to RDBMS datastores. 
DataNucleus provides a large selection of connection pools (DBCP, C3P0, Proxool, BoneCP) but is structured so that you can easily add your 
own variant and have it usable within your DataNucleus usage.

__Note that this plugin point replaces the earlier [RDBMS DataSource ConnectionPool](rdbms_datasource.html)__ (used up to and including 3.2.7 of datanucleus-rdbms).

DataNucleus requires a DataSource to define the datastore in use and consequently allows use of 
connection pooling. DataNucleus provides plugins for various different pooling products, shown below. 
You can easily define your own plugin for pooling. You can extend DataNucleus's capabilities 
using the plugin extension *org.datanucleus.store.rdbms.connectionpool*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>dbcp-builtin</td>
        <td>RDBMS connection pool, using Apache DBCP builtin</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>bonecp</td>
        <td>RDBMS connection pool, using BoneCP</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>c3p0</td>
        <td>RDBMS connection pool, using C3P0</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>dbcp</td>
        <td>RDBMS connection pool, using Apache DBCP</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>proxool</td>
        <td>RDBMS connection pool, using Proxool</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionpool</td>
        <td>tomcat</td>
        <td>RDBMS connection pool, using Tomcat pool</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>

The following sections describe how to create your own connection pooling plugin for DataNucleus.

### Interface

If you have your own DataSource connection pooling implementation you can easily use it with DataNucleus.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/connectionpool/ConnectionPoolFactory.html).
DataNucleus defines a ConnectionPoolFactory interface and you need to implement this.


	package org.datanucleus.store.rdbms.connectionpool;
	
	public interface ConnectionPoolFactory
	{
	    /**
    	 * Method to make a ConnectionPool for use within DataNucleus.
    	 * @param storeMgr StoreManager
    	 * @return The ConnectionPool
    	 * @throws Exception Thrown if an error occurs during creation
    	 */
    	public ConnectionPool createConnectionPool(StoreManager storeMgr);
	}

where you also define a ConnectionPool
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/connectionpool/ConnectionPool.html).


	package org.datanucleus.store.rdbms.connectionpool;
	
	public interface ConnectionPool
	{
	    /**
    	 * Method to call when closing the StoreManager down, and consequently to close the pool.
    	 */
    	void close();
	
    	/**
    	 * Accessor for the pooled DataSource.
    	 * @return The DataSource
    	 */
    	DataSource getDataSource();
	}

### Plugin Specification

The only thing required now is to register this plugin with DataNucleus when you start up your application.
To do this create a file _plugin.xml_ and put it in your JAR at the root of the CLASSPATH. It should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store.rdbms.connectionpool">
        	<connectionpool-factory name="mypool" class-name="mydomain.MyConnectionPoolFactory"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your new _ConnectionPoolFactory_ plugin. You do this by having your plugin in the CLASSPATH at runtime, 
and setting the persistence property __datanucleus.connectionPoolingType__ to _mypool_ (the name you specified in the plugin.xml file).


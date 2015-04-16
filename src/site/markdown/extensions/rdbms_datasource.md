<head><title>Extensions : Connection Pooling</title></head>

## Extensions : DataSource Connection pooling
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is 
pluggable is  the pooling of connections to RDBMS datastores. DataNucleus provides a large selection
of connection pools (DBCP, C3P0, Proxool, BoneCP) but is structured so that you can easily add your 
own variant and have it usable within your DataNucleus usage.

__Note that this plugin point is now discontinued and replaced by [RDBMS Connection Pooling](rdbms_connection_pool.html)__ (from 3.2.8 of datanucleus-rdbms).

DataNucleus requires a DataSource to define the datastore in use and consequently allows use of 
connection pooling. DataNucleus provides plugins for different pooling products - DBCP, C3P0, BoneCP, and
Proxool. You can easily define your own plugin for pooling. You can extend DataNucleus's capabilities 
using the plugin extension *org.datanucleus.store.rdbms.datasource*.


<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.datasource</td>
        <td>c3p0</td>
        <td>RDBMS connection pool, using C3P0</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.datasource</td>
        <td>dbcp</td>
        <td>RDBMS connection pool, using Apache DBCP</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.datasource</td>
        <td>proxool</td>
        <td>RDBMS connection pool, using Proxool</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.datasource</td>
        <td>bonecp</td>
        <td>RDBMS connection pool, using BoneCP</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>

The following sections describe how to create your own connection pooling plugin for DataNucleus.

### Interface

If you have your own DataSource connection pooling implementation you can easily use it with DataNucleus.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/3.1/org/datanucleus/store/rdbms/datasource/DataNucleusDataSourceFactory.html).
DataNucleus defines a DataSourceFactory interface and you need to implement this.


	package org.datanucleus.store.rdbms.datasource;
	
	public interface DataNucleusDataSourceFactory
	{
	    /**
    	 * Method to make a DataSource for use within DataNucleus.
    	 * @param storeMgr StoreManager
    	 * @return The DataSource
    	 * @throws Exception Thrown if an error occurs during creation
    	 */
    	public DataSource makePooledDataSource(StoreManager storeMgr);
	}


### Implementation

So let's suppose you have a library (_mydomain.MyPoolingClass_) that creates a DataSource that handles pooling. So you would do something like this

	package mydomain;

	import org.datanucleus.ClassLoaderResolver;
	import org.datanucleus.store.rdbms.datasource.DataNucleusDataSourceFactory;

	public class MyPoolingClassDataSourceFactory implements DataNucleusDataSourceFactory
	{
    	/**
    	 * Method to make a DataSource for use within DataNucleus.
    	 * @param storeMgr StoreManager
    	 * @return The DataSource
    	 * @throws Exception Thrown if an error occurs during creation
    	 */
    	public DataSource makePooledDataSource(StoreManager storeMgr)
    	{
        	PersistenceConfiguration conf = storeMgr.getNucleusContext().getPersistenceConfiguration();
        	String dbDriver = conf.getStringProperty("datanucleus.ConnectionDriverName");
        	String dbURL = conf.getStringProperty("datanucleus.ConnectionURL");
        	String dbUser = conf.getStringProperty("datanucleus.ConnectionUserName");
        	String dbPassword = conf.getStringProperty("datanucleus.ConnectionPassword");
        	ClassLoaderResolver clr = storeMgr.getNucleusContext().getClassLoaderResolver(null);
	
        	// Load the database driver
        	try
        	{
            	Class.forName(dbDriver);
        	}
        	catch (ClassNotFoundException cnfe)
        	{
            	try
            	{
                	clr.classForName(dbDriver);
            	}
            	catch (RuntimeException e)
            	{
                	// JDBC driver not found
                	throw new DatastoreDriverNotFoundException(dbDriver);
            	}
        	}

        	// Check the presence of "mydomain.MyPoolingClass"
        	try
        	{
            	Class.forName("mydomain.MyPoolingClass");
        	}
        	catch (ClassNotFoundException cnfe)
        	{
            	try
            	{
                	clr.classForName("mydomain.MyPoolingClass");
            	}
            	catch (RuntimeException e)
            	{
                	// "MyPoolingClass" library not found
            	    throw new DatastoreLibraryNotFoundException("MyPoolingClass", "MyPoolingClass");
            	}
        	}
	
        	// Create the Data Source for your pooling library
        	// Use the input driver, URL, user/password
        	// Use the input configuration file as required
        	DataSource ds = new mydomain.MyPoolingClass(...);
	
        	return ds;
    	}
	}

### Plugin Specification

The only thing required now is to register this plugin with DataNucleus when you start up your application.
To do this create a file _plugin.xml_ and put it in your JAR at the root of the CLASSPATH. 
It should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store.rdbms.datasource">
        	<datasource-factory name="MyPoolingClass" class-name="mydomain.MyPoolingClassDataSourceFactory"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your new _DataNucleusDataSourceFactory_ plugin. You do 
this by having your plugin in the CLASSPATH at runtime, and setting the persistence property 
__datanucleus.connectionPoolingType__ to _MyPoolingClass_ (the name you specified in the plugin.xml file).

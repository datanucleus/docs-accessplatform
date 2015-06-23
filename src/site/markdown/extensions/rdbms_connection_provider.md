<head><title>Extensions : Connection Provider</title></head>

## Extensions : Connection Provider
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is 
pluggable is the failover mechanism. DataNucleus provides a support for basic failover algorithm, 
and is structured so that you can easily add your own failover algorithm and have them usable within 
your DataNucleus usage.

Failover algorithm for DataNucleus can be plugged using the plugin extension *org.datanucleus.store.rdbms.connectionprovider*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store.rdbms.connectionprovider</td>
        <td>PriorityList</td>
        <td>Ordered List Algorithm</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>

### Interface

Any Connection Provider plugin will need to implement _org.datanucleus.store.rdbms.ConnectionProvider_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/ConnectionProvider.html).
So you need to implement the following interface

	package org.datanucleus.store.rdbms;
	
	import java.sql.Connection;
	import java.sql.SQLException;
	
	import javax.sql.DataSource;
	
	/**
	 * Connects to a DataSource to obtain a Connection.
	 * The ConnectionProvider is not a caching and neither connection pooling mechanism.
	 * The ConnectionProvider exists to perform failover algorithm on multiple DataSources
	 * when necessary.
	 * One instance per StoreManager (RDBMSManager) is created.
	 * Users can provide their own implementation via the extension org.datanucleus.store_connectionprovider 
	 */
	public interface ConnectionProvider
	{
    	/**
    	 * Flag if an error causes the operation to thrown an exception, or false to skip to next DataSource. 
    	 * If an error occurs on the last DataSource on the list an Exception will be thrown no matter if 
    	 * failOnError is true or false. This is a hint. 
    	 * Implementations may ignore the user setting and force it's own behaviour
    	 * @param flag true if to fail on error
    	 */
    	void setFailOnError(boolean flag);
    	
    	/**
    	 * Obtain a connection from the datasources, starting on the first
    	 * datasource, and if unable to obtain a connection skips to the next one on the list, and try again 
    	 * until the list is exhausted.
    	 * @param ds the array of datasources. An ordered list of datasources
    	 * @return the Connection, null if <code>ds</code> is null, or null if the DataSources has returned 
    	 *              a null as connection
    	 * @throws SQLException in case of error and failOnError is true or the error occurs while obtaining 
    	 *              a connection with the last
    	 * DataSource on the list
	     */
    	Connection getConnection(DataSource[] ds) throws SQLException;
	}

### Plugin Specification

So we now have our custom "Connection Provider" and we just need to make this into a DataNucleus 
plugin. To do this you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain.connectionprovider" name="My DataNucleus plug-in" provider-name="MyCompany">
    	<extension point="org.datanucleus.store.rdbms.connectionprovider">
       		<connection-provider class-name="mydomain.MyConnectionProvider" name="MyName"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

So here we have our "ConnectionProvider" class "MyConnectionProvider" which is named "MyName". 
When constructing the PersistenceManagerFactory, add the setting 
_datanucleus.rdbms.connectionProviderName=MyName_.

### Lifecycle

The _ConnectionProvider_ instance is created when the RBMSManager is instantiated and hold as hard reference during the lifecycle of the RDBMSManager.

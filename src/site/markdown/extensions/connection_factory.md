<head><title>Extensions : Connection Factory</title></head>

## Extensions : Connection Factory
![Plugin](../images/nucleus_plugin.gif)

Any plugin for a datastore needs a way of connecting to the datastore and linking these connections into the persistence process. This is provided by way of a ConnectionFactory.
This is pluggable so you can define your own and register it for the datastore, and you use the plugin extension *org.datanucleus.store_connectionfactory*.
This plugin point is intended to be implemented by provider of the datastore plugin.
Below are two samples that provide this, but you will find at least the transactional one for any datastore plugin.


<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Name</th>
        <th>Datastore</th>
        <th>Transactional</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store_connectionfactory</td>
        <td>rdbms/tx</td>
        <td>rdbms</td>
        <td>true</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_connectionfactory</td>
        <td>rdbms/nontx</td>
        <td>rdbms</td>
        <td>false</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>


### Interface

Any Connection Factory plugin will need to implement _org.datanucleus.store.connection.ConnectionFactory_.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/connection/ConnectionFactory.html)
So you need to implement the following interface


    public interface ConnectionFactory
    {
        /**
         * Obtain a connection from the Factory. 
         * The connection will be enlisted within the {@link org.datanucleus.Transaction} associated to the ExecutionContext if "enlist" is set to true.
         * @param om the ObjectManager
         * @param options Any options for then creating the connection
         * @return the ManagedConnection
         */
        ManagedConnection getConnection(ExecutionContext ec, org.datanucleus.Transaction transaction, Map options);
    
        /**
         * Create the ManagedConnection. Only used by ConnectionManager so do not call this.
         * @param ec ExecutionContext (if any)
         * @param transactionOptions the Transaction options this connection will be enlisted to, null if non existent
         * @return The ManagedConnection.
         */
        ManagedConnection createManagedConnection(ExecutionContext ec, Map transactionOptions);
    }


### Plugin Specification

So we now have our custom "Connection Factory" and we just need to make this into a DataNucleus plugin. 
To do this you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain.connectionfactory" name="My DataNucleus plug-in" provider-name="MyCompany">
    	<extension point="org.datanucleus.store_connectionfactory">
        	<connectionfactory name="rdbms/tx" class-name="org.datanucleus.store.rdbms.ConnectionFactoryImpl"
            	transactional="true" datastore="rdbms"/>
        	<connectionfactory name="rdbms/nontx" class-name="org.datanucleus.store.rdbms.ConnectionFactoryImpl"
            	transactional="false" datastore="rdbms"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

So now for the datastore "rdbms" we will use this implementation when transactional or non-transactional

### Lifecycle

The _ConnectionFactory_ instance(s) are created when the StoreManager is instantiated 
and held as hard references during the lifecycle of the StoreManager.

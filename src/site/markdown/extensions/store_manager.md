<head><title>Extensions : Store Manager</title></head>

## Extensions : Store Manager
![Plugin](../images/nucleus_plugin.gif)

DataNucleus provides support for persisting objects to particular datastores. It provides this capability via a "Store Manager". 
It provides a Store Manager plugin for many datastores (see below). You can extend DataNucleus's capabilities using the plugin extension 
*org.datanucleus.store_manager*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>URL-key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>rdbms</td>
        <td>jdbc</td>
        <td>Store Manager for RDBMS datastores</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>db4o</td>
        <td>db4o</td>
        <td>Store Manager for DB4O datastore</td>
        <td>datanucleus-db4o</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>neodatis</td>
        <td>neodatis</td>
        <td>Store Manager for NeoDatis datastores</td>
        <td>datanucleus-neodatis</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>ldap</td>
        <td>ldap</td>
        <td>Store Manager for LDAP datastores</td>
        <td>datanucleus-ldap</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>excel</td>
        <td>Store Manager for Excel documents</td>
        <td>excel</td>
        <td>datanucleus-excel</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>odf</td>
        <td>odf</td>
        <td>Store Manager for ODF datastores</td>
        <td>datanucleus-odf</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>xml</td>
        <td>xml</td>
        <td>Store Manager for XML datastores</td>
        <td>datanucleus-xml</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>json</td>
        <td>json</td>
        <td>Store Manager for JSON datastores</td>
        <td>datanucleus-json</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>amazons3</td>
        <td>amazons3</td>
        <td>Store Manager for Amazon S3 datastore</td>
        <td>datanucleus-json</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>googlestorage</td>
        <td>googlestorage</td>
        <td>Store Manager for Google Storage datastore</td>
        <td>datanucleus-json</td>
    </tr>                
    <tr>
        <td>org.datanucleus.store_manager</td>
        <td>hbase</td>
        <td>hbase</td>
        <td>Store Manager for HBase datastores</td>
        <td>datanucleus-hbase</td>
    </tr>
</table>

### Interface

If you want to implement support for another datastore you can achieve it by implementating the StoreManager interface.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/StoreManager.html).

For a brief guide on starting support for a new datastore, follow this
[HOWTO](http://www.datanucleus.org/documentation/development/new_store_plugin_howto.html).

### Plugin Specification

Once you have this implementation you then need to make the class available as a DataNucleus plugin. You do this by putting a file 
_plugin.xml_ in your JAR at the root of the CLASSPATH. The file _plugin.xml_ will look like this

	<?xml version="1.0"?>
	<plugin id="mydomain.mystore" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store_manager">
        	<store-manager class-name="mydomain.MyStoreManager" url-key="mykey" key="mykey"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your StoreManager. To do this you simply define your ConnectionURL to start with the _mykey_ defined in the plugin spec 
(for example, with the RDBMS plugin, the connection URL starts _jdbc:db-name:..._. This will select your store manager based on that.

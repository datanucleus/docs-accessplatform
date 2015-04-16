<head><title>Extensions : JTA Locator</title></head>

## Extensions : JTA Locator
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is 
the locator for JTA TransactionManagers (since J2EE doesnt define a standard mechanism for location). 
DataNucleus provides several plugins for the principal application servers available but is structured 
so that you can easily add your own variant and have it usable within your DataNucleus usage.

Locators for JTA TransactionManagers can be plugged using the plugin extension *org.datanucleus.jta_locator*.
These are of relevance when running with JTA transaction and linking in to the JTA transaction of some controlling application server.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>jboss</td>
        <td>JBoss</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>jonas</td>
        <td>JOnAS</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>jotm</td>
        <td>JOTM</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>oc4j</td>
        <td>OC4J</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>orion</td>
        <td>Orion</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>resin</td>
        <td>Resin</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>sap</td>
        <td>SAP app server</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>sun</td>
        <td>Sun ONE app server</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>weblogic</td>
        <td>WebLogic app server</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>websphere</td>
        <td>WebSphere 4/5</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>custom_jndi</td>
        <td>Custom JNDI</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.jta_locator</td>
        <td>atomikos</td>
        <td>Atomikos</td>
        <td>datanucleus-core</td>
    </tr>
</table>

The following sections describe how to create your own JTA Locator plugin for DataNucleus.

### Interface

If you have your own JTA Locator you can easily use it with DataNucleus. DataNucleus defines a TransactionManagerLocator interface and you need to implement this.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/jta/TransactionManagerLocator.html).


    package org.datanucleus.jta;
    
    import javax.transaction.TransactionManager;
    import org.datanucleus.ClassLoaderResolver;
    
    public interface TransactionManagerLocator
    {
        /**
         * Method to return the TransactionManager.
         * @param clr ClassLoader resolver
         * @return The TransactionManager
         */
        TransactionManager getTransactionManager(ClassLoaderResolver clr);
    }

So you need to create a class, __MyTransactionManagerLocator__ for example, that implements this interface.


### Plugin Specification

Once you have this implementation you then need to make the class available as a DataNucleus plugin.
You do this by putting a file _plugin.xml_ in your JAR at the root of the CLASSPATH. The file _plugin.xml_ will look like this


	<?xml version="1.0"?>
	<plugin id="mydomain.mylocator" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.jta_locator">
        	<jta_locator name="MyLocator" class-name="mydomain.MyTransactionManagerLocator"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your JTA Locator plugin. To do this you specify the persistence property _datanucleus.jtaLocator_ 
as __MyLocator__ (the "name" in _plugin.xml_).

<head><title>Extensions : AutoStart Mechanism</title></head>

## Extensions : AutoStart Mechanism
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the mechanism for starting with 
knowledge of previously persisted classes. DataNucleus provides 3 "auto-start" mechanisms, but also allows you to plugin your own variant.

DataNucleus can discover the classes that it is managing at runtime, or you can use an "autostart" mechanism
to inform DataNucleus of what classes it will be managing. DataNucleus provides a selection of plugins for autostart mechanism.
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.autostart*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.autostart</td>
        <td>classes</td>
        <td>AutoStart mechanism specifying the list of classes to be managed</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.autostart</td>
        <td>xml</td>
        <td>AutoStart mechanism using an XML file to store the managed classes</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.autostart</td>
        <td>schematable</td>
        <td>AutoStart mechanism using a table in the RDBMS datastore to store the managed classes</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>

### Interface

Any auto-start mechanism plugin will need to implement <i>org.datanucleus.store.AutoStartMechanism</i>.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/autostart/AutoStartMechanism.html)
So you need to implement the following interface

	package org.datanucleus.store.autostart;
	
	public interface AutoStartMechanism
	{
	    /** mechanism is disabled if None **/ 
	    public static final String NONE = "None";
	
	    /** mechanism is in Quiet mode **/
	    public static final String MODE_QUIET = "Quiet";
	
	    /** mechanism is in Checked mode **/
	    public static final String MODE_CHECKED = "Checked";
	
	    /** mechanism is in Ignored mode **/
	    public static final String MODE_IGNORED = "Ignored";
	
	    /**
	     * Accessor for the mode of operation.
	     * @return The mode of operation
	     **/
	    String getMode();
	
	    /**
	     * Mutator for the mode of operation.
	     * @param mode The mode of operation
	     **/
	    void setMode(String mode);
	
	    /**
	     * Accessor for the data for the classes that are currently auto started.
	     * @return Collection of {@link StoreData} elements
	     * @throws DatastoreInitialisationException
	     **/
	    Collection getAllClassData() throws DatastoreInitialisationException;
	
	    /**
	     * Starts a transaction for writing (add/delete) classes to the auto start mechanism.
	     */
	    void open();
	
	    /**
	     * Closes a transaction for writing (add/delete) classes to the auto start mechanism.
	     */
	    void close();
	
	    /**
	     * Whether it's open for writing (add/delete) classes to the auto start mechanism.
	     * @return whether this is open for writing 
	     */
	    public boolean isOpen();
	
	    /**
 	     * Method to add a class/field (with its data) to the currently-supported list.
	     * @param data The data for the class.
	     **/
	    void addClass(StoreData data);
	
	    /**
	     * Method to delete a class/field that is currently listed as supported in
	     * the internal storage.
	     * It does not drop the schema of the DatastoreClass 
	     * neither the contents of it. It only removes the class from the 
	     * AutoStart mechanism.
	     * TODO Rename this method to allow for deleting fields
	     * @param name The name of the class/field
	     **/
	    void deleteClass(String name);
	
	    /**
	     * Method to delete all classes that are currently listed as supported in
	     * the internal storage. It does not drop the schema of the DatastoreClass 
	     * neither the contents of it. It only removes the classes from the 
	     * AutoStart mechanism.
	     **/
	    void deleteAllClasses();
	
	    /**
	     * Utility to return a description of the storage for this mechanism.
	     * @return The storage description.
	     **/
	    String getStorageDescription();
	}

You can extend _org.datanucleus.store.AbstractAutoStartMechanism_.


### Implementation

So lets assume that you want to create your own auto-starter __MyAutoStarter__.

	package mydomain;
	
	import org.datanucleus.store.AutoStartMechanism;
	import org.datanucleus.store.AbstractAutoStartMechanism;
	import org.datanucleus.store.StoreManager;
	import org.datanucleus.ClassLoaderResolver;
	
	public class MyAutoStarter extends AbstractAutoStartMechanism
	{
    	public MyAutoStarter(StoreManager storeMgr, ClassLoaderResolver clr)
	    {
    	    super();
	    }

	    ... (implement the required methods)
	}


### Plugin Specification

When we have defined our "AutoStartMechanism" we just need to make it into a DataNucleus plugin. 
To do this you simply add a file <i>plugin.xml</i> to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
	    <extension point="org.datanucleus.autostart">
	        <autostart name="myStarter" class-name="mydomain.MyAutoStarter"/>
	    </extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).


### Plugin Usage

The only thing remaining is to use your new _AutoStartMechanism_ plugin. You do this by having your plugin in the CLASSPATH at runtime, 
and setting the PMF property __org.datanucleus.autoStartMechanism__ to _myStarter_ (the name you specified in the plugin.xml file).

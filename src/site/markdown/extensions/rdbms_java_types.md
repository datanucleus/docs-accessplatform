<head><title>Extensions : User-Defined Data Types</title></head>

## Extensions : RDBMS Java Types
![Plugin](../images/nucleus_plugin.gif)

When persisting a class to an RDBMS datastore there is a _mapping_ process from class/field
to table/column. DataNucleus provides a mapping process and allows users to define their own
mappings where required. Each field is of a particular type, and where a field is to be persisted
as a second-class object you need to define a mapping. DataNucleus defines mappings for all of 
the required JDO/JPA types but you want to persist some of your own types as second-class objects.
This extension is not required for other datastores, just RDBMS.

A _type mapping_ defines the way to convert between an object of the Java type and its datastore representation 
(namely a column or columns in a datastore table). There are 2 types of Java types that can be mapped. These are 
__mutable__ (something that can be updated, such as java.util.Date) and __immutable__ (something that is fixed from 
the point of construction, such as java.awt.Color).

__This guide relates to current DataNucleus GitHub. Please consult the DataNucleus source code if you are using an earlier version since things may be different in other versions__.


### Java type to single column mapping

In DataNucleus version 4.0+ creating a Mapping class is now optional and as long as you have defined a
TypeConverter then this will be mapped automatically using _TypeConverterMapping_. For 99.9% of types it should not be necessary to define a 
JavaTypeMapping as well.
If you really want to add a mapping then just create an implementation of JavaTypeMapping, extending SingleFieldMapping (find an example in GitHub and copy it).


### Java Type to multiple column mapping

As we mentioned in the [[Type Converter Guide](type_converter.html)] you can provide a multi-column TypeConverter for this situation.
You only need a JavaTypeMapping if you want to provide for RDBMS method invocation. The guide below shows you how to do write this mapping.

The simplest way to describe how to define your own mapping is to give an example. Here we'll use the example of the Java AWT class _Color_. 
This has 3 colour components (red, green, and blue) as well as an alpha component. So here we want to map the Java type 
java.awt.Color to 4 datastore columns - one for each of the red, green, blue, and alpha components of the colour. 
To do this we define a mapping class extending the DataNucleus class _org.datanucleus.store.rdbms.mapping.java.SingleFieldMultiMapping_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/mapping/java/SingleFieldMultiMapping.html).


	package org.mydomain;
	
	import org.datanucleus.PersistenceManager;
	import org.datanucleus.metadata.AbstractPropertyMetaData;
	import org.datanucleus.store.rdbms.mapping.java.SingleFieldMultiMapping;
	import org.datanucleus.store.rdbms.mapping.java.JavaTypeMapping;
	import org.datanucleus.store.rdbms.table.Table;
	
	public class ColorMapping extends SingleFieldMultiMapping
	{
    	/**
	     * Initialize this JavaTypeMapping with the given DatastoreAdapter for the given FieldMetaData.
    	 * @param dba The Datastore Adapter that this Mapping should use.
    	 * @param fmd FieldMetaData for the field to be mapped (if any)
    	 * @param table The table holding this mapping
    	 * @param clr the ClassLoaderResolver
    	 */
    	public void initialize(AbstractMemberMetaData mmd, Table table, ClassLoaderResolver clr)
    	{
			super.initialize(mmd, table, clr);
	
     	   	addDatastoreField(ClassNameConstants.INT); // Red
        	addDatastoreField(ClassNameConstants.INT); // Green
        	addDatastoreField(ClassNameConstants.INT); // Blue
        	addDatastoreField(ClassNameConstants.INT); // Alpha
    	}
	
    	public Class getJavaType()
    	{
        	return Color.class;
    	}
	
    	public Object getSampleValue(ClassLoaderResolver clr)
    	{
        	return java.awt.Color.red;
    	}
	
    	public void setObject(ExecutionContext ec, PreparedStatement ps, int[] exprIndex, Object value)
    	{
        	Color color = (Color) value;
        	if (color == null)
        	{
            	getDataStoreMapping(0).setObject(ps, exprIndex[0], null);
            	getDataStoreMapping(1).setObject(ps, exprIndex[1], null);
            	getDataStoreMapping(2).setObject(ps, exprIndex[2], null);
            	getDataStoreMapping(3).setObject(ps, exprIndex[3], null);
        	}
        	else
        	{
            	getDataStoreMapping(0).setInt(ps, exprIndex[0], color.getRed());
            	getDataStoreMapping(1).setInt(ps, exprIndex[1], color.getGreen());
            	getDataStoreMapping(2).setInt(ps, exprIndex[2], color.getBlue());
            	getDataStoreMapping(3).setInt(ps, exprIndex[3], color.getAlpha());
        	}
    	}
	
    	public Object getObject(ExecutionContext ec, ResultSet rs, int[] exprIndex)
    	{
        	try
        	{
            	// Check for null entries
            	if (((ResultSet)rs).getObject(exprIndex[0]) == null)
            	{
            	    return null;
            	}
        	}
        	catch (Exception e)
        	{
            	// Do nothing
        	}
	
        	int red = getDataStoreMapping(0).getInt(rs, exprIndex[0]); 
        	int green = getDataStoreMapping(1).getInt(rs, exprIndex[1]); 
        	int blue = getDataStoreMapping(2).getInt(rs, exprIndex[2]); 
        	int alpha = getDataStoreMapping(3).getInt(rs, exprIndex[3]);
        	return new Color(red,green,blue,alpha);
    	}
	}

In the initialize() method we've created 4 columns - one for each of the red, green, blue, 
alpha components of the colour. The argument passed in when constructing these columns is 
the Java type name of the column data being stored. The other 2 methods of relevance are 
the setObject() and getObject(). These have the task of mapping between the _Color_ 
object and its datastore representation (the 4 columns). That's all there is to it.

The only thing we need to do is enable use of this Java type when running DataNucleus. 
To do this we create a <i>plugin.xml</i> (at the root of our CLASSPATH) to contain our mappings.

	<?xml version="1.0"?>
	<plugin>
    	<extension point="org.datanucleus.store.rdbms.java_mapping">
        	<mapping java-type="java.awt.Color" mapping-class="org.mydomain.MyColorMapping"/>
    	</extension>
	</plugin>

Note that we also require a MANIFEST.MF file as per the [Extensions Guide](index.html).
When using the DataNucleus Enhancer, SchemaTool or runtime, DataNucleus automatically searches for the _mapping definition_ at __/plugin.xml__ files in the CLASSPATH.

Obviously, since DataNucleus already supports _java.awt.Color_ there is no need to add this particular mapping to DataNucleus yourself, 
but this demonstrates the way you should do it for any type you wish to add.

If your Java type that you want to map maps direct to a single column then you would instead extend org.datanucleus.store.mapping.SingleFieldMapping 
and wouldn't need to add the columns yourself. Look at [datanucleus-rdbms](https://github.com/datanucleus/datanucleus-rdbms/tree/master/src/main/java/org/datanucleus/store/rdbms/mapping/java)
for many examples of doing it this way.

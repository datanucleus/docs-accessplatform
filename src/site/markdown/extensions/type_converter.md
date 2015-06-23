<head><title>Extensions : Type Converter</title></head>

## Extensions : Type Converter
![Plugin](../images/nucleus_plugin.gif)

DataNucleus allows you to provide alternate ways of persisting Java types. Whilst it includes the majority of normal converters built-in, 
you can extend DataNucleus capabilities using the plugin extension *org.datanucleus.type_converter*.

__This guide relates to current DataNucleus GitHub. Please consult the DataNucleus source code if you are using an earlier version since things may be different in other versions__.


### TypeConverter Interface

Any type converter plugin will need to implement _org.datanucleus.store.types.converters.TypeConverter_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/types/converters/TypeConverter.html).
So you need to implement the following interface

	public interface TypeConverter<X, Y> extends Serializable
	{
    	/**
    	 * Method to convert the passed member value to the datastore type.
    	 * @param memberValue Value from the member
    	 * @return Value for the datastore
    	 */
    	Y toDatastoreType(X memberValue);
	
    	/**
    	 * Method to convert the passed datastore value to the member type.
    	 * @param datastoreValue Value from the datastore
    	 * @return Value for the member
    	 */
    	X toMemberType(Y datastoreValue);
	}


### TypeConverter Implementation Example

Let's take an example. If we look at the Java type URI we want to persist it as a String since a native URI type isn't present in datastores. We define our class as

	public class URIStringConverter implements TypeConverter<URI, String>
	{
    	public URI toMemberType(String str)
    	{
        	if (str == null)
        	{
            	return null;
        	}

        	return java.net.URI.create(str.trim());
    	}

    	public String toDatastoreType(URI uri)
    	{
        	return uri != null ? uri.toString() : null;
    	}
	}

So when converting it for the datastore it will use the _toString()_ form of the URI,
and will be converted back to a URI (on retrieval from the datastore) using the _URI.create_ method. 
Obviously this particular TypeConverter is included in DataNucleus, but hopefully it gives an idea of what to do to provide your own.

### Controlling default column length

Some datastore plugins may support schemas where you can put an upper limit on the length of columns (e.g RDBMS). You can build this information
into your TypeConverter plugin by also implementing the interface ColumnLengthDefiningTypeConverter
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/types/converters/ColumnLengthDefiningTypeConverter.html)
which simply means adding the method _int getDefaultColumnLength(int columnPosition)_.



### Converting a member to multiple columns

The default is to convert a member type to a single column type in the datastore. DataNucleus allows you to convert to multiple columns, for example imagine
a type Point that has an _x_ and _y_. You want to persist this into 2 columns, the _x_ stored in column 0, and the _y_ stored in column 1. So now you update your
TypeConverter to also implement MultiColumnConverter
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/types/converters/MultiColumnConverter.html)
which simply means adding the method _Class[] getDatastoreColumnTypes()_.



### Plugin Specification

So we now have our custom "value generator" and we just need to make this into a DataNucleus plugin. To do this you simply add a file 
_plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.type_converter">
        	<type-converter name="dn.uri-string" member-type="java.net.URI" datastore-type="java.lang.String"
            	converter-class="mydomain.converters.URIStringConverter"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

The name "dn.uri-string" can be used to refer to this converter from within a [java_types extension point](java_types.html) definition 
for the default converter to use for a Java type.

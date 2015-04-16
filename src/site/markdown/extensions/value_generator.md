<head><title>Extensions : Value Generators</title></head>

## Extensions : Value Generators
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is 
the generation of identity or field values. DataNucleus provides a [large selection](http://www.datanucleus.org/products/accessplatform/jdo/value_generation.html) 
of generators but is structured so that you can easily add your own variant and have it usable within your DataNucleus usage. Below are listed
some of those available, but each store plugin typically will define its own. The JDO/JPA specs define various that are required.
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.store_valuegenerator*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Datastore</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>auid</td>
        <td>all datastores</td>
        <td>Value Generator using AUIDs</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>uuid-hex</td>
        <td>all datastores</td>
        <td>Value Generator using uuid-hex</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>uuid-string</td>
        <td>all datastores</td>
        <td>Value Generator using uuid-string</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>timestamp</td>
        <td>all datastores</td>
        <td>Value Generator using Timestamp</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>timestamp-value</td>
        <td>all datastores</td>
        <td>Value Generator using Timestamp millisecs value</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>increment</td>
        <td>rdbms</td>
        <td>Value Generator using increment strategy</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>sequence</td>
        <td>rdbms</td>
        <td>Value Generator using datastore sequences</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>table-sequence</td>
        <td>rdbms</td>
        <td>Value Generator using a database table to generate sequences (same as increment)</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>max</td>
        <td>rdbms</td>
        <td>Value Generator using max(COL)+1 strategy</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>datastore-uuid-hex</td>
        <td>rdbms</td>
        <td>Value Generator using uuid-hex attributed by the datastore</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>identity</td>
        <td>mongodb</td>
        <td>Value Generator for MongoDB using identity strategy</td>
        <td>datanucleus-mongodb</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_valuegenerator</td>
        <td>identity</td>
        <td>neo4j</td>
        <td>Value Generator for Neo4j using identity strategy</td>
        <td>datanucleus-neo4j</td>
    </tr>
</table>

The following sections describe how to create your own value generator plugin for DataNucleus.

### Interface

Any value generator plugin will need to implement _org.datanucleus.store.valuegenerator.ValueGenerator_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/valuegenerator/ValueGenerator.html).
So you need to implement the following interface


	public interface ValueGenerator
	{
    	String getName ();
	
    	void allocate (int additional);
	
    	Object next ();
    	Object current ();
	
    	long nextValue();
    	long currentValue();
	}


### Implementation

DataNucleus provides an abstract base class _org.datanucleus.store.valuegenerator.AbstractValueGenerator_ to extend if you don't require 
datastore access. If you do require (RDBMS) datastore access for your ValueGenerator then you can extend _org.datanucleus.store.rdbms.valuegenerator.AbstractRDBMSValueGenerator_
Let's give an example, here we want a generator that provides a form of UUID identity. We define our class as

    package mydomain;
    
    import org.datanucleus.store.valuegenerator.ValueGenerationBlock;
    import org.datanucleus.store.valuegenerator.AbstractValueGenerator;
    
    public class MyUUIDValueGenerator extends AbstractValueGenerator
    {
        public MyUUIDValueGenerator(String name, Properties props)
        {
            super(name, props);
        }
    
        /**
         * Method to reserve "size" ValueGenerations to the ValueGenerationBlock.
         * @param size The block size
         * @return The reserved block
         */
        public ValueGenerationBlock reserveBlock(long size)
        {
            Object[] ids = new Object[(int) size];
            for (int i = 0; i < size; i++)
            {
                ids[i] = getIdentifier();
            }
            return new ValueGenerationBlock(ids);
        }
    
        /**
         * Create a UUID identifier.
         * @return The identifier
         */
        private String getIdentifier()
        {
            ... Write this method to generate the identifier
        }
    }

As show you need a constructor taking 2 arguments _String_ and _java.util.Properties_. The first being the name of the generator, and the 
second containing properties for use in the generator.

* __class-name__ Name of the class that the value is being added to
* __root-class-name__ Name of the root class in this inheritance tree
* __field-name__ Name of the field whose value is being set (not provided if this is datastore identity field)
* __catalog-name__ Catalog that objects of the class are stored in
* __schema-name__ Schema that objects of the class are stored in
* __table-name__ Name of the (root) table storing this field
* __column-name__ Name of the column storing this field
* __sequence-name__ Name of the sequence (if specified in the MetaData)


### Plugin Specification

So we now have our custom "value generator" and we just need to make this into a DataNucleus plugin. To do this
you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store_valuegenerator">
        	<valuegenerator name="myuuid" class-name="mydomain.MyUUIDValueGenerator" unique="true"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

The name "myuuid" is what you will use as the "strategy" when specifying to use it in MetaData.
The flag "unique" is only needed if your generator is to be unique across all requests. For example if your
generator was only unique for a particular class then you should omit that part. Thats all. You now have a 
DataNucleus "value generator" plugin.


### Plugin Usage

To use your value generator you would reference it in your JDO MetaData like this

	<class name="MyClass">
    	<datastore-identity strategy="myuuid"/>
    	...
	</class>

Don't forget that if you write a value generator that could be of value to others you could easily donate it to DataNucleus for inclusion in the next release.

<head><title>Extensions : Query Cache</title></head>

## Extensions : Query Cache
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the caching of query compilations. 
DataNucleus provides some inbuilt cache options, but also allows you to provide your own.

DataNucleus is able to support third party Query Cache products. 
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.cache_query*.


<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.cache_query</td>
        <td>weak</td>
        <td>Weak Query Cache (default)</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_query</td>
        <td>soft</td>
        <td>Soft Query Cache</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_query</td>
        <td>strong</td>
        <td>Strong Query Cache</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_query</td>
        <td>javax.cache</td>
        <td>javax.cache Query Cache</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_query</td>
        <td>cacheonix</td>
        <td>Cacheonix Query Cache</td>
        <td>datanucleus-cache</td>
    </tr>
</table>

The following sections describe how to create your own Query cache plugin for DataNucleus.

### Interface

If you have your own Query cache you can easily use it with DataNucleus. DataNucleus defines a QueryCache interface and you need to implement this.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/store/query/cache/QueryCache.html).


	package org.datanucleus.store.query.cache;
	public interface QueryCache
	{
    	void close();
    	void evict(String queryKey);
    	void clear();
    	boolean isEmpty();
    	int size();
    	CachedQuery get(String queryKey);
    	CachedQuery put(String queryKey, CachedQuery cachedQuery);
    	boolean contains(String queryKey);
	}

### Implementation

Let's suppose your want to implement your own Level 2 cache _MyLevel2Cache_

	package mydomain;
	
	import org.datanucleus.NucleusContext;
	import org.datanucleus.store.query.cache.QueryCache;
	
	public class MyQueryCache implements QueryCache
	{
	    public MyQueryCache(NucleusContext nucCtx)
    	{
        	...
    	}
	
    	... (implement the interface)
	}

### Plugin Specification

Once you have this implementation you then need to make the class available as a DataNucleus plugin. You do this by putting a file 
_plugin.xml_ in your JAR at the root of the CLASSPATH. The file _plugin.xml_ will look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.cache_query">
	    	<cache name="MyCache" class-name="mydomain.MyQueryCache"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your Query Cache plugin. To do this you specify the persistence property _datanucleus.cache.query.type_ as __MyCache__ (the "name" in _plugin.xml_).


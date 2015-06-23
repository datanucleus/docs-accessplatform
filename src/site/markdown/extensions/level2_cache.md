<head><title>Extensions : Level 2 Cache</title></head>

## Extensions : Level 2 Cache
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the Level 2 caching of objects 
(between PM/EMs for the same PMF/EMF). DataNucleus provides a large selection of Level 2 caches (builtin-map-based, Coherence, EHCache, OSCache, others) 
but is structured so that you can easily add your own variant and have it usable within your DataNucleus usage. 

DataNucleus is able to support third party Level 2 Cache products. There are provided plugins for EHCache, SwarmCache, OSCache, and others. 
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.cache_level2*.


<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>default</td>
        <td>Level 2 Cache (default)</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>soft</td>
        <td>Level 2 Cache using Soft maps</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>ehcache</td>
        <td>Level 2 Cache using EHCache</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>ehcacheclassbased</td>
        <td>Level 2 Cache using EHCache (based on classes)</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>jcache</td>
        <td>Level 2 Cache using JCache (early javax.cache)</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>javax.cache</td>
        <td>Level 2 Cache using javax.cache</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>cacheonix</td>
        <td>Level 2 Cache using Cacheonix</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>xmemcached</td>
        <td>Level 2 Cache using Xmemcached</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>spymemcached</td>
        <td>Level 2 Cache using Spymemcached</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>coherence</td>
        <td>Level 2 Cache using Oracle Coherence</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>oscache</td>
        <td>Level 2 Cache using OSCache</td>
        <td>datanucleus-cache</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level2</td>
        <td>swarmcache</td>
        <td>Level 2 Cache using SwarmCache</td>
        <td>datanucleus-cache</td>
    </tr>
</table>


The following sections describe how to create your own Level 2 cache plugin for DataNucleus.

### Interface

If you have your own Level2 cache you can easily use it with DataNucleus. DataNucleus defines a Level2Cache interface and you need to implement this.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/cache/Level2Cache.html).


	package org.datanucleus.cache;
	public interface Level2Cache
	{
    	void close();
	
    	void evict (Object oid);
    	void evictAll ();
    	void evictAll (Object[] oids);
    	void evictAll (Collection oids);
    	void evictAll (Class pcClass, boolean subclasses);

	    void pin (Object oid);
   	 	void pinAll (Collection oids);
    	void pinAll (Object[] oids);
    	void pinAll (Class pcClass, boolean subclasses);
	
    	void unpin(Object oid);
    	void unpinAll(Collection oids);
    	void unpinAll(Object[] oids);
    	void unpinAll(Class pcClass, boolean subclasses);
	
    	int getNumberOfPinnedObjects();
    	int getNumberOfUnpinnedObjects();
    	int getSize();
    	CachedPC get(Object oid);
    	CachedPC put(Object oid, CachedPC pc);
    	boolean isEmpty();
    	void clear();
    	boolean containsOid(Object oid);
	}


### Implementation

Let's suppose your want to implement your own Level 2 cache _MyLevel2Cache_


	package mydomain;
	
	import org.datanucleus.OMFContext;
	import org.datanucleus.cache.Level2Cache;
	
	public class MyLevel2Cache implements Level2Cache
	{
	    /**
	     * Constructor.
	     * @param omfCtx OMF Context
	     */
	    public MyLevel2Cache(OMFContext omfCtx)
	    {
	        ...
	    }
	
	    ... (implement the interface)
	}


### Plugin Specification

Once you have this implementation you then need to make the class available as a DataNucleus plugin. You do this by putting 
a file _plugin.xml_ in your JAR at the root of the CLASSPATH. The file _plugin.xml_ will look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.cache_level2">
        	<cache name="MyCache" class-name="mydomain.MyLevel2Cache"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).


### Plugin Usage

The only thing remaining is to use your L2 Cache plugin. To do this you specify the persistence property 
_datanucleus.cache.level2.type_ as __MyCache__ (the "name" in _plugin.xml_).

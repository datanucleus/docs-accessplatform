<head><title>Extensions : Level 1 Cache</title></head>

## Extensions : Level 1 Cache
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the Level 1 caching of objects 
(between PM/EMs for the same PMF/EMF). DataNucleus comes with builtin support for three Level 1 caches, but is a plugin point so that you can 
easily add your own variant and have it usable within your DataNucleus usage.

DataNucleus is able to support third party Level 1 Cache products. There are DataNucleus-provided plugins for
weak, soft referenced caches etc. You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.cache_level1*. 

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level1</td>
        <td>weak</td>
        <td>Weak referenced cache (default)</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level1</td>
        <td>soft</td>
        <td>Soft referenced cache</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.cache_level1</td>
        <td>strong</td>
        <td>Strong-referenced cache (HashMap)</td>
        <td>datanucleus-core</td>
    </tr>
</table>

The following sections describe how to create your own Level 1 cache plugin for DataNucleus.

### Interface

If you have your own Level1 cache you can easily use it with DataNucleus. DataNucleus defines a Level1Cache interface and you need to implement this.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/cache/Level1Cache.html).

	package org.datanucleus.cache;
	
	public interface Level1Cache extends Map
	{
	}

So you need to create a class, __MyLevel1Cache__ for example, that implements this interface (i.e that implements _java.util.Map_).


### Plugin Specification

Once you have this implementation you then need to make the class available as a DataNucleus plugin. You do this by putting a file 
_plugin.xml_ in your JAR at the root of the CLASSPATH. The file _plugin.xml_ will look like this

	<?xml version="1.0"?>
	<plugin id="mydomain.mycache" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.cache_level1">
	    	<cache name="MyCache" class-name="mydomain.MyLevel1Cache"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your L1 Cache plugin. To do this you specify the 
persistence property _datanucleus.cache.level1.type_ as __MyCache__ (the "name" in _plugin.xml_).


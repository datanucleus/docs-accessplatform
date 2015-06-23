<head><title>Extensions : Annotations</title></head>

## Extensions : Annotations
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is the reading of annotations. 
DataNucleus provides a support for JDO and JPA annotations, but is structured so that you can easily add your own annotations and have them
usable within your DataNucleus usage.

DataNucleus supports Java annotations. More than this, it actually provides a pluggable framework whereby you can plug in your own annotations support. 
The JDO API plugin provides support for JDO annotations, and the JPA API plugin provides support for JPA annotations. 
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.annotations*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.annotations</td>
        <td>@PersistenceCapable</td>
        <td>JDO annotation reader</td>
        <td>datanucleus-api-jdo</td>
    </tr>
    <tr>
        <td>org.datanucleus.annotations</td>
        <td>@PersistenceAware</td>
        <td>JDO annotation reader</td>
        <td>datanucleus-api-jdo</td>
    </tr>
    <tr>
        <td>org.datanucleus.annotations</td>
        <td>@Entity</td>
        <td>JPA annotation reader</td>
        <td>datanucleus-api-jpa</td>
    </tr>
    <tr>
        <td>org.datanucleus.annotations</td>
        <td>@MappedSuperclass</td>
        <td>JPA annotation reader</td>
        <td>datanucleus-api-jpa</td>
    </tr>
    <tr>
        <td>org.datanucleus.annotations</td>
        <td>@Embeddable</td>
        <td>JPA annotation reader</td>
        <td>datanucleus-api-jpa</td>
    </tr>
</table>


### Interface

Any annotation reader plugin will need to implement _org.datanucleus.metadata.annotations.AnnotationReader_.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/metadata/annotations/AnnotationReader.html)
So you need to implement the following interface

	package org.datanucleus.metadata.annotations;

	import org.datanucleus.metadata.PackageMetaData;
	import org.datanucleus.metadata.ClassMetaData;

	public interface AnnotationReader
	{
	    /**
	     * Accessor for the annotations packages supported by this reader.
	     * @return The annotations packages that will be processed.
	     */
	    String[] getSupportedAnnotationPackages();

	    /**
	     * Method to get the ClassMetaData for a class from its annotations.
	     * @param cls The class
	     * @param pmd MetaData for the owning package (that this will be a child of)
	     * @return The ClassMetaData (unpopulated and unitialised)
	     */
	    public ClassMetaData getMetaDataForClass(Class cls, PackageMetaData pmd);
	}

### Plugin Specification

So we now have our custom "annotation reader" and we just need to make this into a DataNucleus 
plugin. To do this you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain.annotations" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.annotations">
        	<annotations annotation-class="mydomain.annotations.MyAnnotationType" reader="mydomain.annotations.MyAnnotationReader"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

So here we have our "annotations reader" class "MyAnnotationReader" which will process any classes annotated with the annotation "MyAnnotationType".

<head><title>Extensions : Class Annotation Handler</title></head>

## Extensions : Class Annotation Handler
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is 
the handling of annotations at class-level. DataNucleus provides a support for JDO and JPA annotations, 
but is structured so that you can easily add your own annotations and have them usable within your DataNucleus usage.

### Interface

Any class annotation handler plugin will need to implement _org.datanucleus.metadata.annotations.ClassAnnotationHandler_.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/metadata/annotations/ClassAnnotationHandler.html).
So you need to implement the following interface

	package org.datanucleus.metadata.annotations;
	
	import org.datanucleus.ClassLoaderResolver;
	import org.datanucleus.metadata.AbstractClassMetaData;
	
	public interface ClassAnnotationHandler
	{
	    /**
	     * Method to process a class level annotation.
	     * @param annotation The annotation
	     * @param cmd Metadata for the class to update with any necessary information.
	     * @param clr ClassLoader resolver
	     */
	    void processClassAnnotation(AnnotationObject annotation, AbstractClassMetaData cmd, ClassLoaderResolver clr);
	}

### Plugin Specification

So we now have our custom "annotation handler" and we just need to make this into a DataNucleus 
plugin. To do this you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this


	<?xml version="1.0"?>
	<plugin id="mydomain.annotations" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.class_annotation_handler">
        	<class-annotation-handler annotation-class="mydomain.annotations.MyAnnotation" handler="mydomain.annotations.MyAnnotationHandler"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).
So here, when the metadata for our class is processed, if it finds the @MyAnnotation annotation
it will call this handler after generating the basic metadata for the class, allowing us to update it.

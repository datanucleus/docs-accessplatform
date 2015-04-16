<head><title>Extensions : Identity String Translators</title></head>

## Extensions : Identity String Translators
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is 
translation of identities. When you call <i>pm.getObjectById(id)</i> you pass in an object. This object
can be the toString() form of an identity. Some other JDO implementations (e.g Xcalia) allowed
non-standard String input here including a discriminator. This plugin point allows for such
non-standard String input forms to pm.getObjectById(id) and can provide a plugin that translates this 
String into a valid JDO identity. Alternatively you could do this in your own code, but the facility 
is provided. This means that in your application you only use your own form of identities.

You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.identity_string_translator*.

<table>
<tr>
  <th>Plugin extension-point</th>
  <th>Key</th>
  <th>Description</th>
  <th width="80">Location</th>
</tr>
<tr>
  <td>org.datanucleus.identity_string_translator</td>
  <td>xcalia</td>
  <td>Translator that allows for {discriminator}:key as well as the usual input, as supported by Xcalia XIC</td>
  <td>datanucleus-core</td>
</tr>
</table>

### Interface

Any identifier factory plugin will need to implement _org.datanucleus.store.IdentifierStringFactory_.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/identity/IdentityStringTranslator.html).
So you need to implement the following interface


	package org.datanucleus.identity;
	
	public interface IdentityStringTranslator
	{
    	/**
    	 * Method to translate the string into the identity.
    	 * @param om ObjectManager
    	 * @param stringId String form of the identity
    	 * @return The identity
    	 */
    	Object getIdentity(ObjectManager om, String stringId);
	}


### Plugin Specification

When we have defined our "IdentityStringTranslator" we just need to make it into a DataNucleus plugin. To do this you simply add a file 
_plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this


	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.identity_string_translator">
        	<identitystringtranslator name="mytranslator" class-name="mydomain.MyIdStringTranslator"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage


The only thing remaining is to use your new _IdentityStringTranslator_ plugin. You do this by having your plugin in the CLASSPATH at runtime, 
and setting the persistence property __datanucleus.identityStringTranslatorType__ to _mytranslator_ (the name you specified in the plugin.xml file).

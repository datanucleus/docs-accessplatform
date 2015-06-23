<head><title>Extensions : Datastore Identity</title></head>

## Extensions : Datastore Identity
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable is 
the class used to represent datastore-identity (for JDO). DataNucleus provides a default 
implementation for use and this generates identifiers (returned by _JDOHelper.getObjectId(obj)_) 
of the form "3286[OID]mydomain.MyClass". Having this component configurable means that you can
override the output of the toString() to be more suitable for any use of these identities. Please 
be aware that the JDO2 specification (5.4.3) has strict rules for datastore identity classes.

DataNucleus allows identities to be either datastore or application identity. When using datastore 
identity it needs to have a class to represent an identity. DataNucleus provides its own default 
datastore identity class. You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.store_datastoreidentity*.

<table>
<tr>
  <th>Plugin extension-point</th>
  <th>Key</th>
  <th>Description</th>
  <th width="80">Location</th>
</tr>
<tr>
  <td>org.datanucleus.store_datastoreidentity</td>
  <td>datanucleus</td>
  <td>Datastore Identity used by DataNucleus since DataNucleus 1.0 ("1[OID]org.datanucleus.myClass")</td>
  <td>datanucleus-core</td>
</tr>
<tr>
  <td>org.datanucleus.store_datastoreidentity</td>
  <td>kodo</td>
  <td>Datastore Identity in the style of OpenJPA/Kodo ("org.datanucleus.myClass-1")</td>
  <td>datanucleus-core</td>
</tr>
<tr>
  <td>org.datanucleus.store_datastoreidentity</td>
  <td>xcalia</td>
  <td>Datastore Identity in the style of Xcalia ("org.datanucleus.myClass:1"). This ignores Xcalias support for class aliases</td>
  <td>datanucleus-core</td>
</tr>
</table>


The following sections describe how to create your own datastore identity plugin for DataNucleus.

### Interface

Any datastore identity plugin will need to implement _org.datanucleus.identity.OID_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/identity/OID.html)
So you need to implement the following interface

    import org.datanucleus.identity;
    
    public interface OID
    {
        /**
         * Provides the OID in a form that can be used by the database as a key.
         * @return The key value
         */
        public abstract Object getKeyValue();

        /**
         * Accessor for the PC class name 
         * @return the PC Class
         */
        public abstract String getPcClass();
    
        /**
         * Equality operator.
         * @param obj Object to compare against
         * @return Whether they are equal
         */
        public abstract boolean equals(Object obj);
    
        /**
         * Accessor for the hashcode
         * @return Hashcode for this object
         */
        public abstract int hashCode();

        /**
         * Returns the string representation of the OID.
         * The string representation should contain enough information to be usable as input to a String constructor
         * to create the OID.
         * @return the string representation of the OID.
         */
        public abstract String toString();
    }


### Implementation

DataNucleus provides an abstract base class _org.datanucleus.identity.OIDImpl_ as a guideline.
The DataNucleus internal implementation is defined as


	package org.datanucleus.identity;
	
	public class OIDImpl implements java.io.Serializable, OID
	{
        /** Separator to use between fields. */
        private transient static final String oidSeparator = "[OID]";
    
        // JDO spec 5.4.3 - serializable fields required to be public.
    
        /** The key value. */
        public final Object oid;
    
        /** The PersistenceCapable class name */
        public final String pcClass;
    
        /** pre-created toString to improve performance **/ 
        public final String toString;
    
        /** pre-created hasCode to improve performance **/ 
        public final int hashCode;
    
        /**
        * Creates an OID with no value. Required by the JDO spec
        */
        public OIDImpl()
        {
            oid = null;
            pcClass = null; 
            toString = null;
            hashCode = -1;
        }
    
        /**
         * Create a string datastore identity.
         * @param pcClass The PersistenceCapable class that this represents
         * @param object The value
         */
        public OIDImpl(String pcClass, Object object)
        {
            this.pcClass = pcClass;
            this.oid = object;
    
            StringBuffer s = new StringBuffer();
            s.append(this.oid.toString());
            s.append(oidSeparator);
            s.append(this.pcClass);
            toString = s.toString();
            hashCode = toString.hashCode();        
        }
    
        /**
         * Constructs an OID from its string representation that is consistent with the output of toString().
         * @param str the string representation of an OID
         * @exception IllegalArgumentException if the given string representation is not valid.
         * @see #toString
         */
        public OIDImpl(String str)
        throws IllegalArgumentException
        {
            if (str.length() < 2)
            {
                throw new IllegalArgumentException(Localiser.msg("OID.InvalidValue", str));
            }
    
            int start = 0;
            int end = str.indexOf(oidSeparator, start);
            String oidStr = str.substring(start, end);
            Object oidValue = null;
            try
            {
                // Use Long if possible, else String
                oidValue = new Long(oidStr);
            }
            catch (NumberFormatException nfe)
            {
                oidValue = oidStr;
            }
            oid = oidValue;

            start = end + oidSeparator.length();
            this.pcClass = str.substring(start, str.length());
            
            toString = str;
            hashCode = toString.hashCode();
        }
    
        /**
         * Accessor for the key value.
         * @return The key value
         */
        public Object getKeyValue()
        {
            return oid;
        }

        /**
         * Accessor for the PersistenceCapable class name.
         * @return PC class name
         */
        public String getPcClass()
        {
            return pcClass;
        }
    
        /**
         * Equality operator.
         * @param obj Object to compare against
         * @return Whether they are equal
         */
        public boolean equals(Object obj)
        {
            if (obj == null)
            {
                return false;
            }
            if (obj == this)
            {
                return true;
            }
            if (!(obj.getClass().getName().equals(ClassNameConstants.OIDImpl)))
            {
                return false;
            }
            if (hashCode() != obj.hashCode())
            {
                return false;
            }
            return true;
        }
    
        /**
         * Accessor for the hashcode
         * @return Hashcode for this object
         */
        public int hashCode()
        {
            return hashCode;
        }
    
        /**
         * Creates a String representation of the datastore identity, formed from the PC class name
         * and the key value. This will be something like
         * <pre>3254[OID]mydomain.MyClass</pre>
         * @return The String form of the identity
         */
        public String toString()
        {
            return toString;
        }
    }


As show you need 3 constructors. One is the default constructor. One takes a String (which is the output
of the toString() method). The other takes the PC class name and the key value.


### Plugin Specification

So once we have our custom "datastore identity" we just need to make this into a DataNucleus plugin. To do this
you simply add a file _plugin.xml_ to your JAR at the root. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.store_datastoreidentity">
        	<datastoreidentity name="myoid" class-name="mydomain.MyOIDImpl" unique="true"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

The name "myoid" should be specified when you create the PersistenceManagerFactory using 
the persistence property name "org.datanucleus.datastoreIdentityType". Thats all. 
You now have a DataNucleus "datastore identity" plugin.

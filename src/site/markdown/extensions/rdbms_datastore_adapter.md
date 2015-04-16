<head><title>Extensions : RDBMS Adapters</title></head>

## Extensions : RDBMS Adapters
![Plugin](../images/nucleus_plugin.gif)

DataNucleus is developed as a plugin-driven framework and one of the components that is pluggable 
is the adapter for the datastore. The datastore adapter provides the translation between DataNucleus 
and the specifics of the RDBMS in use. DataNucleus provides support for a 
[large selection](http://www.datanucleus.org/products/accessplatform/datastores/rdbms.html)
of RDBMS but is structured so that you can easily add your own adapter for your RDBMS and have it usable within your DataNucleus usage.

DataNucleus supports many RDBMS databases, and by default, ALL RDBMS are supported without the need 
to extend DataNucleus. Due to incompabilities, or specifics of each RDBMS database, it's allowed to 
extend DataNucleus to make the support to a specific database fit better to DataNucleus and your needs.
The [RDBMS](http://www.datanucleus.org/products/accessplatform/datastores/rdbms.html) page lists 
all RDBMS databases that have been tested with DataNucleus, and some of these databases has been 
adapted internally to get a good fit. 
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.store_datastoreadapter*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>derby</td>
        <td>Adapter for Apache Derby/Cloudscape</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>db2</td>
        <td>Adapter for IBM DB2</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>as/400</td>
        <td>Adapter for IBM DB2 AS/400</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>firebird</td>
        <td>Adapter for Firebird/Interbase</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>microsoft</td>
        <td>Adapter for MSSQL server</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>h2</td>
        <td>Adapter for H2</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>hsql</td>
        <td>Adapter for HSQLDB</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>mckoi</td>
        <td>Adapter for McKoi DB</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>mysql</td>
        <td>Adapter for MySQL</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>sybase</td>
        <td>Adapter for Sybase</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>oracle</td>
        <td>Adapter for Oracle</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>pointbase</td>
        <td>Adapter for Pointbase</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>postgresql</td>
        <td>Adapter for PostgreSQL</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>sapdb</td>
        <td>Adapter for SAPDB/MaxDB</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>sqlite</td>
        <td>Adapter for SQLite</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>timesten</td>
        <td>Adapter for Timesten</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_datastoreadapter</td>
        <td>informix</td>
        <td>Adapter for Informix</td>
        <td>datanucleus-rdbms</td>
    </tr>
</table>

DataNucleus supports a very wide range of RDBMS datastores. It will typically auto-detect the datastore adapter to use
and select it. This, in general, will work well and the user will not need to change anything to benefit
from this behaviour. There are occasions however where a user may need to provide their own datastore adapter
and use that. For example if their RDBMS is a new version and something has changed relative to the previous
(supported) version, or where the auto-detection fails to identify the adapter since their RDBMS is not yet
on the supported list.

By default when you create a [PMF](http://www.datanucleus.org/products/accessplatform/jdo/pmf.html) to connect to a particular datastore DataNucleus will 
automatically detect the _datastore adapter_ to use and will use its own internal adapter for that type of datastore. 
The default behaviour is overridden using the persistence property __org.datanucleus.rdbms.datastoreAdapterClassName__, which specifies the class name 
of the datastore adapter class to use. This class must implement the DataNucleus interface _DatastoreAdapter_
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/store.rdbms/latest/org/datanucleus/store/rdbms/adapter/DatastoreAdapter.html).

So you need to implement _DatastoreAdapter_. You have 2 ways to go here. You can either start from scratch
(when writing a brand new adapter), or you can take the existing DataNucleus adapter for a particular RDBMS and change (or extend)
it. Let's take an example so you can see what is typically included in such an Adapter. Bear in mind that ALL
RDBMS are different in some (maybe small) way, so you may have to specify very little in this adapter, or
you may have a lot to specify depending on the RDBMS, and how capable it's JDBC drivers are.


	public class MySQLAdapter extends BaseDatastoreAdapter
	{
    	/**
    	 * A string containing the list of MySQL keywords that are not also SQL/92
    	 * <i>reserved words</i>, separated by commas.
    	 */
    	public static final String NONSQL92_RESERVED_WORDS =
        	"ANALYZE,AUTO_INCREMENT,BDB,BERKELEYDB,BIGINT,BINARY,BLOB,BTREE," +
        	"CHANGE,COLUMNS,DATABASE,DATABASES,DAY_HOUR,DAY_MINUTE,DAY_SECOND," +
        	"DELAYED,DISTINCTROW,DIV,ENCLOSED,ERRORS,ESCAPED,EXPLAIN,FIELDS," +
        	"FORCE,FULLTEXT,FUNCTION,GEOMETRY,HASH,HELP,HIGH_PRIORITY," +
        	"HOUR_MINUTE,HOUR_SECOND,IF,IGNORE,INDEX,INFILE,INNODB,KEYS,KILL," +
        	"LIMIT,LINES,LOAD,LOCALTIME,LOCALTIMESTAMP,LOCK,LONG,LONGBLOB," +
       	 	"LONGTEXT,LOW_PRIORITY,MASTER_SERVER_ID,MEDIUMBLOB,MEDIUMINT," +
        	"MEDIUMTEXT,MIDDLEINT,MINUTE_SECOND,MOD,MRG_MYISAM,OPTIMIZE," +
        	"OPTIONALLY,OUTFILE,PURGE,REGEXP,RENAME,REPLACE,REQUIRE,RETURNS," +
        	"RLIKE,RTREE,SHOW,SONAME,SPATIAL,SQL_BIG_RESULT,SQL_CALC_FOUND_ROWS," +
        	"SQL_SMALL_RESULT,SSL,STARTING,STRAIGHT_JOIN,STRIPED,TABLES," +
        	"TERMINATED,TINYBLOB,TINYINT,TINYTEXT,TYPES,UNLOCK,UNSIGNED,USE," +
        	"USER_RESOURCES,VARBINARY,VARCHARACTER,WARNINGS,XOR,YEAR_MONTH," +
        	"ZEROFILL";
	
    	/**
    	 * Constructor.
    	 * Overridden so we can add on our own list of NON SQL92 reserved words
    	 * which is returned incorrectly with the JDBC driver.
    	 * @param metadata MetaData for the DB
    	 **/
    	public MySQLAdapter(DatabaseMetaData metadata)
    	{
        	super(metadata);
	
        	reservedKeywords.addAll(parseKeywordList(NONSQL92_RESERVED_WORDS));
    	}
	
    	/**
    	 * An alias for this adapter.
    	 * @return The alias
    	 */
    	public String getVendorID()
    	{
        	return "mysql";
    	}
	
    	/**
    	 * MySQL, when using AUTO_INCREMENT, requires the primary key specified
    	 * in the CREATE TABLE, so we do nothing here. 
    	 * 
    	 * @param pkName The name of the primary key to add.
         * @param pk An object describing the primary key.
     	 * @return The statement to add the primary key separately
    	 */
    	public String getAddPrimaryKeyStatement(SQLIdentifier pkName, PrimaryKey pk)
    	{
        	return null;
    	}

    	/**
    	 * Whether the datastore supports specification of the primary key in
    	 * CREATE TABLE statements.
    	 * @return Whetehr it allows "PRIMARY KEY ..."
    	 */
    	public boolean supportsPrimaryKeyInCreateStatements()
    	{
        	return true;
    	}
	
    	/**
    	 * Method to return the CREATE TABLE statement.
    	 * Versions before 5 need INNODB table type selecting for them.
    	 * @param table The table
    	 * @param columns The columns in the table
    	 * @return The creation statement 
    	 **/
    	public String getCreateTableStatement(TableImpl table, Column[] columns)  
    	{
        	StringBuffer createStmt = new StringBuffer(super.getCreateTableStatement(table,columns));
	
        	// Versions before 5.0 need InnoDB table type
        	if (datastoreMajorVersion < 5)
        	{
            	createStmt.append(" TYPE=INNODB");
        	}
	
        	return createStmt.toString();
    	}
	
    	...
	}

So here we've shown a snippet from the MySQL DatastoreAdapter. We basically take much behaviour from 
the base class but override what we need to change for our RDBMS. You should get the idea by now. 
Just go through the Javadocs of the superclass and see what you need to override.

A final step that is optional here is to integrate your new adapter as a DataNucleus plugin.
To do this you need to package it with a file <i>plugin.xml</i>, specified at the root of the CLASSPATH.
The file should look like this


	<?xml version="1.0"?>
	<plugin id="mydomain" name="MyCompany DataNucleus plug-in" provider-name="MyCompany">
    	<extension point="org.datanucleus.store_datastoreadapter">
        	<datastore-adapter vendor-id="myname" class-name="mydomain.MyDatastoreAdapter" priority="10"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

Where the __myname__ specified is a string that is part of the JDBC "product name" (returned by
"DatabaseMetaData.getDatabaseProductName()"). If there are multiple adapters for the same 
_vendor-id_ defined, the attribute __priority__ is used to determine which one is used. 
The adapter with the highest number is chosen. Note that the behaviour is undefined when two or more 
adapters with _vendor-id_ have the same priority. All adapters defined in DataNucleus and its 
official plugins use priority values between __0__ and __9__. So, to make sure your adapter 
is chosen, use a value higher than that.

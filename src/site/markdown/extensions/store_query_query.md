<head><title>Extensions : Query Language</title></head>

## Extensions : Query Language
![Plugin](../images/nucleus_plugin.gif)

DataNucleus provides support for query languages to allow access to the persisted objects.
DataNucleus Core understands JDOQL, SQL and JPQL for use with the whichever StoreManager you decide to implement them for.
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.store_query_query* to achieve this.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Datastore</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>rdbms</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>SQL</td>
        <td>rdbms</td>
        <td>Query support for SQL</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JPQL</td>
        <td>rdbms</td>
        <td>Query support for JPQL</td>
        <td>datanucleus-rdbms</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>db4o</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-db4o</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>SQL</td>
        <td>db4o</td>
        <td>Query support for SQL</td>
        <td>datanucleus-db4o</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>Native</td>
        <td>db4o</td>
        <td>DB4O Native query support</td>
        <td>datanucleus-db4o</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>ldap</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-ldap</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>excel</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-excel</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>xml</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-xml</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>Native</td>
        <td>neodatis</td>
        <td>NeoDatis Native query support</td>
        <td>datanucleus-neodatis</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>Criteria</td>
        <td>neodatis</td>
        <td>NeoDatis Criteria query support</td>
        <td>datanucleus-neodatis</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>neodatis</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-neodatis</td>
    </tr>
    <tr>
        <td>org.datanucleus.store_query_query</td>
        <td>JDOQL</td>
        <td>json</td>
        <td>Query support for JDOQL</td>
        <td>datanucleus-json</td>
    </tr>
</table>

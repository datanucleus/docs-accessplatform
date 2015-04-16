<head><title>Extensions : MetaData Handler</title></head>

## Extensions : MetaData Handler
![Plugin](../images/nucleus_plugin.gif)

DataNucleus has supported JDO metadata from the outset. More than this, it actually provides a pluggable framework whereby you can plug in 
your own MetaData support. DataNucleus provides JDO and JPA metadata support, as well as "persistence.xml".
You can extend DataNucleus's capabilities using the plugin extension *org.datanucleus.metadata_handler*.


<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.metadata_handler</td>
        <td>jdo</td>
        <td>JDO MetaData handler</td>
        <td>datanucleus-api-jdo</td>
    </tr>
    <tr>
        <td>org.datanucleus.metadata_handler</td>
        <td>persistence</td>
        <td>"persistence.xml" MetaData handler</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.metadata_handler</td>
        <td>jpa</td>
        <td>JPA MetaData handler</td>
        <td>datanucleus-api-jpa</td>
    </tr>
</table>


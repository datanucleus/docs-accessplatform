<head><title>Extensions : Management Server</title></head>

## Extensions : Management Server
![Plugin](../images/nucleus_plugin.gif)

DataNucleus exposes runtime metrics via JMX MBeans. Management Servers permits DataNucleus to 
register its MBeans into a different MBeanServer. Management Servers can be plugged using the 
plugin extension *org.datanucleus.management_server*. The following plugin extensions are currently available

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Key</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.management_server</td>
        <td>default</td>
        <td>Register DataNucleus MBeans into the JVM MBeanServer.</td>
        <td>datanucleus-management</td>
    </tr>
    <tr>
        <td>org.datanucleus.management_server</td>
        <td>mx4j</td>
        <td>Register DataNucleus MBeans into a MX4J MBeanServer.</td>
        <td>datanucleus-management</td>
    </tr>
</table>


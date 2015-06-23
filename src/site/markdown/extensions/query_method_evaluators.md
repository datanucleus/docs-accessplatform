<head><title>Extensions - InMemory Query Methods</title></head>

## Extensions : JDOQL/JPQL In-Memory Query Method Evaluators
![Plugin](../images/nucleus_plugin.gif)

JDOQL/JPQL are defined to support particular methods/functions as part of the supported syntax.
This support is provided by way of an extension point, with support for these methods/functions
added via extensions. You can make use of this extension point to add on your own methods/functions - obviously this will be DataNucleus specific.

__This plugin extension point is currently only for evaluation of queries in-memory__. It will have no effect where the query is evaluated in the datastore.
The plugin extension used here is *org.datanucleus.query_method_evaluators*.

<table>
    <tr>
        <th>Plugin extension-point</th>
        <th>Class</th>
        <th>Name</th>
        <th>Description</th>
        <th width="80">Location</th>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>Math.abs</td>
        <td>Use of Math functions for JDO</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>Math.sqrt</td>
        <td>Use of Math functions for JDO</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>JDOHelper.getObjectId</td>
        <td>Use of JDOHelper functions for JDO</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>JDOHelper.getVersion</td>
        <td>Use of JDOHelper functions for JDO</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>CURRENT_DATE</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>CURRENT_TIME</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>CURRENT_TIMESTAMP</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>ABS</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>SQRT</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>MOD</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>SIZE</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>UPPER</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>LOWER</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>LENGTH</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>CONCAT</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>SUBSTRING</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>LOCATE</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>(static)</td>
        <td>TRIM</td>
        <td>JPQL functions</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>startsWith</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>endsWith</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>indexOf</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>substring</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>toUpperCase</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>toLowerCase</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>matches</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>trim</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.lang.String</td>
        <td>length</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Collection</td>
        <td>size</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Collection</td>
        <td>isEmpty</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Collection</td>
        <td>contains</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Map</td>
        <td>size</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Map</td>
        <td>isEmpty</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Map</td>
        <td>containsKey</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Map</td>
        <td>containsValue</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
    <tr>
        <td>org.datanucleus.query_method_evaluators</td>
        <td>java.util.Map</td>
        <td>get</td>
        <td>JDOQL methods</td>
        <td>datanucleus-core</td>
    </tr>
</table>


### Interface

Any query method/function plugin will need to implement _org.datanucleus.query.evaluator.memory.InvocationEvaluator_.
[![Javadoc](../images/javadoc.gif)](http://www.datanucleus.org/javadocs/core/latest/org/datanucleus/query/evaluator/memory/InvocationEvaluator.html).
So you need to implement the following interface

	public interface InvocationEvaluator
	{
    	/**
    	 * Method to evaluate the InvokeExpression, as part of the overall evaluation
    	 * defined by the InMemoryExpressionEvaluator.
    	 * @param expr The expression for invocation
    	 * @param invokedValue Value on which we are invoking this method
    	 * @param eval The overall evaluator for in-memory
    	 * @return The result
    	 */
    	Object evaluate(InvokeExpression expr, Object invokedValue, InMemoryExpressionEvaluator eval);
	}

### Implementation

Let's assume that you want to provide your own method for "String" _toUpperCase : obviously this is provided out of the box, but is here as an example.

	public class StringToUpperCaseMethodEvaluator implements InvocationEvaluator
	{
    	public Object evaluate(InvokeExpression expr, Object invokedValue, InMemoryExpressionEvaluator eval)
    	{
        	String method = expr.getOperation(); // Will be "toUpperCase"
	
        	if (invokedValue == null)
        	{
            	return null;
        	}
        	if (!(invokedValue instanceof String))
        	{
            	throw new NucleusException(Localiser.msg("021011", method, invokedValue.getClass().getName()));
        	}
        	return ((String)invokedValue).toUpperCase();
    	}
	}

### Plugin Specification

When we have defined our query method we just need to make it into a DataNucleus plugin. 
To do this you simply add a file <i>plugin.xml</i> to your JAR at the root and add a MANIFEST.MF. The file _plugin.xml_ should look like this

	<?xml version="1.0"?>
	<plugin id="mydomain" name="DataNucleus plug-ins" provider-name="My Company">
    	<extension point="org.datanucleus.query_method_evaluators">
        	<query-method-evaluator class="java.lang.String" method="toUpperCase" evaluator="org.datanucleus.query.evaluator.memory.StringToUpperCaseMethodEvaluator"/>
    	</extension>
	</plugin>

Note that you also require a MANIFEST.MF file as per the [Extensions Guide](index.html).

### Plugin Usage

The only thing remaining is to use your method in a JDOQL/JPQL query, like this

	Query q = pm.newQuery("SELECT FROM mydomain.Product WHERE name.toUpperCase() == 'KETTLE'");

so when evaluating the query in memory it will call this evaluator class for the field 'name'.

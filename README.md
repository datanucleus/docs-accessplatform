docs-accessplatform
===================

Maven project providing DataNucleus AccessPlatform documentation, as seen online at
http://www.datanucleus.org/products/accessplatform

In versions up to and including v4.2 this made use of Maven "site" plugin and the "docs-datanucleus-skin" for theming of the site. 
In version 5.0 and later it utilises AsciiDoc and the Maven asciidoctor plugin.

You generate the http://www.datanucleus.org website by invoking Maven "asciidoctor" plugin like this 'mvn clean compile' which generates the website under target/site
You generate a PDF of the AccessPlatform documentation by invoking Maven "pdf" like this `mvn clean pdf:pdf`.

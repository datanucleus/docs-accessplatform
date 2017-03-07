docs-accessplatform
===================

Maven project providing DataNucleus AccessPlatform documentation, as seen online at
http://www.datanucleus.org/products/accessplatform


In versions up to and including v4.2 this made use of Maven "site" plugin and the "docs-datanucleus-skin" for theming of the site.
In version 5.0 and later it utilises AsciiDoc and the Maven asciidoctor plugin.
The site uses Bootstrap v3.3, Bootstrap-TOC plugin, Font Awesome, and AsciiDoc foundation CSS.

You generate the http://www.datanucleus.org website by invoking Maven "asciidoctor" plugin like this
'mvn clean compile' which generates the website under _target/site_

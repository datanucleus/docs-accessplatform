docs-accessplatform
===================

Maven project providing DataNucleus AccessPlatform documentation, as seen online at
http://www.datanucleus.org/products/accessplatform


You generate the AccessPlatform documentation by invoking Maven "site" like this `mvn clean site`
which generates the website under _target/site_ . Makes use of the Maven "site" skin provided by "docs-accessplatform-skin".

You generate a PDF of the AccessPlatform documentation by invoking Maven "pdf" like this `mvn clean pdf:pdf`.

# tomcat7-bestpractices
- enable sticky sessions - ELB

- Increasing the Heap Size in Tomcat
	- To increase the heap size, we must add JAVA_OPTS parameter in catalina.sh, which could be found in TOMCAT_HOME/bin.
	- For instance, to increase max heap size to 512 MB and to set Perm Gen = 256 MB, the JAVA_OPTS parameter is:
		JAVA_OPTS="-Xms2048m -Xmx2048m"
				  "-Xms1024m -Xmx2048m -XX:MaxNewSize=512m -XX:MaxPermSize=256m"

- Thread tuning
	- These configurations need to be performed in TOMCAT_HOME/conf/server.xml.
		- connector configuration: 
			- `<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" acceptorThreadCount="2" maxThreads="400" acceptCount="200" minSpareThreads="20"/> `
				- acceptorThreadCount= # of processors
	 	 		* We have also allocated 400 threads to process requests, the default value is 200
	 			* rule of thumb= 150* #of cores
	 	 		* The "acceptCount" is set to 200 which denotes the maximum queue length to be used for incoming connections, The default value is 100
	 			* If the application is reasonably fast (for example, it exposes REST web services that usually send a response is a few milliseconds), then you can configure a large acceptCount, up to the same number of maxThreads.
	 			* Lastly we have set the minimum threads to 20 so that there are always 20 threads running in the pool to service requests.
		* don't use Bio because Nio is better in every way
			* protocol="org.apache.coyote.http11.Http11NioProtocol"
		* enable keepAlive
		* compression="on"
		* compressableMimeType="text/html,text/xml,text/plain"
		
- OS tuning

## References:
- https://github.com/terrancesnyder/tomcat/blob/master/shared/tc7/server.xml
- https://www.c2b2.co.uk/middleware-blog/tomcat-performance-monitoring-and-tuning.php
- https://tomcat.apache.org/articles/performance.pdf
- https://www.safaribooksonline.com/library/view/tomcat-the-definitive/9780596101060/ch04.html
- https://www.slideshare.net/lovingprince58/tomcat-optimisation-performance-tuning
- http://www.theserverside.com/tip/Two-most-commonly-misconfigured-Tomcat-performance-settings
- https://javamaster.wordpress.com/2013/03/13/apache-tomcat-tuning-guide/  ********
- https://medium.com/netflix-techblog/tuning-tomcat-for-a-high-throughput-fail-fast-system-e4d7b2fc163f ******
- http://terranceasnyder.com/2011/05/tomcat-best-practices/


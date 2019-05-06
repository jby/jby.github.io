---
layout: post
title: Use urandom instead of random with tomcat for JSS
---

Running Jamf Pro on a virtual server (2 cores, 4gb ram, with CentOS 7.3.1611) doesn't seem to work without telling java to use '/dev/urandom' as source for "randomness".

By default it will try to use /dev/random, which blocks random generation until enough entropy is gathered (which can take a very long time). We are not able to start JAMF without adding the flag "-Djava.security.egd=file:/dev/./urandom" to the variable "JAVA_OPTS" in '/etc/init.d/jamf.tomcat8'.  (According to https://wiki.apache.org/tomcat/HowTo/FasterStartUp) Is there a better way of doing this, is this behaviour documented anywhere, or are we simply missing something ? Below are the logs from a start of JAMF without the flag,

= = = = = = = = =
```Java HotSpot(TM) 64-Bit Server VM warning: ignoring option PermSize=256m; support was removed in 8.0 Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=256m; support was removed in 8.0
10-Jul-2017 17:51:48.853 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version: Apache Tomcat/8.0.43
10-Jul-2017 17:51:48.856 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built: Mar 28 2017 14:42:59 UTC
10-Jul-2017 17:51:48.856 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server number: 8.0.43.0
10-Jul-2017 17:51:48.856 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name: Linux
10-Jul-2017 17:51:48.856 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version: 3.10.0-514.16.1.el7.x86_64
10-Jul-2017 17:51:48.856 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture: amd64
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home: /usr/java/jdk1.8.0_131/jre
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version: 1.8.0_131-b11
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor: Oracle Corporation
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE: /usr/local/jss/tomcat
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME: /usr/local/jss/tomcat
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/jss/tomcat/conf/logging.properties
10-Jul-2017 17:51:48.857 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.awt.headless=true
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -XX:PermSize=256m
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -XX:MaxPermSize=256m
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.endorsed.dirs=/usr/local/jss/tomcat/endorsed
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/jss/tomcat
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/jss/tomcat
10-Jul-2017 17:51:48.858 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/jss/tomcat/temp
10-Jul-2017 17:51:48.859 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: /usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
10-Jul-2017 17:51:49.016 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-127.0.0.1-8080"]
10-Jul-2017 17:51:49.031 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
10-Jul-2017 17:51:49.034 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["ajp-nio-8009"]
10-Jul-2017 17:51:49.036 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
10-Jul-2017 17:51:49.036 INFO [main] org.apache.catalina.startup.Catalina.load Initialization processed in 667 ms
10-Jul-2017 17:51:49.060 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service Catalina
10-Jul-2017 17:51:49.060 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet Engine: Apache Tomcat/8.0.43
10-Jul-2017 17:51:49.084 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployWAR Deploying web application archive /usr/local/jss/tomcat/webapps/ROOT.war
10-Jul-2017 17:52:02.667 INFO [localhost-startStop-1] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time. ClassLoaderLeakPreventor: Settings for se.jiderhamn.classloader.leak.prevention.ClassLoaderLeakPreventor (CL: 0x624fc60c): ClassLoaderLeakPreventor: stopThreads = false ClassLoaderLeakPreventor: stopTimerThreads = true ClassLoaderLeakPreventor: executeShutdownHooks = true ClassLoaderLeakPreventor: threadWaitMs = 5000 ms ClassLoaderLeakPreventor: shutdownHookWaitMs = 10000 ms ClassLoaderLeakPreventor: Initializing context by loading some known offenders with system classloader``` <- This is the last row. Nothing shows up after this.
= = = = = = = = =
Below are the logs from a successful startup with the flag '-Djava.security.egd=file:/dev/./urandom' added,
= = = = = = = = =
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option PermSize=256m; support was removed in 8.0 Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=256m; support was removed in 8.0
10-Jul-2017 16:51:03.941 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version: Apache Tomcat/8.0.43
10-Jul-2017 16:51:03.945 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built: Mar 28 2017 14:42:59 UTC
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server number: 8.0.43.0
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name: Linux
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version: 3.10.0-514.16.1.el7.x86_64
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture: amd64
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home: /usr/java/jdk1.8.0_131/jre
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version: 1.8.0_131-b11
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor: Oracle Corporation
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE: /usr/local/jss/tomcat
10-Jul-2017 16:51:03.946 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME: /usr/local/jss/tomcat
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/jss/tomcat/conf/logging.properties
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.awt.headless=true
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -XX:PermSize=256m
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -XX:MaxPermSize=256m
10-Jul-2017 16:51:03.947 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.endorsed.dirs=/usr/local/jss/tomcat/endorsed
10-Jul-2017 16:51:03.948 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/jss/tomcat
10-Jul-2017 16:51:03.948 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/jss/tomcat
10-Jul-2017 16:51:03.948 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/jss/tomcat/temp
10-Jul-2017 16:51:03.948 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: /usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
10-Jul-2017 16:51:04.147 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-127.0.0.1-8080"]
10-Jul-2017 16:51:04.164 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
10-Jul-2017 16:51:04.168 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["ajp-nio-8009"]
10-Jul-2017 16:51:04.169 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
10-Jul-2017 16:51:04.170 INFO [main] org.apache.catalina.startup.Catalina.load Initialization processed in 684 ms
10-Jul-2017 16:51:04.199 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service Catalina
10-Jul-2017 16:51:04.200 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet Engine: Apache Tomcat/8.0.43
10-Jul-2017 16:51:04.231 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployWAR Deploying web application archive /usr/local/jss/tomcat/webapps/ROOT.war
10-Jul-2017 16:51:17.300 INFO [localhost-startStop-1] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time. ClassLoaderLeakPreventor: Settings for se.jiderhamn.classloader.leak.prevention.ClassLoaderLeakPreventor (CL: 0x5811bf9f): ClassLoaderLeakPreventor: stopThreads = false ClassLoaderLeakPreventor: stopTimerThreads = true ClassLoaderLeakPreventor: executeShutdownHooks = true ClassLoaderLeakPreventor: threadWaitMs = 5000 ms ClassLoaderLeakPreventor: shutdownHookWaitMs = 10000 ms ClassLoaderLeakPreventor: Initializing context by loading some known offenders with system classloader```

# Notice this line that is not present in the failed startup, this led me to believe there was something wrong with the initialization of the randomiser.

10-Jul-2017 16:59:02.996 INFO [localhost-startStop-1] org.apache.catalina.util.SessionIdGeneratorBase.createSecureRandom Creation of SecureRandom instance for session ID generation using [SHA1PRNG] took [446,973] milliseconds.  Attempting to load ESAPI.properties via file I/O. Attempting to load ESAPI.properties as resource file via file I/O. Not found in 'org.owasp.esapi.resources' directory or file not readable: /usr/local/jss/tomcat/ESAPI.properties Not found in SystemResource Directory/resourceDirectory: .esapi/ESAPI.properties Not found in 'user.home' (/usr/local/jss/tomcat) directory: /usr/local/jss/tomcat/esapi/ESAPI.properties Loading ESAPI.properties via file I/O failed. Exception was: java.io.FileNotFoundException Attempting to load ESAPI.properties via the classpath. SUCCESSFULLY LOADED ESAPI.properties via the CLASSPATH from '/ (root)' using current thread context class loader! Attempting to load validation.properties via file I/O. Attempting to load validation.properties as resource file via file I/O. Not found in 'org.owasp.esapi.resources' directory or file not readable: /usr/local/jss/tomcat/validation.properties Not found in SystemResource Directory/resourceDirectory: .esapi/validation.properties Not found in 'user.home' (/usr/local/jss/tomcat) directory: /usr/local/jss/tomcat/esapi/validation.properties Loading validation.properties via file I/O failed. Attempting to load validation.properties via the classpath. SUCCESSFULLY LOADED validation.properties via the CLASSPATH from '/ (root)' using current thread context class loader! SecurityConfiguration for ESAPI.printProperties not found in ESAPI.properties. Using default: false SecurityConfiguration for Encryptor.CipherTransformation not found in ESAPI.properties. Using default: AES/CBC/PKCS5Padding SecurityConfiguration for ESAPI.Logger not found in ESAPI.properties. Using default: org.owasp.esapi.reference.JavaLogFactory SecurityConfiguration for Logger.LogApplicationName not found in ESAPI.properties. Using default: true SecurityConfiguration for Logger.LogServerIP not found in ESAPI.properties. Using default: true
10-Jul-2017 16:59:06.583 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployWAR Deployment of web application archive /usr/local/jss/tomcat/webapps/ROOT.war has finished in 482,352 ms
10-Jul-2017 16:59:06.648 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-127.0.0.1-8080"]
10-Jul-2017 16:59:06.651 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["ajp-nio-8009"]
10-Jul-2017 16:59:06.653 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 482482 ms CMFileReset:JAMFCMFILE CMSyslogReset:JAMFCMSYSLOG
10-Jul-2017 16:59:48.560 INFO [main] org.apache.catalina.core.StandardServer.await A valid shutdown command was received via the shutdown port. Stopping the Server instance. §10-Jul-2017 16:59:48.561 INFO [main] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["http-nio-127.0.0.1-8080"]
10-Jul-2017 16:59:48.612 INFO [main] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["ajp-nio-8009"]
10-Jul-2017 16:59:48.662 INFO [main] org.apache.catalina.core.StandardService.stopInternal Stopping service Catalina Starting deallocation Deallocation complete```

# How to Build
Maven and Java are required. After that it's as simple as running `mvn:install`.  A folder called `libs` will be created in your `target` directory. It contains all jars that you need. Copy them to you home dir.

For the KNOX and SSL:
    ```
    java -Xmx1024m -classpath :lib/* -Dhdp.version=2.6.5.0-292 -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=${HOME} -Dhadoop.log.file=hadoop.log \
    -Dhadoop.home.dir=${HOME} -Dhadoop.id.str=${USER} -Dhadoop.root.logger=INFO,console -Djava.library.path=:${JAVA_HOME} -Dhadoop.policy.file=hadoop-policy.xml \
    -Djava.net.preferIPv4Stack=true -Djavax.security.auth.useSubjectCredsOnly=false -Dhadoop.security.logger=INFO,NullAppender org.apache.hadoop.util.RunJar \
    ${HOME}/lib/hive-beeline-1.2.1000.2.6.5.0-292.jar org.apache.hive.beeline.BeeLine  -n knox_user -p knox_password -u \
    "jdbc:hive2://KNOX_HOSTNAME:443/;ssl=true;sslTrustStore=/etc/ssl/certs/java/cacerts;trustStorePassword=changeit?hive.server2.transport.mode=http;hive.server2.thrift.http.path=gateway/default/hive"
    ```

# Logging
All logging dependencies have been filtered and bridged with SLF4J in this jar and Log4J has been included as the logging implementation.  While no `log4j.properties` has been included in this jar, its fairly easy to configure Log4J and DbVisualizer to debug whats happening inside JDBC.  To setup Log4J in DbVisualizer, do the following.

1. Create a `log4j.properties` file and put it somewhere easy to remember find on your workstation.  Below is a very simple example.

```log4j
log4j.rootLogger=WARN, console

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{HH:mm:ss:SSS} %-5p [%c]: %m%n

log4j.logger.org.apache.hive=DEBUG
log4j.logger.org.apache.hadoop=DEBUG
log4j.logger.org.apache.thrift=DEBUG
```

2. Add the following JVM flag

```dosini
-Dlog4j.configuration=file:[path to your log4j.properties file]/log4j.properties
```


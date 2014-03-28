# Fluent Logger for Java

Forked from https://github.com/fluent/fluent-logger-java.

## Overview

Many web/mobile applications generate huge amount of event logs (c,f. login,
logout, purchase, follow, etc).  Analyzing these event logs can be quite
valuable for improving services.  However, collecting these logs easily and 
reliably is a challenging task.

Fluentd solves the problem by having: easy installation, small footprint, plugins
reliable buffering, log forwarding, etc.

  * Fluentd website: [http://github.com/fluent/fluentd](http://github.com/fluent/fluentd)

**fluent-logger-java** is a Java library, to record events via Fluentd, from Java application.

## Requirements

Java >= 1.6

## Install

### Install from Maven2 repository

Fluent Logger for Java is released on Fluent Maven2 repository.  You can 
configure your pom.xml as follows to use it:

    <dependencies>
      ...
      <dependency>
        <groupId>com.bericotech.fork.org.fluentd</groupId>
        <artifactId>fluent-logger</artifactId>
        <version>${logger.version}</version>
      </dependency>

      <repositories>
        <repository>
         <id>nexus.bericotechnologies.com</id>
         <name>Berico Technologies Nexus</name>
         <url>http://nexus.bericotechnologies.com/content/groups/public</url>
         <releases><enabled>true</enabled></releases>
         <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
      ...
    </dependencies>


## Quickstart

The following program is a small example of Fluent Logger for Java.

    import java.util.HashMap;
    import java.util.Map;
    import org.fluentd.logger.FluentLogger;

    public class Main {
        private static FluentLogger LOG = FluentLogger.getLogger("app");

        public void doApplicationLogic() {
            // ...
            Map<String, String> data = new HashMap<String, String>();
            data.put("from", "userA");
            data.put("to", "userB");
            LOG.log("follow", data);
            // ...
        }
    }

To create Fluent Logger instances, users need to invoke getLogger method in 
FluentLogger class like org.slf4j, org.log4j logging libraries.  The method 
should be called only once.  By default, the logger assumes fluent daemon is 
launched locally.  You can also specify remote logger by passing the following 
options.  

    // for remote fluentd
    private static FluentLogger LOG = FluentLogger.getLogger("app", "remotehost", port);

Then, please create the events like this.  This will send the event to fluentd, 
with tag 'app.follow' and the attributes 'from' and 'to'.

Close method in FluentLogger class should be called explicitly when application 
is finished.  The method close socket connection with the fluentd.

    FluentLogger.close();

## License

Apache License, Version 2.0

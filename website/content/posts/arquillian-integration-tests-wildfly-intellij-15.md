+++
author = "Alexander Sparkowsky"
categories = ["Java", "javaee", "intellij", "maven", "junit", "testing", "integration-testing"]
date = 2016-03-01T18:00:00Z
description = ""
draft = false
slug = "arquillian-integration-tests-wildfly-intellij-15"
tags = ["Java", "javaee", "intellij", "maven", "junit", "testing", "integration-testing"]
title = "Running/Debugging Arquillian Integration-Tests with WildFly inside IntelliJ 15"

+++

Unfortunately integration testing needs a great effort to be set up. But once you have it up and running you benefit from being able to examine how multiple parts are working together.

## Configure the embedded container using `maven`

Add an Arquillian profile to your `pom.xml`. This will allow you to run the integration-tests using maven and allows you to deploy and configure the WildFly container itself.

```xml
    <profiles>
        <profile>
            <id>arquillian-wildfly-embedded</id>
            <dependencies>
                <dependency>
                    <groupId>org.wildfly.arquillian</groupId>
                    <artifactId>wildfly-arquillian-container-embedded</artifactId>
                    <version>1.1.0.Beta1</version>
                    <scope>test</scope>
                </dependency>
                <!-- this is the wildfly emb.container - BUT eventually it is not a fully blown emb.container-->
                <dependency>
                    <groupId>org.wildfly</groupId>
                    <artifactId>wildfly-embedded</artifactId>
                    <version>${org.wildfly}</version>
                </dependency>

                <dependency>
                    <groupId>io.undertow</groupId>
                    <artifactId>undertow-websockets-jsr</artifactId>
                    <version>1.0.0.Beta25</version>
                    <scope>test</scope>
                </dependency>
                <dependency>
                    <groupId>org.jboss.resteasy</groupId>
                    <artifactId>resteasy-client</artifactId>
                    <version>3.0.5.Final</version>
                    <scope>test</scope>
                </dependency>
                <dependency>
                    <groupId>org.jboss.resteasy</groupId>
                    <artifactId>resteasy-jaxb-provider</artifactId>
                    <version>3.0.5.Final</version>
                    <scope>test</scope>
                </dependency>
                <dependency>
                    <groupId>org.jboss.resteasy</groupId>
                    <artifactId>resteasy-json-p-provider</artifactId>
                    <version>3.0.5.Final</version>
                    <scope>test</scope>
                </dependency>
                <dependency>
                    <groupId>org.postgresql</groupId>
                    <artifactId>postgresql</artifactId>
                    <version>9.4.1208.jre7</version>
                    <scope>test</scope>
                </dependency>
            </dependencies>
            <build>
                <plugins>
                    <plugin>
                        <artifactId>maven-dependency-plugin</artifactId>
                        <executions>
                            <execution>
                                <id>unpack</id>
                                <phase>process-test-classes</phase>
                                <goals>
                                    <goal>unpack</goal>
                                </goals>
                                <configuration>
                                    <artifactItems>
                                        <artifactItem>
                                            <groupId>org.wildfly</groupId>
                                            <artifactId>wildfly-dist</artifactId>
                                            <version>${org.wildfly}</version>
                                            <type>zip</type>
                                            <overWrite>false</overWrite>
                                            <outputDirectory>target</outputDirectory>
                                        </artifactItem>
                                    </artifactItems>
                                </configuration>
                            </execution>
                        </executions>
                    </plugin>

                    <plugin>
                        <groupId>org.wildfly.plugins</groupId>
                        <artifactId>wildfly-maven-plugin</artifactId>
                        <version>1.0.2.Final</version>
                        <configuration>
                            <jboss-home>${project.basedir}/target/wildfly-${org.wildfly}</jboss-home>
                            <modules-path>${project.basedir}/target/wildfly-${org.wildfly}/modules</modules-path>
                        </configuration>
                        <executions>
                            <!--
                             - Start the test container
                             -->
                            <execution>
                                <phase>prepare-package</phase>
                                <goals>
                                    <goal>start</goal>
                                </goals>
                            </execution>
                            <!--
                             - Deploy the jdbc driver
                             -->
                            <execution>
                                <id>jdbc</id>
                                <phase>package</phase>
                                <goals>
                                    <goal>deploy-artifact</goal>
                                </goals>
                                <configuration>
                                    <groupId>org.postgresql</groupId>
                                    <artifactId>postgresql</artifactId>
                                    <version>9.4.1208.jre7</version>
                                    <name>postgresql.jar</name>
                                </configuration>
                            </execution>
                            <!--
                             - Add the datasource for testing
                             -->
                            <execution>
                                <id>datasource</id>
                                <phase>package</phase>
                                <goals>
                                    <goal>add-resource</goal>
                                </goals>
                                <configuration>
                                    <address>subsystem=datasources,data-source=tests</address>
                                    <resources>
                                        <resource>
                                            <properties>
                                                <connection-url>${test.datasource.jdbcUrl}</connection-url>
                                                <jndi-name>${test.datasource.jndi}</jndi-name>
                                                <enabled>true</enabled>
                                                <enable>true</enable>
                                                <user-name>${test.datasource.userName}</user-name>
                                                <password>${test.datasource.password}</password>
                                                <driver-name>postgresql.jar</driver-name>
                                                <use-ccm>false</use-ccm>
                                            </properties>
                                        </resource>
                                    </resources>
                                </configuration>
                            </execution>
                            <!--
                             - Shutdown the test container
                             -->
                            <execution>
                                <id>stop-after-config</id>
                                <phase>package</phase>
                                <goals>
                                    <goal>shutdown</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>

                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-failsafe-plugin</artifactId>
                        <version>2.17</version>
                        <configuration>
                            <systemPropertyVariables>
                                <java.util.logging.manager>org.jboss.logmanager.LogManager</java.util.logging.manager>
                                <jboss.home>${project.basedir}/target/wildfly-${org.wildfly}</jboss.home>
                                <module.path>${project.basedir}/target/wildfly-${org.wildfly}/modules</module.path>
                            </systemPropertyVariables>
                        </configuration>
                        <executions>
                            <execution>
                                <id>integration-test</id>
                                <goals>
                                    <goal>integration-test</goal>
                                </goals>
                            </execution>
                            <execution>
                                <id>verify</id>
                                <goals>
                                    <goal>verify</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>

        </profile>
```

## Configure the IntelliJ Run/Debug Configuration

* Add a new configuration of type _Arquillian JUnit_
* If not already present add an _Arquillian Container_ from the _Embedded_ category of type _WildFly Embedded_ I've been chasing Version `1.1.0.Beta1` of the container.
* Select the _WildFly Embedded_ container
* Configure your integration-test in the _Configuration_ tab
  * Test kind: _Method_ or _Class_
  * Repeat: once
  * Use class path of module: _Your module_
  * Class: _Your test class_
  * Method: _Your test method_
  * VM options: `-Djava.util.logging.manager=org.jboss.logmanager.LogManager`  
_This is important since WildFly will not run without a logging manager_
  * Working directory: `$MODULE_DIR$`
  * JRE: Default
  * Before launch:
    * Make
    * Run Maven Goal with command line `-Parquillian-wildfly-embedded process-test-classes`
    * Run Maven Goal with command line `-Parquillian-wildfly-embedded prepare-package`


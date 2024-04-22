With Quarkus 3.9.4 and Azure Quarkiverse 1.0.2 native build fails

This is just a repo that uses [latest Quarkus getting started](https://github.com/quarkusio/quarkus-quickstarts/tree/main/getting-started).

Changes brought are just adding the Quarkiverse Azure dependency

```xml
         <dependency>
            <groupId>io.quarkiverse.azureservices</groupId>
            <artifactId>quarkus-azure-storage-blob</artifactId>
            <version>${azure-quarkiverse-version}</version>
            <exclusions>
                <exclusion>
                    <groupId>com.azure</groupId>
                    <artifactId>azure-core-http-netty</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

Run this command to reproduce

```sh
mvn install -Dnative -DskipTests

``` 

Logs

```sh
➜  azure-quarkiverse-native-build-fails git:(master) mvn install -Dnative -DskipTests
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< org.acme:getting-started >----------------------
[INFO] Building getting-started 1.0.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ getting-started ---
[INFO] Copying 2 resources from src/main/resources to target/classes
[INFO]
[INFO] --- compiler:3.11.0:compile (default-compile) @ getting-started ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- resources:3.3.1:testResources (default-testResources) @ getting-started ---
[INFO] skip non existing resourceDirectory /home/olivier/dev/bugs/azure-quarkiverse-native-build-fails/src/test/resources
[INFO]
[INFO] --- compiler:3.11.0:testCompile (default-testCompile) @ getting-started ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- surefire:3.1.2:test (default-test) @ getting-started ---
[INFO] Tests are skipped.
[INFO]
[INFO] --- jar:3.3.0:jar (default-jar) @ getting-started ---
[INFO] Building jar: /home/olivier/dev/bugs/azure-quarkiverse-native-build-fails/target/getting-started-1.0.0-SNAPSHOT.jar
[INFO]
[INFO] --- quarkus:3.9.4:build (default) @ getting-started ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.642 s
[INFO] Finished at: 2024-04-22T18:39:26+02:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal io.quarkus.platform:quarkus-maven-plugin:3.9.4:build (default) on project getting-started: Failed to build quarkus application: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
[ERROR]         [error]: Build step io.quarkiverse.azure.jackson.datafromat.xml.deployment.JacksonDataformatXmlSupportProcessor#serviceProviders threw an exception: java.lang.NoClassDefFoundError: com/ctc/wstx/shaded/msv/org_isorelax/verifier/VerifierFactoryLoader
[ERROR]         at io.quarkiverse.azure.jackson.datafromat.xml.deployment.JacksonDataformatXmlSupportProcessor.serviceProviders(JacksonDataformatXmlSupportProcessor.java:46)
[ERROR]         at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
[ERROR]         at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
[ERROR]         at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
[ERROR]         at java.base/java.lang.reflect.Method.invoke(Method.java:568)
[ERROR]         at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:849)
[ERROR]         at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
[ERROR]         at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
[ERROR]         at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
[ERROR]         at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
[ERROR]         at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1495)
[ERROR]         at java.base/java.lang.Thread.run(Thread.java:833)
[ERROR]         at org.jboss.threads.JBossThread.run(JBossThread.java:483)
[ERROR] Caused by: java.lang.ClassNotFoundException: com.ctc.wstx.shaded.msv.org_isorelax.verifier.VerifierFactoryLoader
[ERROR]         at org.codehaus.plexus.classworlds.strategy.SelfFirstStrategy.loadClass(SelfFirstStrategy.java:50)
[ERROR]         at org.codehaus.plexus.classworlds.realm.ClassRealm.unsynchronizedLoadClass(ClassRealm.java:271)
[ERROR]         at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:247)
[ERROR]         at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:239)
[ERROR]         at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:518)
[ERROR]         at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:468)
[ERROR]         ... 13 more
[ERROR] -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```

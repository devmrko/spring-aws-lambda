# spring-aws-lambda

## Prerequisites
* Respond AWS lambda call

## Configuration
- Function configuratoin
  + put class name at function.name in application.properties
  + class has to be implemented by java.util.function.Function
  + since it's running by Spring boot, this class has to have component annotation
- SpringBootRequestHandler configuratoin
  + aws lambda actually appoint this class
  + class has to be extends by org.springframework.cloud.function.adapter.aws.SpringBootRequestHandler
  + define which kinds of type will be used
- add defendencies in pom.xml
```
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-function-adapter-aws</artifactId>
  <version>2.0.1.RELEASE</version>
</dependency>

<dependency>
  <groupId>com.amazonaws</groupId>
  <artifactId>aws-lambda-java-core</artifactId>
  <version>1.2.0</version>
</dependency>
```
- add plugins in pom.xml
```
<!-- plugin to generate shaded jar -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-shade-plugin</artifactId>
  <configuration>
    <createDependencyReducedPom>false</createDependencyReducedPom>
  </configuration>
  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>shade</goal>
      </goals>
    </execution>
  </executions>
</plugin>

<!-- set a JDK compiler level -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <configuration>
    <source>1.8</source>
    <target>1.8</target>
  </configuration>
</plugin>

<!-- make this jar executable -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-jar-plugin</artifactId>
</plugin>

<!-- make this jar thin. it has to be set because of AWS lambda size limitation -->
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot.experimental</groupId>
      <artifactId>spring-boot-thin-layout</artifactId>
      <version>1.0.22.RELEASE</version>
    </dependency>
  </dependencies>
</plugin>
```
* in terms of technology, thin is
in terms of technology, thin is defined below
```
Skinny – Contains ONLY the bits you literally type into your code editor, and NOTHING else.
Thin – Contains all of the above PLUS the app’s direct dependencies of your app (db drivers, utility libraries, etc).
Hollow – The inverse of Thin – Contains only the bits needed to run your app but does NOT contain the app itself. Basically a pre-packaged “app server” to which you can later deploy your app, in the same style as traditional Java EE app servers, but with important differences, we’ll get to later.
Fat/Uber – Contains the bit you literally write yourself PLUS the direct dependencies of your app PLUS the bits needed to run your app “on its own”.
```

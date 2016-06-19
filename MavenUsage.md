# Maven Usage Instructions #

To use the forbidden API checker in Maven, use the following template to include the plugin:

```xml
<properties>
  <!-- 
   It is recommended to set the compiler version globally,
   as the compiler plugin and the forbidden API checker both
   use this version
  -->
  <maven.compiler.target>1.6</maven.compiler.target>
</properties>

<build>
  <plugins>
    <plugin>
      <groupId>de.thetaphi</groupId>
      <artifactId>forbiddenapis</artifactId>
      <version>2.2</version>
      <configuration>
        <!--
          if the used Java version is too new,
          don't fail, just do nothing:
        -->
        <failOnUnsupportedJava>false</failOnUnsupportedJava>
        <bundledSignatures>
          <!--
            This will automatically choose the right
            signatures based on 'maven.compiler.target':
          -->
          <bundledSignature>jdk-unsafe</bundledSignature>
          <bundledSignature>jdk-deprecated</bundledSignature>
          <!-- disallow undocumented classes like sun.misc.Unsafe: -->
          <bundledSignature>jdk-non-portable</bundledSignature>
        </bundledSignatures>
        <signaturesFiles>
          <signaturesFile>./rel/path/to/signatures.txt</signaturesFile>
        </signaturesFiles>
      </configuration>
      <executions>
        <execution>
          <goals>
            <goal>check</goal>
            <goal>testCheck</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
  <!-- more build settings here... -->
</build>
```

The possible `<bundledSignatures>` can be found on a [separate page](BundledSignatures). You can also give your own signatures in separate files from your project directory.

Since version 1.2 the goal was renamed to "check" and "testCheck" (to check the test classes) was added. Since version 2.0, the plugin runs by default in the `verify` lifecycle phase.
Of course, you can assign any other phase, like the previous `process-classes` / `process-test-classes`.

The detailed documentation (based on nightly snapshots) can be found here: http://jenkins.thetaphi.de/job/Forbidden-APIs/javadoc/

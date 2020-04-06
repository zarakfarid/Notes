# Fat Jar


```Shell
java -jar -DdrUpdate.properties=/home/ana/zarak/drUpdate.properties -Dsharedshelf.properties=/home/ana/sharedshelf/properties/sharedshelf.properties -Dsharedshelf.log4j.properties=/home/ana/sharedshelf/properties/sharedshelf.log4j.properties GettingDrs-1.0-SNAPSHOT.jar

java -jar -DdrUpdate.properties=/home/ana/zarak/drUpdate.properties GettingDrs-1.0-SNAPSHOT-jar-with-dependencies.jar
```
## Assembly Plugin
```Shell
clean compile assembly:single
```
```xml
<plugin>
      <!-- Build an executable JAR -->
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>org.artstor.xml.updater.XmlClassificationUpdate</mainClass>
      </manifest>
    </archive>
    <descriptorRefs>
      <descriptorRef>jar-with-dependencies</descriptorRef>
    </descriptorRefs>
  </configuration>
  <executions>
    <execution>
      <id>make-assembly</id> <!-- this is used for inheritance merges -->
      <phase>package</phase> <!-- bind to the packaging phase -->
      <goals>
        <goal>single</goal>
      </goals>
    </execution>
  </executions>
</plugin>
}
```

## Shade Plugin
```Shell
clean package
```
```xml
<plugin>
            <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-shade-plugin</artifactId>
  <version>2.3</version>
  <executions>
     <!-- Run shade goal on package phase -->
    <execution>
<phase>package</phase>
<goals>
<goal>shade</goal>
</goals>
<configuration>
<filters>
        <filter>
            <artifact>*:*</artifact>
            <excludes>
                <exclude>META-INF/*.SF</exclude>
                <exclude>META-INF/*.DSA</exclude>
                <exclude>META-INF/*.RSA</exclude>
            </excludes>
        </filter>
    </filters>
  <transformers>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>org.artstor.xml.updater.XmlClassificationUpdate</mainClass>
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                <resource>META-INF/spring.handlers</resource>
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                <resource>META-INF/spring.schemas</resource>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
```

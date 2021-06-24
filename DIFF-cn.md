> # 关于新打包的JanusGraph相关Jar的改动

`不同版本大同小异，可以自行按照版本就该，这里主要是解决了guava冲突的问题。`
`本项目已做修改，可参考对比。`

> ## JanusGraph Core

1. 重定义guava相关库，pom增加include。

```
<include>com.google.guava:guava</include>
```


2. pom增加relocation。

```
<relocation>
    <pattern>com.google.guava</pattern>
    <shadedPattern>org.janusgraph.shaded.google_guava</shadedPattern>
</relocation>
<relocation>
    <pattern>com.google.common</pattern>
    <shadedPattern>org.janusgraph.shaded.google_common</shadedPattern>
</relocation>
<relocation>
    <pattern>com.google.thirdparty</pattern>
    <shadedPattern>org.janusgraph.shaded.google_thirdparty</shadedPattern>
</relocation>
```

3. 增加独立版本号0.2.3-shade。

```
<version>0.2.3-shade</version>
```
4. 使用maven install打出新的包，得到target下janusgraph-core-0.2.3-shade.jar。

> ## JanusGraph ES 

1. 增加独立版本号0.2.3-shade。

```
<version>0.2.3-shade</version>
```

2. 修改依赖，janusgraph-core改为0.2.3-shade，其他的janusgraph相关依赖不再依赖${project.version}版本，改为0.2.3版本。
```
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-core</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-test</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-berkeleyje</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-berkeleyje</artifactId>
    <version>0.2.3</version>
    <classifier>tests</classifier>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-cassandra</artifactId>
    <version>0.2.3</version>
    <classifier>tests</classifier>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-cassandra</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
3. 在build plugins中增加shade相关配置，重定义httpcore库和elasticsearch-rest-client库。
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.2.1</version>
    <executions>
        <execution>
            <id>shade-gremlin-groovy</id>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <createDependencyReducedPom>false</createDependencyReducedPom>
                <artifactSet>
                    <includes>
                        <include>org.elasticsearch.client:*</include>
                        <include>org.apache.httpcomponents:*</include>
                    </includes>
                </artifactSet>
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/MANIFEST.MF</exclude>
                        </excludes>
                    </filter>
                </filters>
                <relocations>
                    <relocation>
                        <pattern>org.apache.http</pattern>
                        <shadedPattern>org.janusgraph.shaded.http_441</shadedPattern>
                    </relocation>
                    <relocation>
                        <pattern>org.elasticsearch.client</pattern>
                        <shadedPattern>org.janusgraph.shaded.elasticsearch_client_601</shadedPattern>
                    </relocation>
                </relocations>
                <!-- false below means the shade plugin overwrites the main project artifact (the one with no classifier).
                     false does *not* actually detach the main artifact, despite what the option name suggests. -->
                <shadedArtifactAttached>false</shadedArtifactAttached>
                <minimizeJar>false</minimizeJar>
            </configuration>
        </execution>
    </executions>
</plugin>
```
4. 使用maven install打出新的包，得到target下janusgraph-es-0.2.3-shade.jar。

> ## JanusGraph HBase

1. 修改janusgraph-hbase-parent的pom文件，增加独立版本号0.2.3-shade。
```<version>0.2.3-shade</version>```
2. 修改janusgraph-hbase-parent的pom文件中的依赖，janusgraph-core/janusgraph-es改为0.2.3-shade，其他的janusgraph相关依赖不再依赖${project.version}版本，改为0.2.3版本。
```
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-core</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-test</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-es</artifactId>
    <version>0.2.3-shade</version>
    <scope>test</scope>
</dependency>
```
3. 修改janusgraph-hbase-core的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
4. 在janusgraph-hbase-core的pom文件build plugins增加shade相关配置，重定义guava库。
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.2.1</version>
    <executions>
        <execution>
            <id>shade-gremlin-groovy</id>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <createDependencyReducedPom>false</createDependencyReducedPom>
                <artifactSet>
                    <includes>
                        <include>com.google.guava:guava</include>
                    </includes>
                </artifactSet>
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/MANIFEST.MF</exclude>
                        </excludes>
                    </filter>
                </filters>
                <relocations>
                    <relocation>
                        <pattern>com.google.guava</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_guava</shadedPattern>
                    </relocation>
                    <relocation>
                        <pattern>com.google.common</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_common</shadedPattern>
                    </relocation>
                    <relocation>
                        <pattern>com.google.thirdparty</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_thirdparty</shadedPattern>
                    </relocation>
                </relocations>
                <!-- false below means the shade plugin overwrites the main project artifact (the one with no classifier).
                     false does *not* actually detach the main artifact, despite what the option name suggests. -->
                <shadedArtifactAttached>true</shadedArtifactAttached>
                <minimizeJar>false</minimizeJar>
            </configuration>
        </execution>
    </executions>
</plugin>
```
5. 修改janusgraph-hbase的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
6. 修改janusgraph-hbase-10的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
7. 修改janusgraph-hbase-098的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
8. 对janusgraph-hbase-parent的pom文件使用maven install打出新的包，得到janusgraph-hbase/target下janusgraph-hbase-0.2.3-shade.jar。

> ## JanusGraph Hadoop
1. 修改janusgraph-hadoop-parent的pom文件，增加独立版本号0.2.3-shade。
```
<version>0.2.3-shade</version>
```
2. 修改janusgraph- hadoop -parent的pom文件中的依赖，janusgraph-core/janusgraph-es/janusgraph-hbase/ janusgraph-hbase-core改为0.2.3-shade，其他的janusgraph相关依赖不再依赖${project.version}版本，改为0.2.3版本。
```
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-core</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-cassandra</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-es</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-test</artifactId>
    <version>0.2.3</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hbase-core</artifactId>
    <version>0.2.3-shade</version>
    <classifier>tests</classifier>
    <scope>test</scope>
</dependency>
```
3. 在janusgraph-hadoop-parent/janusgraph-hadoop-core的pom文件build plugins中增加shade相关配置，重定义guava库。
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.2.1</version>
    <executions>
        <execution>
            <id>shade-gremlin-groovy</id>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <createDependencyReducedPom>false</createDependencyReducedPom>
                <artifactSet>
                    <includes>
                        <include>com.google.guava:guava</include>
                    </includes>
                </artifactSet>
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/MANIFEST.MF</exclude>
                        </excludes>
                    </filter>
                </filters>
                <relocations>
                    <relocation>
                        <pattern>com.google.guava</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_guava</shadedPattern>
                    </relocation>
                    <relocation>
                        <pattern>com.google.common</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_common</shadedPattern>
                    </relocation>
                    <relocation>
                        <pattern>com.google.thirdparty</pattern>
                        <shadedPattern>org.janusgraph.shaded.google_thirdparty</shadedPattern>
                    </relocation>
                </relocations>
                <!-- false below means the shade plugin overwrites the main project artifact (the one with no classifier).
                     false does *not* actually detach the main artifact, despite what the option name suggests. -->
                <shadedArtifactAttached>true</shadedArtifactAttached>
                <minimizeJar>false</minimizeJar>
            </configuration>
        </execution>
    </executions>
</plugin>
```
4. 修改janusgraph-hadoop的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hadoop-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
5. 修改janusgraph-hadoop-2的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hadoop-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
6. 修改janusgraph-hadoop-core的pom文件中parent版本号，与上一级相符。
```
<parent>
    <groupId>org.janusgraph</groupId>
    <artifactId>janusgraph-hadoop-parent</artifactId>
    <version>0.2.3-shade</version>
    <relativePath>../pom.xml</relativePath>
</parent>
```
7. 修改janusgraph-hadoop-core的pom文件中的依赖，janusgraph-es/janusgraph-hbase-core改为0.2.3-shade，其他的janusgraph相关依赖不再依赖${project.version}版本，改为0.2.3版本。
```
<dependency>
    <groupId>${project.groupId}</groupId>
    <artifactId>janusgraph-cassandra</artifactId>
    <version>0.2.3</version>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
    <!-- TODO this should be scoped test -->
</dependency>
<dependency>
    <groupId>${project.groupId}</groupId>
    <artifactId>janusgraph-hbase-core</artifactId>
    <version>0.2.3-shade</version>
</dependency>
<dependency>
    <groupId>${project.groupId}</groupId>
    <artifactId>janusgraph-cassandra</artifactId>
    <version>0.2.3</version>
    <classifier>tests</classifier>
    <exclusions>
        <exclusion>
            <groupId>org.janusgraph</groupId>
            <artifactId>janusgraph-core</artifactId>
        </exclusion>
    </exclusions>
    <!-- TODO this should be scoped test -->
</dependency>
<dependency>
    <groupId>${project.groupId}</groupId>
    <artifactId>janusgraph-es</artifactId>
    <version>0.2.3-shade</version>
    <classifier>tests</classifier>
    <scope>test</scope>
</dependency>
```
8. 对janusgraph-hadoop-parent的pom文件使用maven install打出新的包，得到janusgraph-hadoop/target下janusgraph-hadoop-0.2.3-shade.jar。

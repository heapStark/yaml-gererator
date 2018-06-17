# 一个简单的yaml文档生成工具
```
之前在京东云参与接入OpenApi网关改造,需要提供yaml文档给网关组生成sdk
为了减少工作量以及后面方便维护,实现了一个简单的yaml文档生成工具
```
```
由于泛型的问题，当前版本仅支持jdk1.8以及spring4.2
所有controller类应当满足示例代码中的注解规范
由于基于包扫描查找controller,mvn仓库中应当首先install对应的工程,
可以增加一个单独的profile引入插件以及插件依赖
``` 
## 一个简单的示例工程

[demo](https://github.com/heapStark/yaml-generator-demo)  "示例代码"
## todoList & problem
不支持在yaml文档中输出注释
解决方案1：定义注释注解,这种方案对代码侵入太高
## 使用说明,一个pom文件实例

```
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <classpathPrefix></classpathPrefix>
                            <mainClass>com.xx.xx.xx</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy</id>
                        <phase>install</phase>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>
                                ${project.build.directory}
                            </outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>heap-stark</groupId>
                <artifactId>yaml-generator</artifactId>
                <version>0.1-SNAPSHOT</version>
                <configuration>
                    <controllerPackage>heap.stark.controller.api</controllerPackage>
                    <resultPath>/home/wzl/Downloads/heap-stark/src/main/resources
                    </resultPath>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>heap-stark</groupId>
                        <artifactId>heap-stark-web</artifactId>
                        <version>1.0-SNAPSHOT</version>
                        <type>jar</type>
                    </dependency>
                </dependencies>
                <executions>
                    <execution>
                        <id>first</id>
                        <phase>compile</phase>
                        <goals>
                            <goal>yaml</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```



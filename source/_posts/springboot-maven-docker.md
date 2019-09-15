title: springboot使用maven打包docker
date: 2019-03-05 18:09:23
tags: spring,maven,docker
---

# Springboot项目配置Maven插件打包Docker镜像

1. 创建Dockerfile配置文件
2. 在pom.xml中添加插件
3. 需要注意的地方
<!-- more -->

## 增加Dockerfile
由于springboot打出的包是一个fatjar,为了提高docker的打包速度,可以将lib等一些不太经常变化的资源文件单独做一个层出来。而fatjar怎么来`unpack`下面会提到，大体是使用`maven-dependency-plugin`插件的`unpack`。

创建的`Dockerfile`放置于项目的根目录下。

```Docker
FROM openjdk:8-jdk-alpine
ARG DEPENDENCY=target/dependency
# 配置时区
RUN apk add -U tzdata
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN apk del tzdata
# docker分层后前两层基本不变可加快打包速度
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
# springboot的启动类
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-cp","app:app/lib/*","xxx.CommonApplication"]
```

## 增加plugin
添加`maven-dependency-plugin`和`maven-dependency-plugin`两个插件如下：

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <fork>true</fork>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>unpack</id>
            <phase>package</phase>
            <goals>
                <goal>unpack</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>com.spotify</groupId>
    <artifactId>dockerfile-maven-plugin</artifactId>
    <version>1.4.9</version>
    <configuration>
        <repository>${docker.image.prefix}/${project.artifactId}</repository>
    </configuration>
</plugin>
```

## 注意事项

- maven插件顺序

从上至下依次为
1. `spring-boot-maven-plugin`
2. `maven-dependency-plugin`
3. `dockerfile-maven-plugin`

由于`spring-boot-maven-plugin`中的`repackage`会把springboot项目打成一个fatjar，它必须在`maven-dependency-plugin`的`unpack`之前执行，否则`target/dependency`目录中的文件结构将不会包含`BOOT-INF`和`META-INF`目录，造成后面Docker进行build时失败

- windows中需要配置`docker`暴露出`tcp://localhost:2375`，否则无法build
- 清除`~/.docker`下的所有`.pem`文件（windows中是在`C:\Users\{账号}\.docker`），否则会出现`javax.net.ssl.SSLException: Unrecognized SSL message, plaintext connection?`异常

## 参考:
https://spring.io/guides/gs/spring-boot-docker/
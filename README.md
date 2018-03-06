1、spring-cloud-config-server

pom：

    <dependency>
    	<groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>

application.yml配置

    spring:
      application:
        name:
          configserver

      cloud:
        config:
          server:
            git:
              uri:https://github.com/ZuckerChen/config-repo

编写启动类(@EnableConfigServer)

    package cloud;

    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.boot.builder.SpringApplicationBuilder;
    import org.springframework.cloud.config.server.EnableConfigServer;

    /**
     * Description:
     *
     * @version 1.0 17/11/27 上午9:49 by zucker
     */
    @SpringBootApplication
    @EnableConfigServer
    public class ApplicationConfigServer {
        public static void main(String[] args) {
            new SpringApplicationBuilder(ApplicationConfigServer.class)
                    .properties("spring.config.name=configserver").run(args);
        }
    }


应用和数据获取映射规则

- application:文件名
- label:git上的分支名
- profile:环境变量名 默认default

  获取git上的资源信息遵循如下规则
  ```
    /{application}/{profile}[/{label}]
    /{application}-{profile}.yml
    /{label}/{application}-{profile}.yml
    /{application}-{profile}.properties
    /{label}/{application}-{profile}.properties
  ```




2、spring-cloud-config-client

bootstrap.yml

    spring:
      cloud:
        config:
         uri: http://localhost:8888
         profile: default #对应业务名称的profile
         label: master #当配置文件在git上时为分支名
      application:
        name: foobar  #对应文件的业务名称



client

    package com.clsaa.springcloud.config;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class EurekaClientAppication {
        public static void main(String[] args) {
            SpringApplication.run(EurekaClientAppication.class, args);
        }
    }

3、git仓库配置

TODO

4、加解密

TODO

5、用户认证

server:

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    server:
      port: 8888
    spring:
      cloud:
        config:
          server:
            git:
              uri: https://github.com/ZuckerChen/config-repo
    security:
      basic:
        enabled: true
      user:
        name: user
        password: password

client:

    spring:
      cloud:
        config:
         uri: http://user:password@localhost:8080
         profile: dev #对应业务名称的profile
         label: master #当配置文件在git上时为分支名
      application:
        name: foobar  #对应文件的业务名称



6、通过Eureka发现configserver

TODO

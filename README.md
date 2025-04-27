# microservicenote
#############################################################################################
### APIS
spring.cloud.config.enabled=true
spring.config.import=optional:configserver:http://localhost:9999

spring.cloud.config.enabled=true
spring.config.import=optional:configserver:http://localhost:8888

<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>


#############################################################################################
### config-service :

@EnableConfigServer

server.port=9999
spring.cloud.config.server.git.uri=https://github.com/zakariachafi/config-fils

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>


#############################################################################################
#### discovery service:

@EnableEurekaServer

server.port=8761
eureka.client.fetch-registry=false
eureka.client.register-with-eureka=false

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>


#############################################################################################
### gateway service :

@Bean
    DiscoveryClientRouteDefinitionLocator locator(
            ReactiveDiscoveryClient rdc, DiscoveryLocatorProperties dlp){
        return new DiscoveryClientRouteDefinitionLocator(rdc,dlp);
    }

server.port=8888
spring.cloud.config.enabled=false
spring.cloud.discovery.enabled=true
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
eureka.instance.prefer-ip-address=true
spring.cloud.gateway.discovery.locator.lower-case-service-id=true



spring:
  cloud:
    gateway:
      routes:
        - id: r1
          uri: lb://CUSTOMER-SERVICE
          predicates:
            Path= /api/customers/**
        - id: r2
          uri: lb://INVENTORY-SERVICE
          predicates:
            Path= /api/products/**


 <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

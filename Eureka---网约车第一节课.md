#### 概念

服务管理

注册中心，服务提供者将服务名、ip地址等信息注册到Eureka，服务消费者定时获取注册信息表到缓存，访问其他服务。

使用RESTful请求

#### 使用

##### 单节点

1. pom 依赖 sprin-cloud-starter-netflix-eureka-server

2. application.yml/properties 文件配置属性

   ```sh
   eureka:
   	client:
   	 #是否将自己注册到Eureka Server (Eureka本身也是service 也需要注册),默认为true，由于当前就是server，故而设置成false，表明该服务不会向eureka注册自己的信息
   		register-with-eureka: false
   		#是否从eureka server获取注册信息，由于单节点，不需要同步其他节点数据，用false
   		fetch-registry: false
   		service-url:
   			defaultZone: http://root:root@eureka-7901:7901/eureka
   ```

   

3. class上 @EnableEurekaServer

   上面例子中如果service-url为空，且register-with-eureka，fetch-registry为true，则会报错，Cannot execute request on any known server，因为server同时也是一个client，他会尝试注册自己，所以要有一个注册中心url去注册。

##### 多节点 

1. pom依赖
2. yml/properties 文件配置属性 多个server url用“,” 隔开
3. @

##### client

###### 功能

1. 注册：每个微服务启动时会将自己的IP地址等信息注册到注册中心
2. 获取服务注册表：服务消费者从注册中心，查询服务提供者的网络地址，并使用该地址调用服务提供者，为了避免每次都查注册表信息，所以client会定时去server拉取注册表信息到缓存到client本地。
3. 心跳：各个微服务与注册中心通过某种机制（心跳）通信，若注册中心长时间和服务间没有通信，就会注销该实例。
4. 调用：实际的服务调用，通过注册表，解析服务名和具体地址的对应关系，找到具体服务的地址，进行实际调用.

###### 服务注册

pom.xml

```<dependency>
		<dependency>			
						<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
```

application.yml

```sh
#注册中心
eureka: 
  client:
    #设置服务注册中心的URL
    service-url:                      
      defaultZone: http://root:root@localhost:7900/eureka/
```

除了依赖client pom外 也可以通过post请求注册服务到Eureka


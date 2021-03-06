## Config server

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigApplication {
	public static void main(String[] args) {
		SpringApplication.run(ConfigApplication.class, args);
	}
}

-------------------
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.csrf().disable();
		http.authorizeRequests().antMatchers("/actuator/**").permitAll().anyRequest().authenticated().and().httpBasic();
	}
}
```
```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-config-server</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
	</dependencies>
```
```yml
#src/main/resource/application.yml
spring:
  cloud:
    config:
      server:
        native:
          search-locations: classpath:/shared
  profiles:
     active: native
  security:
    user:
      name: jethro
      password: jethro

server:
  port: 8888  
--------------------
#src/main/resource/shared/application.yml
logging:
  level:
    org.springframework.security: INFO
--------------------
# hi-service-dev.yml
server:
  port: 6001
```
> mvn spring-boot:run   
> http://localhost:8888/hi-server-dev.yml   user/jethro  
> http://localhost:8888/hi-server/dev   


## eureka
```java
@SpringBootApplication
@EnableEurekaServer
public class RegistryApplication {
	public static void main(String[] args) {
		SpringApplication.run(RegistryApplication.class, args);
	}
}
```
```yml
spring:
  application:
    name: eureka
  cloud:
    config:
      uri: http://config:8888
      fail-fast: true
      username: jethro
      password: jethro
eureka:
  instance:
    prefer-ip-address: true
  client:
    registerWithEureka: false
    fetchRegistry: false
    server:
      waitTimeInMsWhenSyncEmpty: 0
```



## Reference
https://github.com/sqshq/PiggyMetrics  

https://github.com/cloudframeworks-springcloud/user-guide-springcloud/blob/master/README_CN.md   

https://github.com/ityouknow/awesome-spring-cloud

https://cloud.spring.io/spring-cloud-config/single/spring-cloud-config.html
 


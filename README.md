# Spring Boot 接入 Prometheus

---

新建项目，使用以下配置。

![](image/1.png)

已有项目添加以下依赖。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
    <scope>runtime</scope>
</dependency>
```

在`application.properties`中添加以下配置，`spring.application.name`属性必须有，一般为自己项目名称。

```properties
spring.application.name=spring-boot-prometheus-demo

#Metrics related configurations
management.endpoint.metrics.enabled=true
management.endpoints.web.exposure.include=*
management.endpoint.prometheus.enabled=true
management.metrics.export.prometheus.enabled=true
```

启动项目，访问`http://localhost:8080/actuator/prometheus `，看看是否有数据，如果有数据，则表示接入成功。

配置Prometheus。

```yaml
  - job_name: 'spring-boot-prometheus-demo.jvm'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ["localhost:8081"]
        labels:
          env: 'Prod'
          app: 'spring-boot'
```

然后配置`Grafana`，配置Prometheus数据源，导入ID是`4701`的Dashboard即可，[地址](https://grafana.com/grafana/dashboards/4701)。

效果如下。

![](image/p4.png)
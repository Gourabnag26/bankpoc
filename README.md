##### Spring DataSource Configs #####
spring.r2dbc.url=r2dbc:postgresql://localhost:5432/mydb?schema=customer
spring.r2dbc.username=postgres
spring.r2dbc.password=admin

spring.liquibase.driver-class-name=org.postgresql.Driver
spring.liquibase.url=jdbc:postgresql://localhost:5432/postgres
spring.liquibase.user=admin
spring.liquibase.password=admin
spring.liquibase.enabled=true

spring.r2dbc.pool.enabled=true

app.security.auth.header.value=test

server.port=9002

##### AWS Configs #####
app.aws.config.us-east-1.event-queue-name=ipa-d-1-app-use1-fifo-event-sqs.fifo
app.aws.config.us-east-1.ms-api-host=https://ms-alb-use1.ipa.dev1.r53comerica.net
app.aws.config.us-east-2.event-queue-name=ipa-d-1-app-use2-fifo-event-sqs.fifo
app.aws.config.us-east-2.ms-api-host=https://ms-alb-use2.ipa.dev1.r53comerica.net

#### Redis Cache Config #####
spring.cache.type=redis
spring.data.redis.ssl.enabled=false
spring.data.redis.host=localhost
spring.data.redis.port=6379

logging.level.liquibase= DEBUG

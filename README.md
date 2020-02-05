# quarkus-edu

mvn clean package -DuberJar=true -DskipTests
oc new-project quarkus-spring --display-name="Sample Quarkus App using Spring APIs"
oc new-app \

    -e POSTGRESQL_USER=sa \
    
    -e POSTGRESQL_PASSWORD=sa \
    
    -e POSTGRESQL_DATABASE=fruits \
    
    -e POSTGRESQL_MAX_CONNECTIONS=200 \
    
    --name=postgres-database \
    
    openshift/postgresql

oc new-build registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.5 --binary --name=fruit-taster

oc start-build fruit-taster --from-file target/*-runner.jar --follow

oc new-app fruit-taster \

   -e QUARKUS_DATASOURCE_URL=jdbc:postgresql://postgres-database:5432/fruits && \

oc expose svc/fruit-taster

oc rollout status -w dc/fruit-taster

curl -s http://fruit-taster-quarkus-spring.2886795281-80-kitek03.environments.katacoda.com/taster | jq

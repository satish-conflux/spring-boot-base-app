# spring-boot-base-app
Basic spring boot application with standard libraries for experimentation

## Getting started

Pre-requisites
- Java 1.8
- Docker
- Docker-compose

### Installation (debian base)

#### Install Docker

    apt-get install apparmor lxc cgroup-lite
    wget -qO- https://get.docker.com/ | sh
    sudo usermod -aG docker YourUserNameHere
    sudo service docker restart

#### Install Docker-compose  (1.6+)

*MAKE SURE YOU HAVE AN UP TO DATE VERSION OF DOCKER COMPOSE*

To check the version:

    docker-compose --version

To install the 1.6.0:

    sudo su
    curl -L https://github.com/docker/compose/releases/download/1.6.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    exit

#### Eclipse notes
Necessary changes to exclude code from the automatic formatter can be found here
Note. Eclipse formatting in Mars appears to be broken (http://stackoverflow.com/questions/31048040/how-to-force-the-eclipse-mars-4-5-formatter-not-to-join-already-wrapped-lines)

http://www.eclipseonetips.com/2013/10/15/disable-eclipse-formatting-for-certain-sections-of-code-only/

Eclipse formatter and import order can be found under `config/checkstyle`

## To run

    docker-compose up

### To run docker mysql while running app locally

    docker-compose up mysql
    ./gradlew bootRun

## Features

Includes the following
- Spring Actuator (\actuator content)
  - Populated \info endpoint
  - Custom \git endpoint (enabled by default)
- Docker support with docker-compose
- Flyway migrations
- Full HAL & APLS REST API
  - Validation example
  - Lifecycle hook example (pre-persist)
  - Timestamp auditing (JPA Entity auditing)
- Integration testing via RestAssured

## Refs
### spring-data-rest
http://docs.spring.io/spring-data/rest/docs/current/reference/html/

## Validation notes
http://blog.codeleak.pl/2014/06/better-error-messages-with-bean.html
http://docs.spring.io/spring/docs/current/spring-framework-reference/html/validation.html
http://stackoverflow.com/questions/24318405/spring-data-rest-validator
http://www.bernhardwenzel.com/blog/2014/10/10/spring-boot-rest-knockout-bookmarks#the-domain-model
http://stackoverflow.com/questions/32349479/what-is-the-best-way-to-implement-custom-validation-in-spring-application

## JPA Parent -> Child heirarchical entity relationship
http://viralpatel.net/blogs/hibernate-self-join-annotations-one-to-many-mapping/

## JPA Lifecycle handle hooks
Interesting approach if you need 'dirty' object values
http://stackoverflow.com/questions/25283767/how-to-use-spring-managed-hibernate-interceptors-in-spring-boot
https://guylabs.ch/2014/02/22/autowiring-pring-beans-in-hibernate-jpa-entity-listeners/
http://www.javabeat.net/jpa-entity-listeners-callback-methods/

## Testing
http://www.jayway.com/2013/12/10/json-schema-validation-with-rest-assured/
http://www.jayway.com/2014/01/14/unit-testing-spring-mvc-controllers-with-rest-assured/
https://github.com/flyway/flyway-test-extensions/wiki/Usage-flyway-spring-test
https://github.com/Hippoom/spring-test-dbunit-template

http://g00glen00b.be/spring-boot-rest-assured/
http://www.javabeat.net/spring-boot-testing/
http://blog.czeczotka.com/2015/01/20/spring-mvc-integration-test-with-rest-assured-and-mockmvc/
http://www.joecolantonio.com/2014/02/26/rest-testing-with-java-part-two-getting-started-with-rest-assured/

## Logging
https://mdeinum.wordpress.com/2015/07/01/spring-framework-hidden-gems/

## Git
http://www.petrikainulainen.net/programming/spring-framework/spring-from-the-trenches-returning-git-commit-information-as-json/
https://github.com/n0mer/gradle-git-properties/
http://www.jayway.com/2014/02/16/spring-propertysource/

## Access notes
alps+json
hal+json
schema+json
### JSON Schema
curl -H 'Accept:application/schema+json' http://localhost:8080/profile/persons
### ALPS
curl http://localhost:8080/profile/persons
### Post new record
curl -i -X POST -H "Content-Type:application/json" -d '{  "firstName" : "Frodo",  "lastName" : "Baggins" }' http://localhost:8080/person/

curl -i -X POST -H "Content-Type:application/json" -d '{  "person" : "http://localhost:8080/people/3",  "data" : "I had a thought!" }' http://localhost:8080/thoughts

curl -i -X POST -H "Content-Type:application/json" -d '{  "firstName" : "Frodo",  "lastName" : "Baggins", "thoughts" : [{"data":"My precious"},{"data":"One ring to rule them all"}] }' http://localhost:8080/person

curl -i -X POST -H "Content-Type:application/json" -d '{  "firstName" : "Frodo1",  "lastName" : "Baggins1", "children" : [{  "firstName" : "Samwise",  "lastName" : "Gangees"}] }' http://localhost:8080/person


## Credits/ Contributions

Bill Koch (https://github.com/billkoch)
- Integration test improvements and fixes.


## To Investigate

- Seperation of unit and integration tests
- Jenkins
- Auto reload
- Lombock
- Profiles
- Logging
- Statsd compatibility (fix api logging and add system level logging per host)
- Multi db support
- Redis caching
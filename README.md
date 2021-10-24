# Knowledge - content

- [Tools](https://github.com/andrzejsydor/knowledge#tools)

-- [Collaborative Tools](collaborative.md)

-- [JVM](jvm.md)

-- [IntelliJ](intellij.md)

-- [Docker](Docker.md)

-- [Git](git.md)

-- [Browser Plugins](browser-plugins.md)

-- [WSL](wsl.md)

- [dev](https://github.com/andrzejsydor/knowledge#dev)

- [repos](https://github.com/andrzejsydor/knowledge#repos)

- [Problems](https://github.com/andrzejsydor/knowledge#problems)

- [Links](https://github.com/andrzejsydor/knowledge#links)

## pages

- [Books](https://github.com/andrzejsydor/knowledge/blob/main/BOOKS.md)

# Tools

[PlantUML](https://plantuml.com/download)

[NoSQLBooster](https://nosqlbooster.com)

[MobaXterm](https://mobaxterm.mobatek.net/)

## Visual Studio Code

[GitLens](https://gitlens.amod.io/)

## Online

[Structurizr](https://structurizr.com/)

## Windows

[Double Commander](https://doublecmd.sourceforge.io/)

## Code Quality

[checkstyle](https://checkstyle.sourceforge.io/)

[ArchUnit](https://www.archunit.org/)

[modernizer-maven-plugin](https://github.com/gaul/modernizer-maven-plugin)

[SpotBugs (a new FindBugs)](https://spotbugs.github.io/)

[PMD](https://pmd.github.io/)

[JaCoCo](https://www.jacoco.org/)

[mutation testing](https://pitest.org/)

# dev

## Microservices

### Resilience

[Resilience Patterns](https://github.com/resilience4j/resilience4j#4-resilience-patterns)

## Kafka

[Kafka Visualisation](https://softwaremill.com/kafka-visualisation/)

# repos

[https://github.com/Hakky54/log-captor](https://github.com/Hakky54/log-captor)

# Problems

## zip with encrypt

```sh
zip -er directory output
```

## curl: Unknown error

[curl: Unknown error (0x80092012) - The revocation function was unable to check revocation for the certificate](https://stackoverflow.com/questions/54938026/curl-unknown-error-0x80092012-the-revocation-function-was-unable-to-check-r)

```
curl "https://www.example.com" --ssl-no-revoke -x 127.0.0.1:8081
```

## How to check user id & group id?

```
cat /etc/passwd
cat /etc/group
```

## MySQL Docker backup & restore


```
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql
```

```
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```

## Check processor (MacOS)

```
sysctl -a | grep brand_string
sysctl -a | grep Processor
```

# Links

[JavaMagazine](https://blogs.oracle.com/javamagazine)

[https://github.com/s4kibs4mi/java-developer-roadmap](https://github.com/s4kibs4mi/java-developer-roadmap)

[BigO Notation](https://bigocheatsheet.io/)

[https://bigocheatsheet.com](https://bigocheatsheet.com/)

[Web Developer RoadMap](https://github.com/kamranahmedse/developer-roadmap)

[Web Development Resources](https://github.com/markodenic/web-development-resources)

[Tech Interview Handbook](https://github.com/yangshun/tech-interview-handbook)

## Awesome

[Awesome Java](https://github.com/akullpp/awesome-java)

[Awesome Design Pattern](https://github.com/DovAmir/awesome-design-patterns)

[Awesome Interview Questions](https://github.com/DopplerHQ/awesome-interview-questions)

[Awesome Microservices](https://github.com/mfornos/awesome-microservices)

[Awesome REST](https://github.com/marmelab/awesome-rest)

[Awesome Cheatsheets](https://github.com/LeCoupa/awesome-cheatsheets)


## Hibernate

[Spring Data JPA – How to Return DTOs from Native Queries](https://thorben-janssen.com/spring-data-jpa-dto-native-queries/)

[The JPA and Hibernate second-level cache](https://vladmihalcea.com/jpa-hibernate-second-level-cache/)

## Other

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

[https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)

[cheatsheet-kubernetes-A4](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)

### free blog services

[weebly](https://www.weebly.com/)

[Blogger](https://www.blogger.com/)


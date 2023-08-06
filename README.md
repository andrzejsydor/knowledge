# Knowledge - content

- [Tools](https://github.com/andrzejsydor/knowledge#tools)

-- [Collaborative Tools](collaborative.md) / [JVM](jvm.md) / [IntelliJ](IntelliJ/IntelliJ.md) / [Docker](Docker.md) / [Git](git.md) / [Hibernate](Hibernate/Hibernate.md) / [Browser Plugins](browser-plugins.md) / [MacOS](macos.md) / [WSL](wsl.md)

- [dev](https://github.com/andrzejsydor/knowledge#dev)

- [repos](https://github.com/andrzejsydor/knowledge#repos)

- [Problems](https://github.com/andrzejsydor/knowledge#problems)

- [Links](https://github.com/andrzejsydor/knowledge#links)

- [Courses](https://github.com/andrzejsydor/knowledge#courses)

- [Tags](https://github.com/andrzejsydor/knowledge#tags)

## pages

- [Books](https://github.com/andrzejsydor/knowledge/blob/main/BOOKS.md)

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

# Tools

[PlantUML](https://plantuml.com/download)

[NoSQLBooster](https://nosqlbooster.com)

[MobaXterm](https://mobaxterm.mobatek.net/)

[Postman](https://www.postman.com/)

[httpie](https://httpie.io/)

[SoapUI](https://www.soapui.org/downloads/soapui/)

[JMeter](JMeter/JMeter.md)

[Locust](https://locust.io/)

[JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html)

[Structurizr](https://structurizr.com/pricing)

[Snapwatch](https://snapwatch.io/)

[JRebel](https://www.jrebel.com/)

## Visual Studio Code

[GitLens](https://gitlens.amod.io/)

## Online

[Structurizr](https://structurizr.com/)

## [Windows](windows.md)

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

## [Kafka](Kafka.md)

## dev - tools

[togglz - Feature Flags for the Java platform](https://www.togglz.org/)

[openfeature](https://openfeature.dev)

[jOOQ](https://www.jooq.org/)

# repos

[https://github.com/Hakky54/log-captor](https://github.com/Hakky54/log-captor)

# Tips & Problems

## log4j & gradle find dependency

[Log4Shell Remediation CheatSheet - Snyk](https://snyk.io/wp-content/uploads/cheat-sheet-log4shell-remediation-v6.pdf)

### dependencyInsight

Single module project:
`gradle dependencyInsight --dependency log4j-core`

Multi module project:

```
gradle :app:dependencyInsight --dependency log4j-core
gradle :background:dependencyInsight --dependency log4j-core
```

### Custom `findDependency` task

In the `build.gradle` add:

```
task findDependency(type: DependencyInsightReportTask) {}
```

then
`gradle findDependency --configuration compile --dependency log4j`

For multi-module projects:

```
allprojects {
    task findDependency(type: DependencyInsightReportTask) {}
}
```

## zip with encrypt

```sh
zip -er directory output
zip -r -e output.zip /path/to/folder/
```

### unzip

```sh
zip -r -e -P 'YOUR_PASSWORD_HERE' output.zip /path/to/folder/
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

[Java Concurrency Patterns and Features](https://github.com/LeonardoZ/java-concurrency-patterns)

[List of Free Learning Resources In Many Languages](https://github.com/EbookFoundation/free-programming-books)

[The Twelve-Factor App](https://12factor.net/)

## Awesome

[Awesome Java](https://github.com/akullpp/awesome-java)

[Awesome Software Architecture](https://github.com/mehdihadeli/awesome-software-architecture)

[Awesome Design Pattern](https://github.com/DovAmir/awesome-design-patterns)

[Awesome Interview Questions](https://github.com/DopplerHQ/awesome-interview-questions)

[Awesome Microservices](https://github.com/mfornos/awesome-microservices)

[Awesome REST](https://github.com/marmelab/awesome-rest)

[Awesome Cheatsheets](https://github.com/LeCoupa/awesome-cheatsheets)

## Other

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

[https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)

[cheatsheet-kubernetes-A4](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)

[JHipster](https://github.com/jhipster)



### free blog services

[weebly](https://www.weebly.com/)

[Blogger](https://www.blogger.com/)

# Courses

[90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps/blob/main/Days/day49.md)

# Tags

[#docker](https://github.com/andrzejsydor/knowledge/search?q=%23docker)

# Slack

- jvm-poland.slack.com

- kubernetes.slack.com

- techstudyslack.slack.com

- tutorialsdojo.slack.com

- cloud-native.slack.com

- tech-leaders-hq.slack.com



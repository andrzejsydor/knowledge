# Knowledge - content

- [Tools](https://github.com/andrzejsydor/knowledge#tools) 

[HELP/Obsidian](./HELP/Obsidian.md)

-- [Collaborative Tools](collaborative.md) / [JVM](jvm.md) / [IntelliJ](./dev/IntelliJ/IntelliJ.md) / [Docker](Docker.md) / [Git](dev/git.md) / [Hibernate](Hibernate.md) / [Browser Plugins](browser-plugins.md) / [MacOS](macos.md) / [WSL](wsl.md)

- [dev](https://github.com/andrzejsydor/knowledge#dev)

- [repos](https://github.com/andrzejsydor/knowledge#repos)

- [Problems](https://github.com/andrzejsydor/knowledge#problems)

- [Links](https://github.com/andrzejsydor/knowledge#links)

- [Courses](https://github.com/andrzejsydor/knowledge#courses)

- [Blogs](https://github.com/andrzejsydor/knowledge#blogs)

- [Awesome](https://github.com/andrzejsydor/knowledge/blob/main/AWESOME.md)

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

[CATS](https://github.com/andrzejsydor/cats) - OpenAPI - REST API fuzzer and negative testing tool. Run thousands of self-healing API tests within minutes with no coding effort!

[JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html)

[Structurizr](https://structurizr.com/pricing)

[JRebel](https://www.jrebel.com/)

## Visual Studio Code

[GitLens](https://gitlens.amod.io/)

[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

[DevPod](https://devpod.sh/)

## Online

[Structurizr](https://structurizr.com/)


## Code Quality

[checkstyle](https://checkstyle.sourceforge.io/)

[ArchUnit](https://www.archunit.org/)

[modernizer-maven-plugin](https://github.com/gaul/modernizer-maven-plugin)

[SpotBugs (a new FindBugs)](https://spotbugs.github.io/)

[PMD](https://pmd.github.io/)

[JaCoCo](https://www.jacoco.org/)

[mutation testing](https://pitest.org/)

# dev

[Basic](./Architecture/Basic.md)

[Architecture](./Architecture/Architecture.md)

## dev roadmaps

[https://github.com/andrzejsydor/developer-roadmap](https://github.com/andrzejsydor/developer-roadmap)

[Spring Boot Developer](https://roadmap.sh/spring-boot)

[Backend Developer](https://roadmap.sh/backend)

[AWS Roadmap](https://roadmap.sh/aws)

## Microservices

### Resilience 

[Resilience Patterns](https://github.com/resilience4j/resilience4j#4-resilience-patterns)

| Name            | Mechanism                                  | Explanation                                                                                |
|-----------------|--------------------------------------------|--------------------------------------------------------------------------------------------|
| Retry           | Attempts a failed operation again          | Transient errors may resolve on their own after a short delay.                             |
| Circuit Breaker | Temporarily stops triggering failing calls | Prevents exhausted resources by failing fast instead of waiting on an overwhelmed system.  |
| Rate Limiter    | Restricts the number of incoming requests  | Controls incoming requests to avoid overloading the system.                                |
| Time Limiter    | Caps how long an operation can run         | If it takes too long, the chance of success diminishes, so waiting further is impractical. |
| Bulkhead        | Separates resources into independent pools | One service failure won’t spill over into other areas, improving overall resilience.       |
| Cache           | Stores results of past successful calls    | Reuses outcomes for repeated or similar requests, reducing duplicate processing.           |
| Fallback        | Provides an alternative response           | Even with failures, having a plan for a substitute solution helps maintain functionality.   |

## [README](Kafka/README.md)

## dev - tools

[togglz - Feature Flags for the Java platform](https://www.togglz.org/)

[openfeature](https://openfeature.dev)

[jOOQ](https://www.jooq.org/)

### dev - observability stack tools

[OpenTelemetry](https://opentelemetry.io/) - for tracing, he also has an OTEL collector container running locally to route the data to the various tools.

[Jaeger](https://www.jaegertracing.io/) - for visualizing traces

[Micrometer](https://micrometer.io/) - for collecting metrics

[Prometheus](https://prometheus.io/) - for saving matrics

[Grafana](https://grafana.com/oss/) -  for visualizing metrics


# repos

[https://github.com/Hakky54/log-captor](https://github.com/Hakky54/log-captor)

# Tips & Problems

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

# Links #Link 

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


## Other

[https://architecture-antipatterns.tech/](https://architecture-antipatterns.tech/)

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

[https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)

[cheatsheet-kubernetes-A4](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)

[JHipster](https://github.com/jhipster)



### free blog services

[weebly](https://www.weebly.com/)

[Blogger](https://www.blogger.com/)

# Courses

[Patoarchitekci](https://patoarchitekci.io/)

[Developer to Architect](https://www.developertoarchitect.com/)

[90DaysOfDevOps](https://github.com/MichaelCade/90DaysOfDevOps)

[@ByteByteGo](https://www.youtube.com/@ByteByteGo)

[@ByteMonk](https://www.youtube.com/@ByteMonk)

[Trainingportal Linuxfoundation](https://trainingportal.linuxfoundation.org/)

# Blogs #Link

[AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/)

[stripe](https://stripe.com/blog)

[slack engineering](https://slack.engineering/)

[Discord Blog](https://discord.com/category/engineering)

[LinkedIn Engineering Blog](https://www.linkedin.com/blog/engineering)

[Engineering at Meta](https://engineering.fb.com/)

[The Cloudflare Blog](https://blog.cloudflare.com/tag/developers)

[The Netflix Tech Blog](https://netflixtechblog.com/)


# Tags


```dataview
TABLE WITHOUT ID (tag + "(" + length(rows.file.link) + ")") AS Tags, join(rows.file.folder) AS Folder, 
join(rows.file.link, ", ") AS Files 
FROM "" 
WHERE file.tags FLATTEN file.tags AS tag 
GROUP BY tag 
SORT length(rows.file.link) DESC
```



# Slack

- jvm-poland.slack.com

- kubernetes.slack.com

- techstudyslack.slack.com

- tutorialsdojo.slack.com

- cloud-native.slack.com

- tech-leaders-hq.slack.com

# Discord




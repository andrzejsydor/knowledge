# Knowledge - content

- [Tools](https://github.com/andrzejsydor/knowledge#tools)

- [dev](https://github.com/andrzejsydor/knowledge#dev)

- [repos](https://github.com/andrzejsydor/knowledge#repos)

- [Problems](https://github.com/andrzejsydor/knowledge#problems)

- [Links](https://github.com/andrzejsydor/knowledge#links)

## pages

- [Books](https://github.com/andrzejsydor/knowledge/blob/main/BOOKS.md)

# Tools

[NoSQLBooster](https://nosqlbooster.com)

[MobaXterm](https://mobaxterm.mobatek.net/)

## [IntelliJ](intellij.md)

## Visual Studio Code

[GitLens](https://gitlens.amod.io/)

## Online

[Structurizr](https://structurizr.com/)

## Windows

[Double Commander](https://doublecmd.sourceforge.io/)

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

## MySQL Docker backup & restore


```
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql
```

```
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```

# Links

[JavaMagazine](https://blogs.oracle.com/javamagazine)

[https://github.com/s4kibs4mi/java-developer-roadmap](https://github.com/s4kibs4mi/java-developer-roadmap)

[BigO Notation](https://bigocheatsheet.io/)

[https://bigocheatsheet.com](https://bigocheatsheet.com/)


## Other

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

[https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)

[cheatsheet-kubernetes-A4](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)

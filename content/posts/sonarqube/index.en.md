---
weight: 4
title: "Implementation of SonarQube in a Spring Boot application"
date: 2024-01-18T21:55:11+02:00
draft: false
description: "SonarQube"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["SonarQube"]
categories: ["SonarQube"]

lightgallery: true**
---
Implementation of SonarQube in a Spring Boot application.
<!--more-->

## What is SonarQube?

SonarQube is an open-source platform for static code analysis that assists developers and teams in monitoring and improving
the quality of their source code. The main objectives of SonarQube include identifying code smells, bugs, and security
vulnerabilities, as well as monitoring code quality over time.

<br>

## Create docker-compose.yml

<strong>docker-compose.yml</strong>
``` yml
version: "3.8"
services:
    sonarqube:
        container_name: sonarqube
        image: sonarqube
        depends_on:
            - sonarqube-database
        environment:
            - SONARQUBE_JDBC_USERNAME=sonarqube
            - SONARQUBE_JDBC_PASSWORD=sonarpass
            - SONARQUBE_JDBC_URL=jdbc:postgresql://sonarqube-database:5432/sonarqube
        volumes:
            - sonarqube_conf:/opt/sonarqube/conf
            - sonarqube_data:/opt/sonarqube/data
            - sonarqube_logs:/opt/sonarqube/logs
            - sonarqube_extensions:/opt/sonarqube/extensions
            - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins
        ports:
            - 9000:9000
 
    sonarqube-database:
        container_name: sonarqube-database
        image: postgres:12
        environment:
            - POSTGRES_DB=sonarqube
            - POSTGRES_USER=sonarqube
            - POSTGRES_PASSWORD=sonarpass
        volumes:
            - sonarqube_database:/var/lib/postgresql
            - sonarqube_database_data:/var/lib/postgresql/data
        ports:
            - 5432:5432

volumes:
    sonarqube_database_data:
    sonarqube_bundled-plugins:
    sonarqube_conf:
    sonarqube_data:
    sonarqube_logs:
    sonarqube_database:
    sonarqube_extensions:
```
<br>

### SonarQube Service

- `container_name`: Specifies the name of the Docker container for the SonarQube service.
- `image`: Specifies the Docker image to be used for the SonarQube service (in this case, "sonarqube").
- `depends_on`: Defines the dependency of the SonarQube service on another service (sonarqube-database) to ensure that the
  database is started before SonarQube.
- `environment`: Sets environment variables for the SonarQube application, including JDBC connection data to connect to the PostgreSQL database.
- `volumes`: Connects Docker volumes to specific directories in the SonarQube container to persistently store data.
- `ports`: Makes the SonarQube service accessible on the host through port 9000.

<br>

### SonarQube Database Service

- `container_name`: Specifies the name of the Docker container for the SonarQube database.
- `image`: Specifies the Docker image for the PostgreSQL database.
- `environment`: Sets environment variables for configuring the PostgreSQL database.
- `volumes`: Links Docker volumes to directories in the database container to persistently store data.
- `ports`: Publishes the database service on port 5432 on the host.

<br>

### Volumes

Defines various Docker volumes used for the persistent data of the SonarQube service and the database. 
These volumes allow maintaining data between container instances even when the containers are restarted.

<br>

<strong>Start docker-compose</strong>
```bash
docker-compose up -d
```
<br>

<strong>Stop docker-compose</strong>
```bash
docker-compose down
```
<br>

## Integration of SonarQube into a Spring Boot Gradle project

The integration of SonarQube into a Spring Boot Gradle project is done through the `build.gradle` file. 
Alternatively, this can be done through the `sonar-project.properties` file in the root directory of the project.

<br>

<strong>Integration of the SonarQube plugin</strong>
```groovy
plugins {
    id 'org.sonarqube' version '4.4.1.3373'
}
```
<br>

<strong>Setting the SonarQube parameters</strong>

{{< admonition >}}
This is just an excerpt of possible parameters. Additional parameters can be found in the
[Dokumentation](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/analysis-parameters/).
{{< /admonition >}}

```groovy
sonar {
    properties {
        property 'sonar.host.url', 'http://localhost:9000'
        property "sonar.token", 'Your Project Token'
        property "sonar.projectKey", 'Your Project Key'
        property 'sonar.projectName', 'Your Project Name'
        property 'sonar.sources', 'src/main'
        property 'sonar.java.source', 17
        property 'sonar.gradle.skipCompile', 'true'
        property 'sonar.test.exclusions', 'src/test/**,src/integrationTest/**'
        property 'sonar.exclusions', '**/*Generated.java,src/test,src/integrationTest'
        property 'sonar.coverage.exclusions', '**/*Configuration.java,**/*Application.java'
        property 'sonar.scm.disabled', 'true'
        property 'encoding', 'UTF-8'
        property 'charSet', 'UTF-8'
    }
}
```
<br>

### Explanation of SonarQube parameters

- `sonar.host.url`: The URL of SonarQube to which the analysis results should be sent.
- `sonar.token`: The project key used for authentication and authorization.
Here, the token for your specific project should be provided.
- `sonar.projectKey`: The unique key that identifies the project in SonarQube.
- `sonar.projectName`: The name of the project as displayed in SonarQube.
- `sonar.sources`: The directory containing the source code files for analysis.
- `sonar.java.source`: The Java version used for source code analysis.
- `sonar.gradle.skipCompile`: Whether compilation should be skipped during SonarQube analysis.
- `sonar.test.exclusions`: The directories or file paths to be excluded from test analysis.
- `sonar.exclusions`: The directories or file paths to be excluded from general code analysis.
- `sonar.coverage.exclusions`: File paths to be excluded from code coverage analysis.
- `sonar.scm.disabled`: Whether the integration of Software Configuration Management (SCM) should be disabled during analysis.
- `encoding` und `charSet`: Setting the character encoding for code analysis.

<br>

After the analysis has been performed, the project should be accessible at [http://localhost:9000](http://localhost:9000).
The default login credentials for SonarQube are `admin` (username) and `admin` (password).
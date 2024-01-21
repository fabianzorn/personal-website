---
weight: 4
title: "Implementierung von SonarQube in eine Spring Boot Anwendung."
date: 2024-01-18T21:55:11+02:00
draft: false
description: "SonarQube"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["SonarQube"]
categories: ["SonarQube"]

lightgallery: true
---
Implementierung von SonarQube in eine Spring Boot Anwendung.
<!--more-->

## Was ist SonarQube?

SonarQube ist eine Open-Source-Plattform zur statischen Code-Analyse, die Entwicklern und Teams dabei hilft, 
die Qualität ihres Quellcodes zu überwachen und zu verbessern.
Die Hauptziele von SonarQube sind die Identifizierung von Code-Smells, Bugs und Sicherheitslücken sowie die Überwachung 
der Code-Qualität über die Zeit.

<br>

## Erstellen von docker-compose.yml

<strong>docker-compose.yml</strong>
```yml
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

- `container_name`: Legt den Namen des Docker-Containers für den SonarQube-Dienst fest.
- `image`: Gibt das Docker-Image an, das für den SonarQube-Dienst verwendet werden soll (in diesem Fall "sonarqube").
- `depends_on`: Definiert die Abhängigkeit des SonarQube-Dienstes von einem anderen Dienst (sonarqube-database), 
um sicherzustellen, dass die Datenbank vor SonarQube gestartet wird.
- `environment`: Setzt Umgebungsvariablen für die SonarQube-Anwendung, einschließlich JDBC-Verbindungsdaten zur 
Verbindung mit der PostgreSQL-Datenbank.
- `volumes`: Verbindet Docker-Volumes mit spezifischen Verzeichnissen im SonarQube-Container, um Daten persistent zu speichern.
- `ports`: Macht den SonarQube-Dienst über den Port 9000 auf dem Host erreichbar.

<br>

### SonarQube Database Service

- `container_name`: Legt den Namen des Docker-Containers für die SonarQube-Datenbank fest.
- `image`: Gibt das Docker-Image für die PostgreSQL-Datenbank an.
- `environment`: Setzt Umgebungsvariablen für die Konfiguration der PostgreSQL-Datenbank.
- `volumes`: Verbindet Docker-Volumes mit Verzeichnissen im Datenbank-Container, um Daten persistent zu speichern.
- `ports`: Veröffentlicht den Datenbankdienst über den Port 5432 auf dem Host.

<br>

### Volumes

Definiert verschiedene Docker-Volumes, die für die persistenten Daten des SonarQube-Dienstes und der Datenbank verwendet werden. 
Diese Volumes ermöglichen es, Daten zwischen Containerinstanzen beizubehalten, auch wenn die Container neu gestartet werden.

<br>

<strong>docker-compose ausführen</strong>
```bash
docker-compose up -d
```
<br>

<strong>docker-compose stoppen</strong>
```bash
docker-compose down
```
<br>

## Einbindung von SonarQube in Spring Boot Gradle Projekt

Die Einbindung von SonarQube in einem Spring Boot Gradle Projekt erfolgt über die Datei `build.grade`.
Alternativ ist dies über die Datei `sonar-project.properties` im Root-Verzeichnis des Projekts möglich.

<br>

<strong>Einbindung des SonarQube Plugins</strong>
```groovy
plugins {
    id 'org.sonarqube' version '4.4.1.3373'
}
```
<br>

<strong>Festlegung der SonarQube Parameter</strong>

{{< admonition >}}
Dies ist nur ein Auszug von möglichen Parametern. Weitere Parameter können der
[Dokumentation](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/analysis-parameters/) entnommen werden.
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

### Erläuterung der SonarQube Parameter

- `sonar.host.url`: Die URL von SonarQube, zu der die Analyseergebnisse gesendet werden sollen.
- `sonar.token`: Der Projektschlüssel, der für die Authentifizierung und Autorisierung verwendet wird. 
- Hier sollte der Token für Ihr spezifisches Projekt angegeben werden.
- `sonar.projectKey`: Der eindeutige Schlüssel, der das Projekt in SonarQube identifiziert.
- `sonar.projectName`: Der Name des Projekts, wie er in SonarQube angezeigt wird.
- `sonar.sources`: Das Verzeichnis, das die Quellcode-Dateien für die Analyse enthält.
- `sonar.java.source`: Die Java-Version, die für die Quellcode-Analyse verwendet wird.
- `sonar.gradle.skipCompile`: Ob die Kompilierung bei der SonarQube-Analyse übersprungen werden soll.
- `sonar.test.exclusions`: Die Verzeichnisse oder Dateipfade, die von der Test-Analyse ausgeschlossen werden sollen.
- `sonar.exclusions`: Die Verzeichnisse oder Dateipfade, die von der allgemeinen Code-Analyse ausgeschlossen werden sollen.
- `sonar.coverage.exclusions`: Dateipfade, die von der Code-Coverage-Analyse ausgeschlossen werden sollen.
- `sonar.scm.disabled`: Ob die Einbindung des Software Configuration Management (SCM) während der Analyse deaktiviert werden soll.
- `encoding` und `charSet`: Festlegung der Zeichenkodierung für die Code-Analyse.

<br>

Nachdem die Analyse ausgeführt wurde, sollte das Projekt unter 
[http://localhost:9000](http://localhost:9000) erreichbar sein.
Die default Anmeldedaten für SonarQube sind `admin` (Benutzername) und `admin` (Passwort).
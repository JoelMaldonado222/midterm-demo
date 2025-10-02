# Midterm Demo: IntelliJ → GitHub → Maven/JUnit → Jenkins → Docker

A minimal Spring Boot 3 app to demonstrate a full workflow:
- Code in **IntelliJ**
- Version control in **GitHub**
- Build & test with **Maven + JUnit**
- CI pipeline in **Jenkins** (build, test, Docker build, smoke test)
- Deploy with **Docker**

## Run locally (no Docker)
mvn clean verify
mvn spring-boot:run
# http://localhost:8080/hello

## Build & run in Docker
docker build -t midterm-demo .
docker run -d --name midterm-demo -p 8080:8080 midterm-demo:ci
curl -s http://localhost:8080/hello
docker rm -f midterm-demo

## Jenkins pipeline
- Job: *Pipeline script from SCM* pointing to this repo
- Tools: JDK 17 (`jdk17`), Maven (`maven3`)
- Pipeline stages: checkout → mvn verify → docker build → smoke test (`curl /hello`)

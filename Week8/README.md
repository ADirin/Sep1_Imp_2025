
# Speed API

A minimal Spring Boot REST API that calculates **speed = distance / time**.

## Project Structure
```
speed-api/
├─ pom.xml
├─ Dockerfile
├─ deployment.yaml
├─ src/
│  └─ main/
│     ├─ java/
│     │  └─ com/
│     │     └─ example/
│     │        └─ speedapi/
│     │           ├─ SpeedApiApplication.java
│     │           └─ SpeedController.java
│     └─ resources/
│        └─ application.properties
└─ README.md
```

## Endpoint
- `GET /speed?distance=100&time=2` → `50.0`

## Build & Run (local)
```bash
mvn clean package
mvn spring-boot:run
# or
java -jar target/speed-api-1.0.0.jar
```

## Docker
```bash
mvn clean package
docker build -t speed-api:1.0 .
docker run -p 8080:8080 speed-api:1.0
```

## Minikube
```bash
minikube start
minikube image load speed-api:1.0
kubectl apply -f deployment.yaml
kubectl get pods
kubectl get svc speed-api-service
```

Open:
```
http://$(minikube ip):30080/speed?distance=120&time=3
```

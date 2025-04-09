
# User Microservice

This is a simple Spring Boot microservice for managing users. It includes full build and deploy steps using Maven, Docker, and Linux terminal commands.

---

## Project Structure

```
user-svc/
├── src/
│   └── main/
│       ├── java/com/example/user/
│       │   ├── controller/
│       │   ├── service/
│       │   ├── repository/
│       │   └── model/
│       └── resources/
│           └── application.properties
├── Dockerfile
├── pom.xml
└── deploy.sh
```

---

## How to Create Project Structure (Linux CLI Only)

```bash
mkdir -p user-svc/src/main/java/com/example/user/{{controller,service,repository,model}}
cd user-svc
touch pom.xml Dockerfile
mkdir -p src/main/resources
nano src/main/resources/application.properties
```

Use `nano` or `vim` to add your Java code files to their respective directories.

---

## Dockerfile

Place this file in the root of your project (`user-svc/`).

```Dockerfile
FROM openjdk:17-jdk-alpine
WORKDIR /app
COPY target/user-svc-0.0.1-SNAPSHOT.jar user-svc.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "user-svc.jar"]
```

---

## Shell Script to Build and Deploy

Create a file named `deploy.sh` and paste the following:

```bash
#!/bin/bash

echo ">>> Building the project with Maven..."
mvn clean package

echo ">>> Building Docker image..."
docker build -t user-svc .

echo ">>> Stopping any running container..."
docker stop user-svc 2>/dev/null
docker rm user-svc 2>/dev/null

echo ">>> Running Docker container..."
docker run -d -p 8080:8080 --name user-svc user-svc

echo ">>> Done! App running at http://localhost:8080/users"
```

Make it executable and run it:

```bash
chmod +x deploy.sh
./deploy.sh
```

---

## Accessing the App

Once deployed, open your browser or use curl:

```bash
curl http://localhost:8080/users
```

You’re all set!

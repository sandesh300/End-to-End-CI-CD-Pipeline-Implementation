# Spring Boot Based Java Web Application

This is a simple **Spring Boot** based Java application that can be built using **Maven**.  
Spring Boot dependencies are handled using the `pom.xml` at the root directory of the repository.

This application follows the **MVC architecture** where the controller returns a page with `title` and `message` attributes to the view.

---

## 🚀 Execute the Application Locally and Access via Browser

### 1. Clone the Repository
```bash
git clone https://github.com/sandesh300/End-to-End-CI-CD-Pipeline-Implementation/spring-boot-app
cd java-maven-sonar-argocd-helm-k8s/sprint-boot-app
````

### 2. Build the Maven Artifacts

```bash
mvn clean package
```

The above command stores the artifacts in the `target` directory.
You can run the artifact directly **(requires Java 11)** or run it as a **Docker container**.

> 💡 **Recommendation:** Use Docker to avoid local setup issues with Java versions and dependencies.

---

## ▶️ Running the Application

### Option 1: Execute Locally (Java 11 Required)

```bash
java -jar target/spring-boot-web.jar
```

Access the application at: **[http://localhost:8080](http://localhost:8080)**

---

### Option 2: Run with Docker

1. **Build the Docker Image**

   ```bash
   docker build -t ultimate-cicd-pipeline:v1 .
   ```

2. **Run the Container**

   ```bash
   docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
   ```

Access the application at:
**http\://<ip-address>:8010**

---

## 🔧 Next Steps – Configure SonarQube Server Locally

### System Requirements

* **Java 17+** (Oracle JDK, OpenJDK, or AdoptOpenJDK)
* **Hardware Recommendations:**

  * Minimum **2 GB RAM**
  * **2 CPU cores**

### Installation Steps

```bash
sudo apt update && sudo apt install unzip -y
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
unzip *
chown -R sonarqube:sonarqube /opt/sonarqube
chmod -R 775 /opt/sonarqube
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start
```

Access SonarQube at:
**http\://<ip-address>:9000**

---

🎉 **Hurray!** Your Spring Boot app and SonarQube setup are ready!
```

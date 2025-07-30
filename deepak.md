
# POC Of Salary API

## Author Information

| Created by      | Created on         | Version          | Last updated On   | pre Reviewer       | L0 Reviewer     | L1 Reviewer          |    L2 Reviewer    |
|-----------------|--------------------|------------------|-------------------|--------------------|-----------------|----------------------|-------------------|
| Deepak Kushwaha  |  28-07-2025        | V 1.0            |        |  Anjali          |  -      |     -   |   - |

# Table of Contents

1. [Pre-Requisites](#pre-requisites)
2. [System Requirements](#system-requirements)
3. [Architecture](#architecture)
4. [Ports](#ports)
5. [API Setup and Execution](#api-setup-and-execution)
   - [Update System Packages](#update-system-packages)
   - [Installing Dependencies for Salary API](#installing-dependencies-for-salary-api)
   - [Working with the Salary API Git Repository](#working-with-the-salary-api-git-repository)
   - [Starting the Application](#starting-the-application)
   - [Access the API](#access-the-api)
6. [Conclusion](#conclusion)
7. [Contact Information](#contact-information)
8. [References](#references)


---

# Pre-Requisites
The Salary API application have some database, cache manager and package dependencies. Some of the dependencies are optional and some are mandatory. To compile the application, we only need maven as build tool, but for running the application following things are required:-



| Tool/Resource                          | Description                                                                                              |
|----------------------------------------|----------------------------------------------------------------------------------------------------------|
| [ScyllaDB](https://www.scylladb.com/)  | A high-performance NoSQL database designed for low-latency and high throughput, ideal for big data applications. |
| [Redis](https://redis.io/)             | An in-memory data structure store used as a database, cache, and message broker, known for its speed and flexibility. |
| [Migrate](https://github.com/golang-migrate/migrate) | A database migration tool for managing schema changes, supporting many different databases.            |
| [Maven](https://maven.apache.org/)     | A build automation tool primarily used for Java projects to manage dependencies and build lifecycle.     |
| [Java](https://www.oracle.com/java/)   | A versatile, object-oriented programming language widely used for building robust, scalable applications. |


---

# System Requirements

| Requirement   | Details                      |
|---------------|------------------------------|
| OS            | Ubuntu or other Linux-based OS |
| RAM           | 4 GB minimum                |
| Disk Space    | 20 GB                       |
| Processor     | Dual-core recommended       |
| Instance Type | t2.medium                   |

---

# Architecture

![logo](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiocVDwKCZV033uIzVooPBRi24a0V4YCkJOdgywwlei8P0alQzOxNAWWeNMG_fyZZK2QXf6CjH7cPnByUrpQUI5aP88Y1vBsjzp7KR_v0lwEnxLa9mI74ZRN_3UUznTguDiFBHDDdflbjTeq8jQvFdi_qRcu0uBnBjamZMg9fmmyi8_rCWVud23NJO1ncY/s16000/daigram.drawio%20(2).png)



---

# Ports

| *Port* | *Protocol/Service*       | *Description*                                                                 |
|----------|----------------------------|---------------------------------------------------------------------------------|
| 22       | SSH                        | Used for secure shell access to the server.                                    |
| 8080     | HTTP (Swagger UI)          | Used for accessing Swagger UI for API documentation.                          |
| 9042     | ScyllaDB                   | The default port for connecting to ScyllaDB (Cassandra-compatible database).   |
| 6379     | Redis                      | The default port for connecting to the Redis in-memory data store.            |
| 80       | HTTP                       | Used for standard web traffic and serving HTTP requests.                      |
| 443      | HTTPS                      | Used for secure web traffic and serving HTTPS requests.                       |


---

# API Setup and Execution

## Update System Packages

> *Follow STEP 3*: [Update System Packages]().





## Installing Dependencies for Salary API
| *Tool*      | *Installation Steps*                                                                                              |
|---------------|--------------------------------------------------------------------------------------------------------------------|
| *ScyllaDB*  | Follow this [link]() to install and configure ScyllaDB |
| *Redis*     | Follow this [link]() to install and configure Redis     |
| *Java 17*   | Follow this [link]() to install Java                                                                        |
| *Maven*     | Follow this [link]() to install Maven                                                                                       |
| *jq*        | Follow this [link]() to install Jq                                                                                          |
| *make*      | Follow this [link]() to install Make                                                                                        |

### golang-migrate
*1. Download and extract:*
   bash
   curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz
   
<img width="1170" height="292" alt="image" src="https://github.com/user-attachments/assets/a89e10d4-b782-44fd-8849-1652a5a03b80" />

*2. Move the binary:*
   bash
   sudo mv migrate /usr/local/bin/migrate
   
<img width="1112" height="183" alt="image" src="https://github.com/user-attachments/assets/4e724009-3ecc-46ac-a392-4973a11ac6bb" />


## Working with the Salary API Git Repository
*1. Clone the repository:*
   bash
   git clone https://github.com/OT-MICROSERVICES/salary-api.git
   cd salary-api
   
<img width="1068" height="246" alt="image" src="https://github.com/user-attachments/assets/b48068af-aa2b-45cd-bd47-130bbe323413" />


---


*2. Configure migration:*
   bash
   vi migration.json
   
   - Replace the IP address with your instance  private IP.

<img width="1006" height="246" alt="image" src="https://github.com/user-attachments/assets/f9acf36e-51cd-49f1-b0b0-0aff200cae58" />



*3. Update application configuration:*
   bash
   vi src/main/resources/application.yml
   
   - Replace the IP address with your instance private IP.

<img width="930" height="292" alt="image" src="https://github.com/user-attachments/assets/cd724b96-c9d7-4b07-b1c3-c1b801e37599" />


*4. Update test configuration:*
   bash
   vi src/test/resources/application.yml
   
   - Replace the IP address with your instance private IP.

<img width="930" height="292" alt="image" src="https://github.com/user-attachments/assets/cd724b96-c9d7-4b07-b1c3-c1b801e37599" />


*5. Update Java API configuration:*
   bash
   vi src/main/java/com/opstree/microservice/salary/config/OpenAPIConfig.java
   
*Changes to make in this file*

- Add the following inside (import java.util.List;) for configuring and enabling Cross-Origin Resource Sharing (CORS) .
bash
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;




- Replace the Localhost with your *public* IP (in devserver.seturl in myOpenAPI() @bean).
  
- add the following CorsFilter bean after the existing myOpenAPI() bean definition inside the OpenAPIConfig class.
  
     java
     @Bean
     public CorsFilter corsFilter() {
       CorsConfiguration config = new CorsConfiguration();
       config.addAllowedOrigin("*");
       config.addAllowedMethod("*");
       config.addAllowedHeader("*");
       UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
       source.registerCorsConfiguration("/**", config);
       return new CorsFilter(source);
     }
     
<img width="1162" height="439" alt="image" src="https://github.com/user-attachments/assets/19eccb96-44fe-4d90-a30a-ac8df9db258c" />

*6. Update test configurations:*
   bash
   sudo vi src/test/java/com/opstree/microservice/salary/config/OpenAPIConfigTests.java
   
   - Replace the IP address with your *public* IP (in assertEquals).

<img width="984" height="370" alt="image" src="https://github.com/user-attachments/assets/dff9e1a6-5931-4103-9732-d63d5f44377b" />

## Starting the Application

*1. Run migrations:*
   bash
   make run-migrations
   

*2. Build the application:*
   bash
   make build
   
<img width="1169" height="439" alt="image" src="https://github.com/user-attachments/assets/9e525837-cc04-4103-a129-a253a8115fa6" />

<img width="1179" height="323" alt="image" src="https://github.com/user-attachments/assets/5145f2a8-5bb4-4534-951d-ddf27c93b833" />



*3. Create a service file:*

A service file manages the salary API with systemd, enabling automatic startup, easy control via systemctl, and recovery from failures, ensuring reliable and seamless operation.

bash
sudo nano /etc/systemd/system/salary-api.service
   


   bash
   [Unit]
Description=Salary API Service
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/salary-api
ExecStart=/usr/bin/java -jar /home/ubuntu/salary-api/target/salary-0.1.0-RELEASE.jar
SuccessExitStatus=143
Restart=on-failure
Environment=JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
Environment=SCYLLA_HOST=<private-ip>
Environment=REDIS_HOST=<private-ip>

[Install]
WantedBy=multi-user.target

   
*5. Service File Commands*

Below are the commands to manage the salary-api service using systemd. These commands help reload systemd configuration, enable the service to start on boot, start the service manually, and check its current status.

| Command                                  | Description                                                              |
|------------------------------------------|--------------------------------------------------------------------------|
| sudo systemctl daemon-reload           | Reloads systemd manager configuration to recognize changes in service files. |
| sudo systemctl enable salary-api.service | Enables the service to start automatically on system boot.              |
| sudo systemctl start salary-api.service | Starts the salary-api service immediately.                            |
| sudo systemctl status salary-api.service | Displays the current status of the service, including logs and errors.  |


## Access the API
Visit the following URL in your browser:

http://<public-ip>:8080/salary-documentation

<img width="917" height="387" alt="image" src="https://github.com/user-attachments/assets/2f71dfb8-ab21-492d-ab5c-611b2d3d59cb" />
<img width="917" height="387" alt="image" src="https://github.com/user-attachments/assets/636ffeff-9af9-4af4-9c0a-815518cd6494" />
<img width="1255" height="628" alt="image" src="https://github.com/user-attachments/assets/58b22ec8-9f81-493c-9fd8-358573c9d70e" />


---

# Conclusion
The Salary API, a key component of the OT-Microservices system, manages salary transactions efficiently. It leverages ScyllaDB for scalable storage, Redis for caching, Migrate for database versioning, Swagger for documentation, and Maven for builds. Designed for high performance and seamless integration, it ensures fast data access, scalability, and enterprise-grade reliability.


## Contact Information
| **Name**           | **Email address**                         |
|--------------------|--------------------------------------------|
| Deepak Kushwaha    | [deepak.kushwaha.snaatak@mygurukulam.co](mailto:deepak.kushwaha.snaatak@mygurukulam.co) |

---

## References
| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| ScyllaDB Introduction documentation    | [Click here](https://github.com/Snaatak-Cloudops-Crew/documentation/tree/SCRUM-78-deepak/OT-Microservices/Softwares/Scylladb/Introduction) |
| ScyllaDB Installation documentation    | [Click here](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-77-abhishek-saini/OT-Microservices/Softwares/Scylladb/POC/README.md) |
| Redis Installation Guide              | [Click here](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-73-aryan-mishra/OT-Microservices/Softwares/Redis/POC/README.md) |
| Maven                   | [Click here](https://maven.apache.org/what-is-maven.html)                    

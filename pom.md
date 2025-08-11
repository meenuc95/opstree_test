# pom.xml Step-by-Step Installation(SOP)

## Author Information

| Author          | Created on | Version   | Last updated by | Internal Reviewer |
|-----------------|------------|-----------|------------------|--------------------|
| Meenu Chauhan       | 22-07-25   | version 1 | 22-07-25              | Siddarth          | 

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Install Java JDK](#install-java-jdk)
- [Install Maven](#install-maven)
- [Clone the Project](#clone-the-project)
- [Review and Edit pom.xml](#review-and-edit-pomxml-if-needed)
- [Build the Project](#build-the-project)
- [Run the Application](#run-the-application)
- [Run Tests](#run-tests)
- [Clean the Project](#clean-the-project)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)


## Introduction

pom.xml is the Project Object Model file used by Apache Maven, a popular build automation and dependency management tool for Java projects.
It defines project dependencies, plugins, build profiles, and other configuration.

---

## Prerequisites

| Requirement | Description |
|-------------|-------------|
| Java        | Java 8 or higher installed  |
| Maven       | Downloaded and installed Apache Maven  |

## Install Java JDK

**On Ubuntu:**
```bash
sudo apt update
sudo apt install -y openjdk-11-jdk
```
**Verify installation:**
```bash
java -version
```

---

## Install Maven

**On Ubuntu:**
```bash
sudo apt install -y maven
```
**Verify installation:**
```bash
mvn -version
```

---


## Clone the Project

```bash
git clone <your-repo-url>
cd <your-project-directory>
```
*Replace `<your-repo-url>` and `<your-project-directory>` with your actual values.*

---

## Review and Edit `pom.xml` (if needed)

- Open `pom.xml` in a text editor.
- Check for required Java version, dependencies, and plugins.
- Update any configuration as needed for your environment.

---

## Build the Project

```bash
mvn clean install
```
This command performs the following actions:
- **Cleans** previously compiled files (`target/` directory).
- **Downloads** all required dependencies.
- **Compiles** the project source code.
- **Runs** all unit tests.
- **Packages** the application into a deployable format (e.g., `.jar` or `.war`).
- **Installs** the built artifact into your local Maven repository (`~/.m2/repository`), making it available for use by other local projects.
---
## Run the Application

After building the project, run the generated JAR file using the following command:

```bash
java -jar target/<your-app>.jar
```
---

## Clean the Project

```bash
mvn clean
```
This command removes the `target/` directory and all previously compiled files, ensuring that the next build starts from a clean state.

---

## Run Tests

```sh
mvn test
```

This command compiles the test source code and runs all unit tests defined in the project. Test results will be displayed in the console. 


## Troubleshooting

- **Dependency download issues:** Check your internet connection and proxy settings.
- **Build errors:** Review the error messages; check for missing dependencies or Java version mismatches.
- **Port conflicts:** If running a web server, ensure the required port is free.

---

## Best Practices

- **Keep your POM files clean and well-organized**: Maintain a clear structure with comments and consistent formatting.
- **Use `<properties>`**: Define and manage common values like Java versions, dependency versions, etc., in the `<properties>` block for reusability.
- **Specify explicit versions**: Always mention specific versions for dependencies and plugins to avoid unexpected changes.
- **Leverage `<dependencyManagement>`**: In multi-module projects, manage versions centrally using `<dependencyManagement>` in the parent POM.
- **Keep dependencies up to date**: Regularly review and update your dependencies to benefit from security patches and improvements.

---


## Contact Information

| Name            | Email address |
|-----------------|---------------|
| Meenu Chauhan       | meenu.chauhan.snaatak@mygurukulam.co

## References
| **Link**                                                                 | **Description**                                   |
|--------------------------------------------------------------------------|---------------------------------------------------|
| [Medium Blog](https://medium.com/@gaganjain9319/what-is-maven-and-the-importance-of-pom-xml-8273f5cd6fd6) | Document for maven and pom.xml introduction        |
| [Pom.xml Document](https://anju-chaurasiya2012.medium.com/understanding-pom-xml-in-maven-a-beginner-friendly-guide-7f2e23232d4a) | Document for pom.xml guide.          |

---

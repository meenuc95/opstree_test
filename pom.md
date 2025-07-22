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
```sh
sudo apt update
sudo apt install -y openjdk-11-jdk
```
**Verify installation:**
```sh
java -version
```

---

## Install Maven

**On Ubuntu:**
```sh
sudo apt install -y maven
```
**Verify installation:**
```sh
mvn -version
```

---


## Clone the Project

```sh
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

```sh
mvn clean install
```
- Downloads dependencies, compiles code, runs tests, and packages the application (e.g., as a `.jar` or `.war` file).

---
## Run the Application

After building the project, run the generated JAR file using the following command:

```sh
java -jar target/<your-app>.jar
```
---

## Run Tests

```sh
mvn test
```

---

## Clean the Project

```sh
mvn clean
```
- Removes the `target/` directory and all compiled files.

---

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
| [Redhat Official Document](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/kernel_administration_guide/working_with_sysctl_and_kernel_tunables) | Document for modify kernel tunables.          |
| [Phoenixnap Document](https://phoenixnap.com/kb/sysctl) | Document for sysctl command in Linux.          |

---

# JQ Documentation

## Author Information

| Author          | Created on | Version   | Last updated by | Internal Reviewer |
|-----------------|------------|-----------|------------------|--------------------|
| Meenu Chauhan       | 21-07-25   | version 1 | 21-07-25              | Siddarth          | 

## Table of Contents
- [Introduction](#introduction)
- [Why Use JQ](#why-use-jq)
- [Features of JQ](#features-of-jq)
- [Common JQ Commands Examples](#common-jq-commands-examples)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)


## Introduction
  JQ is a lightweight and powerful command-line tool designed specifically for working with JSON data. It allows users
  to parse, filter, extract, and manipulate JSON files effortlessly, making it an essential utility for developers, system administrators. 
  Using jq can aid you when you need to manipulate data. For example, if you run a curl call to a JSON API, jq can extract specific information 
  from the server’s response.


---
## Why Use JQ

JSON is everywhere: Modern APIs, configuration files, and logs often use JSON.
Manual parsing is hard: Extracting or manipulating JSON with traditional tools (like grep, awk, or sed) is error-prone and cumbersome.
Automation: JQ enables powerful, scriptable JSON manipulation for automation, data analysis, and DevOps workflows.
Powerful Filtering:	JQ allows us to extract specific fields or values from large JSON datasets with simple and expressive syntax.
Cross-Platform Support: JQ is designed to work consistently across all major operating systems, making it a highly portable and dependable tool in diverse environments

---

## Features of JQ

| Feature                                 | Description                                                                                             |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Lightweight & Fast**               | `jq` is a fast, low-footprint command-line utility written in C, ideal for parsing JSON efficiently.    |
| **Pure JSON Support**                | Works directly on JSON data – no need to convert formats or use additional tools.                       |
| **Powerful Query Language**          | Supports filters, expressions, conditionals, and functions to extract and transform JSON data.          |
| **Formatting & Pretty-Print**        | Makes raw JSON human-readable with colorized, indented output using: <br> `jq . file.json`              |
| **Programmatic Transformation**      | Convert, reshape, or modify JSON with map, reduce, and custom expressions.                              |
| **Supports Streaming**               | With the `--stream` flag, it can parse large JSON files without loading the entire content into memory. |
| **Cross-Platform**                   | Available on Linux, macOS, and Windows (via WSL or native binaries).                                    |
| **Filtering & Extraction**           | Easily extract specific fields or arrays from JSON: <br> `jq '.user.name' file.json`                    |

---
## Common JQ Commands Examples

### 1. Viewing and Formatting JSON

#### Pretty-print JSON  
To display JSON in a readable, indented format:

```bash
echo '{"name":"John","age":30}' | jq '.'
 ```


#### Compact Output
To print JSON in a single line without extra whitespace:

```bash

echo '{"name":"John","age":30}' | jq -c '.'
```

#### Colored Output
To display colorized output in the terminal (if supported):

```bash
echo '{"name":"John","age":30}' | jq -C '.'
```
This helps improve readability in terminal environments with color support.

### 2. Accessing Data in JSON

#### To extract the value of a specific key:

``` bash

echo '{"name":"John","age":30}' | jq '.name'
```
#### To access nested JSON fields:

```bash

echo '{"user": {"name": "Jane", "city": "London"}}' | jq '.user.city'
```

#### To retrieve a value from an array using its index (0-based):

```bash

echo '["apple", "banana", "cherry"]' | jq '.[1]'

```

## Conclusion
JQ is an essential tool for anyone working with JSON on the command line. Its expressive language, powerful features, and ease of use make it ideal for data extraction, transformation, and automation tasks.

## Contact Information

| Name            | Email address |
|-----------------|---------------|
| Meenu Chauhan       | meenu.chauhan.snaatak@mygurukulam.co

## References
| **Link**                                                                 | **Description**                                   |
|--------------------------------------------------------------------------|---------------------------------------------------|
| [JQ Documentation](https://medium.com/@learntheshell/guide-to-jq-command-d75176fc4303) | Introduction to jq.          |
| [JQ Commands](https://www.baeldung.com/linux/jq-command-json) | JQ command example.          |





---

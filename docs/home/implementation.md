# Impementation

## Technology Stack

**Programming Language:** [Java 21](https://www.oracle.com/java/technologies/downloads/#java21)

 Why java? Because it is platform-independent as much as possible. When a computer has the Java Runtime Environment (JRE) installed, a Java program can run on it. Most types of computers are compatible with a JRE including PCs running on Windows, Macintosh computers, Unix or Linux computers, and large mainframe computers.

**Programming Framework:** [Spring-Boot 3.3.0](https://spring.io/projects/spring-boot)

Why Spring? Because it brings together years of wisdom in the form of design patterns. Spring has a long history of innovation, adoption, and standardization. Over the years, it's become mature enough to become a default solution for most common problems faced in the development of large scale enterprise applications.

**Building Tool:** [Maven 3.5+](http://maven.apache.org/download.cgi)

Why Maven? Because it is one of the most popular build tools in Java, designed to take much of the hard work out of the build process. Maven uses a declarative approach, where the project structure and contents are described, rather than the task-based approach used in Ant or in traditional make files, for example.

**Database Management System:** [MySQL 5.7+](https://www.mysql.com/)

Why MySQL? Beacuse it was developed for speed, and maintains a reputation for being fast, even if this may come at the expense of some additional features. It is also known for its reliability, backed by a large community of programmers that have put the code through tough testing over the years. 

## Hardware Requirements

The hardware requirements **really depend on the expected workload**. Faster CPU / more cores / more RAM is likely to lead to improved performance.

### Database

**Minimum:** 1 CPU Core, 600 MB RAM, 1 GB Disk space <br />
**Recommended:** 2 CPU Cores, 4 GB RAM, 2+ GB Disk space (depending on the expected amount of data to be stored)

### Java Runtime Environment (JRE)

**Disk space:** ~150 MB

### Core system

**Minimum (per system):** 1 CPU Core, 128 MB RAM + ~100 MB RAM for the JVM, 100 MB Disk space <br />
**Recommended (per system):** 4 CPU Cores, 2 GB RAM, 100 MB Disk space

> _**Hint:**_ [customize the memory usage](../help/faq.md#java-memory-usage)

> _**Note:**_ Please take into consideration, that the above mentioned requirements are reflecting the native deployment. If you use any additional containerization (like Docker, Kubernetes, etc..) or any virtualization, then more resources will be needed for the same performance. 
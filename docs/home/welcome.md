**This site** provides the specifications, architectural descriptions, and practical examples required to work with the **java-spring implementation** of the Eclipse Arrowhead Framework. It is designed as a central resource for developers, integrators, and system architects building Arrowhead compliant solutions.

## What Is Eclipse Arrowhead?

The **Eclipse Arrowhead Framework** is an open-source, service-oriented framework for automation and digitalization. It was built on the principles of **Service-Oriented Architecture (SOA)** to provide a unified methodology that enables interoperability across heterogeneous devices, systems, and industrial domains. Arrowhead abstracts information exchange into loosely coupled services, allowing systems — regardless of vendor, technology, or scale — to interact through standardized, secure, and runtime-orchestrated service interfaces.

<iframe width="756" height="415" src="https://www.youtube.com/embed/b9ROE9pSgE4?si=XNHL3c977ozXGOBH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## The Local Cloud Concept

The fundamental governance unit in Eclipse Arrowhead is the **Local Cloud**:
a closed, local industrial network designed for deterministic, secure, and scalable system-of-systems collaboration.

Each Local Cloud is recommended to host a set of core systems providing the minimum functionality needed for secure, interoperable service exchange. These include:

- **ServiceRegistry** – Registers and discovers services, systems and optionally devices.
- **ServiceOrchestration** – Selects suitable service providers based on the chosen orchestration strategy.
- **ConsumerAuthorization** – Enforces access-control policies for service consumption.
- **Authentication** – Ensures trusted identity management within the Local Cloud.

**Supporting Systems**

Beyond the recommended set, Arrowhead includes a suite of supporting core systems enabling engineering workflows, configuration management, monitoring, operational control, and maintenance functions across system-of-systems deployments.

## Learn More

:octicons-link-16: [arrowhead technology](https://arrowhead.eu/technology/)<br />
:octicons-link-16: [eclipse governance](https://projects.eclipse.org/projects/iot.arrowhead)
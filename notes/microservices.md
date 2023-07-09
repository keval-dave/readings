Monolitihic to Microservice

- what is monolithic?
    - sedimented layers of features and redundant logic translated into thousands of lines of code, written in a single, not so modern programming language, based on outdated software architecture patterns and principles.
- Why monolithic is not a good option
    - making development more challenging - loading, compiling, and building times increase with every new update
    - expensive - has to satisfy its compute, memory, storage, and networking requirements
    - the scaling of individual features of the monolith is almost impossible
        - one way to tackle this by scaling whole application. This can be acheived by creating new instances & load balancing app
    - During upgrade or migrations downtime is inevitable

- what is microservice?
    - Microservices can be deployed individually on separate servers provisioned with fewer resources - only what is required by each service and the host system itself, helping to lower compute resource expenses.
    - Seamless upgrades and patching processes are other benefits of microservices architecture. There is virtually no downtime and no service disruption to clients because upgrades are rolled out seamlessly - one service at a time, rather than having to re-compile, re-build and re-start an entire monolithic application. As a result, businesses are able to develop and roll-out new features and updates a lot faster, in an agile approach, having separate teams focusing on separate features, thus being more productive and cost-effective.



Container Orchestration [https://www.capitalone.com/tech/cloud/what-is-container-orchestration/]

- What are containers?
    - Containers are an application-centric method to deliver high-performing, scalable applications on any infrastructure of your choice. Containers are best suited to deliver microservices by providing portable, isolated virtual environments for applications to run without interference from other running applications.
    - Containers encapsulate microservices and their dependencies but do not run them directly. Containers run container images.
    - A container image bundles the application along with its runtime, libraries, and dependencies, and it represents the source of a container deployed to offer an isolated executable environment for the application. Containers can be deployed from a specific image on many platforms, such as workstations, Virtual Machines, public cloud, etc.

- What Is Container Orchestration?
    - In Development (Dev) environments, running containers on a single host for development and testing of applications may be a suitable option. However, when migrating to Quality Assurance (QA) and Production (Prod) environments, that is no longer a viable option because the applications and services need to meet specific requirements:
        - Fault-tolerance
        - On-demand scalability
        - Optimal resource usage
        - Auto-discovery to automatically discover and communicate with each other
        - Accessibility from the outside world
        - Seamless updates/rollbacks without any downtime.
    - Container orchestrators are tools which group systems together to form clusters where containers' deployment and management is automated at scale while meeting the requirements mentioned above

- Why user container orchestration?
    - Instead of focusing how it focuses on what
    - Declarative way


Introduction to Kubernetes

- what is kubernetes?
    - Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.
    - Greek word - which means helmsman or ship pilot. With this analogy in mind, we can think of Kubernetes as the pilot on a ship of containers.
    - Kubernetes is highly inspired by the Google Borg system, a container and workload orchestrator for its global operations for more than a decade. It is an o pen source project written in the Go language and licensed under the Apache License, Version 2.0.
    - Kubernetes was started by Google and, with its v1.0 release in July 2015

- Form Borg To Kubernetes
    - Borg - Google's Borg system is a cluster manager that runs hundreds of thousands of jobs, from many thousands of different applications, across a number of clusters each with up to tens of thousands of machines
    - Features that are inspired from google borg
        - API servers
        - Pods
        - IP-per-pod
        - Services
        - Labels

- Kubernetes feats.
    - Automatic bin packing
	


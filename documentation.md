---
layout: page
title: Documentation
subtitle: ElastXY all in one documentation page
bigimg: /img/documentation.jpg
---

## Table of contents

- [Introduction](#introduction)
- [What ElastXY can do for you?](#what-elastxy-can-do-for-you)
- [Features list](#elastxy-features)
- [Roadmap](#roadmap)
- [Technical documentation](#technical-documentation)
- [About the project](#about-the-project)



## Introduction

ElastXY is an open, scalable, configurable Java **Distributed Genetic Algorithm Framework** for solving optimization and research problems, powered by **Apache Spark**.

Problems are addressed by creating an `Application`, suggesting a `Target` goal and asking ElastXY heuristic to find a suitable `Solution` via simple REST APIs.  
Problem space can be defined by modeling a `Genoma` via raw or metadata-based model. Optionally, a `DataProvider` can be implemented to ingest any local or distributed datasets.

Why **elastic**? ElastXY allows to specify a **scalable amount of computing resources** to be used, enabling a wide runtime contexts spectrum, that goes from running locally on a single core to servlerless cloud computing on indefinite number of cores and CPUs,  
thanks to:
- two genetics algorithm engine strategies: `SingleColony` and a `MultiColony`.
- a number of deployment techniques for delivering: 1) locally as a Java **SpringBoot application**, 2) on a datacenter via **Docker** containers managed by **Rancher**, or 3) via cloud serverless services like **Amazon EMR** or **Google Dataproc** (experimental).
 
ElastXY is entirely written in **Java 8 language** and adopts **OAuth 2.0 security** standard flows.

*DISCLAIMER*: the project is at an early stage but growing and consolidating fast, so contributions and ideas are very welcome here! :)

> Curious why the butterfly? Discover the [note](#about-the-project) at the end...


## What ElastXY can do for you

This documentation and web site can be read the following ways, based on your needs:
- need a [**Quick Start**](/tutorials#quick-start)? Look at the 10' [Single Colony Tutorial](/tutorials#single-colony-tutorial) for local computing use cases, or to 20' [Multi Colony Tutorial](/tutorials#multi-colony-tutorial) showing distributed computing capabilities of ElastXY.
- curious about ElastXY [**Features**](#elastxy-features)? Here below the [Project goal](#project-goal), a current [Features list](#features-list) and [Coming soon](#coming-soon) capabilities on Roadmap at the time.
- interested on [**Framework design**](#how-it-works) behind the scenes? Maybe technical documentation section through [Framework Components](#framework-components) of [Framework Runtime Contexts](#framework-runtime-contexts) sections will satisfy your curiosity.
- want to know how to contribute or help running this project? Please look at these sections: Open an issue, Contributing, Code of conduit, License on [**GitHub project site**](https://www.github.com/elastxy/elastxy-framework).

And finally, author is working to other materials, so please stay tuned on [Twitter](http://www.twitter.com/elastxy) or on this Blog:
- if you are looking for a **demo** of ElastXY capabilities: soon a **Showcase Applications** section will appear with interesting use cases applied.
- a brand new **Showcase Site** will appear, whose name is a... surprise! :)
- want to access to ElastXY **APIs and specifications**? Author is working to deliver in a short time a decently organized API documentation and extended tutorial.


## ElastXY Features

### Project goal

ElastXY aims to solve any optimization and research problems as an open, scalable, configurable **Distributed Genetic Algorithm Framework**.

Near Framework project, some **Applications** are being provided to cover most interesting use cases.

### Features list

Basic features:
* app development through simple framework interface implementations
* super-simple built-in dependency injection for configuring apps
* a basic set of genetic operators (selection, mutation, crossover) with default implementation, but fully configurable and customizable
* elitism policy: proportion of elite with respect to population
* customizable genoma provider with straightforward domain (Gene, Allele, Chromosome, Strand)
* metadata based Genes user definition
* customizable solution renderer
* environment observer to track execution

Distributed features:
* built-in implementations for ProcessingOnly and DataDriven scenarios through Apache Spark
* basic clustering via Spark Standalone Cluster
* `DistributedDatasetProvider` interface for implementing custom datasource
* results collected via shared file system or thanks to Apache Kafka async exactly-once delivery policy

REST APIs for:
* requesting a fully parametrized algorithm execution
* executing a fixed benchmark for checking fine parameters tuning
* testing algorithm performance comparing to a "total random" execution
* do some analytics, by requesting a (potentially big) number of executions
* some monitoring and health-checks

Development support features:
* application templates via Maven archetypes
* TODO: example applications to showcase common use cases
* APIs documentation via [Swagger UI](https://swagger.io/swagger-ui/) and [Postman](https://www.getpostman.com/) collections

## Roadmap

### Coming soon

* complete implementation of common genetic operators
* internationalization
* non-blocking async APIs
* migration of Spark RDD to Spark Dataframes representation

Following optional **SMACK stack** components will be enabled to:
* enable robust cluster management through **Apache Mesos**
* ingest big quantity of data via **Apache Cassandra** distributed database connector
* exchange async messaging for tracking execution and sharing results thanks to **Apache Kafka**
* plug-in another library as the algorithm Engine, via excellent **Jenetics Library**

[Note: it can be noticed that these technologies form the **SMACK Framework** without A(kka), but don't worry: Actor model with a Play frontend showcase application is on the roadmap... ;) ]

### Future developments
- seamless Scala integration or partial/complete porting to Scala language for simplifying and ease coding
- dynamic behaviour via scripting (Groovy, Javascript...)
- PaaS for offering ElastXY services and dynamic registering of apps, without infrastructure burdening
- developer portal for easily integrate with ElastXY
- cover other OAuth 2.0 / OpenID Connect security flows and security concerns, like token signature and verification

and many other items.


## Technical documentation

### How it works

Given a problem, ElastXY allows users to define their own specific goal as a `Target`, choose or implement `Application` components to achieve it and their preferred execution environment (local, on premise, cloud) to horizontally scale, if needed.

As any other Genetic Algorithm library or framework, ElastXY can elaborate generations of Solutions evolving over time trying to optimize a Fitness function.

Algorithm then works until reaches an end condition, based on configurable runtime parameters set by user, either:
- the optimal Solution is found (maximum Fitness)
- a sub-optimal Solution is found
- another end condition is found, such as a fitness threshold, a maximum execution time or number of generations, a minimum/maximum/exact fitness values, etc.

At runtime, user is allowed to call simple APIs to run his own `Application` with specific execution parameters, monitoring `Experiment` execution, and finally receiving feedback as an optimal or sub-optimal `Solution`, with `ExperimentStats` to show results.

Anything having a representation can be a `Target` or a `Solution`, and almost all `Application` components can be extended and reimplemented (even the default Genetic Engine components).

Finally, a `SingleColony` and a `MultiColony` runtime contexts are provided to seamlessly run your project locally or distributed.

To do this, ElastXY natively adopts **Apache Spark** as a distributed computing framework, wrapped behind a simple API interface which can be easily integrated on client ecosystems.



### Framework Components

#### The `Application`

As explained, the core concept is the `Application`. Here happens all the magics.

But, fortunately, all `Application` components can be configured via some Java coding and a simple JSON metadata file, to be injected by an internal lightweight injection module at application Bootstrap phase (no Spring/Guice/whatever dependencies here, only simple raw Java code).

All framework components are interface and default implementations *open to extension* by design, so behaviour can be completely redefined.

But generally they prefer conventions over configurations and at least one default implementation is provided. So, ehy, don't be scared to play with them! :)

#### Main framework abstractions
To be more specific, an ElastXY `Application` is written by the User, who defines a `Target` and all components needed for it to be achieved.

Most of the 

Engine components:
- the crucial `FitnessCalculator` function to establish who's best between `Solution` of a given `Population`
- `Genotype`, optionally defined via `Genes` metadata, as the genetic material algorithm works on
- `Incubator` grow function to raise it to a `Phenotype` representation, against which fitness is calculated, exactly as giraffe neck influences her chance to survival, not the DNA strand 
- genetic operators like `Selector`, `Recombinator` and `Mutator`

Infrastructural components:
- `DatasetProvider` to ingest input (big)data
- `EnvObserver` to receive and eventually redistributed execution events of interest

Rendering/reporting components:
- `SolutionRenderer` to "see" `Solution` in a friendly way, if needed
- `ResultsRenderer` to export resulting `Solution` and `ExperimentStats` in a readable format

Advanced Engine components:
- `AlleleGenerator` to define how to build `Gene` values composing `Solutions` (usually the `MetadataAlleleGenerator` should be enough)
- factory methods for creating execution context, such as: `PopulationFactory`, `EnvFactory`, `GenomaProvider`

Distributed context components:

Many of the above have a counterpart for distributed computing of Spark Dataset, Cassandra tables, and so on.


### Framework Runtime Contexts

SingleColony and MultiColony modes support following typical runtime contexts:
- *local*, as a **standalone Spring Boot** application or as an **Apache Spark cluster** run locally.
- *distributed*, on multi-server environments via **Docker containers** managed by **Rancher Console**.
- (to be verified soon) *in cloud*, by any serverless computing services supporting Apache Spark, like **Google Dataproc** or **Amazon EMR**.

#### Single Colony Execution [local]
In a `SingleColonyExperiment`, ElastXY tries to solve the problem by evolving generations of candidate `Solutions` over a single `Era`, until `EndCondition` is found.

`EndCondition` includes:
* maximum, minimum or exact value of a `Fitness` function typically in [0.0;1.0] interval
* `Fitness` minimum threshold
* max number of generations
* max execution time in ms


#### Multiple Colony Execution [distributed]
This is the distributed context, where Solution population is equally divided (partitioned) to be further executed on separate Spark node workers, runned through an `Era`, and finally ended.
Partitioning is a key concept here: a `DistributedDatasetProvider` helps creating such partitions and distributed database if needed, and a `MultiColonyEvolver` distributes processing around, collecting results back to Client.

Optionally, a specific `SolutionRenderer` can be implemented for returning a specific representation of the `Solution` other than raw data to the Client.
For example, one can need an html template representation for a web site.

Also, `EnvObserver` can be instructed to send asynchronous messages for coarse or fine tracking of executions.

An important distinction at this stage is, two different distributed flavours can be adopted:
- **execution only (XO)**, where only elaborations are made in memory in Spark nodes.
- **with data**, where elaborations and data are partitioned and executed on Spark nodes via RDD.
Specific templates via Maven archetypes can be used for this purpose. See [10' Single Colony Tutorial](http://elastxy.io/tutorials/#single-colony-tutorial) for a basic usage.

### Architecture
A detailed architecture component diagram is shown here below, showing modules and interactions.

![Architecture Component Diagram](/img/architecture-components.png)


#### Repositories
* **elastxy-framework**: framework repository, with core engine, APIs and distributed basic components. Sample Maven archetypes are also provided for start coding in a minute with basic local and distributed applications.
* **elastxy-applications** (TODO): module with a number of sample working applications covering main standard use cases, which can be run both locally and distributed.

#### Modules
The main components of the framework, from a deployment perspective, are:
* **elastxy-core**: module containing the **Genetics Algorithm Engine** with logics, domain and metadata, plus application components and configurations, interfaces and standard implementations, tools for rendering, collecting statistics, etc. 
* **elastxy-web**: the web application publishes all APIS and controls local and distributed `Experiment` executions, running cluster Driver and returning results.
* **elastxy-distributed**: the module containing the distributed algorithm parts, to be executed on the cluster driver and executors.

#### Templates

Templates are made via **Maven Archetypes**.

For a detailed explanation on how they work, please read following tutorials:
- **elastxy-singlecolony-archetype**: Single Colony Archetype,  see [10' Single Colony Tutorial](http://elastxy.io/tutorials/#single-colony-tutorial) for building your own app for local execution.
- **elastxy-multicolony-xo-archetype**: Multi Colony Execution-Only (XO) Archetype, see [20' Multi Colony Tutorial](http://elastxy.io/tutorials/#multi-colony-tutorial) for building a sample distributed application.
- **elastxy-multicolony-data-archetype** (TODO): Multi Colony Data Archetype, it's an extension of Multi Colony XO, with specific data management components declared.


#### Runtime
Locally, a Spring Boot **Web Application** exposes APIs for running ElastXY applications. Examples are available as Postman Collections and documented via Swagger API Documentation.

Distributed processing is also started by Web Application APIs, runned inside as a Driver, then delivered to **Apache Spark Cluster** coordinated by a cluster manager (Apache Mesos, YARN or Standalone Cluster), either on-premise or optionally by connecting cloud computing services, like Amazon EMR or Google Dataproc.

Optionally, ElastXY will able to ingest and analyse distributed database via **Apache Cassandra** based `DatasetProvider`, and send asynchronous events and results with an `EnvObserver` implemented via Apache Kafka.

#### Deployment
ElastXY works either locally (Single Colony) or in a clustered environment (Multi Colony), deployed either:
* as a standalone Spring Boot application
* distributed in an on-premise Apache Spark cluster datacenter
* distributed as a cloud application via containerized Docker images and controlled by its Rancher Catalog
* or even, it can produce an uberjar to be published to Google Cloud Dataproc or Amazon AMS EMR Spark processing providers

#### Built With

* [Maven](https://maven.apache.org/) - Dependency Management
* [Spring](https://spring.io/) - adopted for building the web application via Spring Boot and APIs via Spring MVC 
* [Postman](https://www.getpostman.com/apps) - collection of sample calls for interacting with ElastXY
* [Apache Spark](https://spark.apache.org/) - The Distributed Computing Framework natively adopted

Optional:
* [Apache Mesos](https://mesos.apache.org/) - The Cluster Manager used for governing both Spark cluster and containers
* [Apache Cassandra](http://cassandra.apache.org/) - The locality aware Distributed Database holding data under the hood
* [Apache Kafka](https://kafka.apache.org/) - The Messaging System for collecting events during execution
* [Docker](https://www.docker.com/) - Used to containerize deliverables
* [Rancher](https://rancher.com/) - The Container Management console


## About the project 

Why "ElastXY"?
> Nothing special, **X** and **Y** being the two most famous chromosomes grouping genes of human genetic material, and **Elastic** given scalable capabilities of the framework.

Why the butterfly?

> At the heart of Genetic Algorithms lies Probability, basically being them modeled as a convolution of statistical distributions over time (see Markov chains) with possibly convergent results under certain circumstances, or non-convergent results in others.

> The idea of Markov chains reminded me Statistics course at University and the Butterfly Effect, which is not something I could foresee in my framework, nonetheless imagination wanders on how a slight ripple on big sea of data and tiny schemata could grow after Eras and Eons... to a definite, useful, Solution. Or nothing interesting.

> Gabriele Rossi (author of ElastXY Framework)

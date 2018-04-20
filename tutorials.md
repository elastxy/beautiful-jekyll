---
layout: page
title: Tutorials
subtitle: Manuals and tutorials page
bigimg: /img/documentation.jpg
---

## Table of contents

- [Quick start](#quick-start)
- [Single Colony tutorial](#single-colony-tutorial)
- [Multi Colony tutorial](#multi-colony-tutorial)

## Quick Start
These instructions will get you a copy of the project up and running on your local machine for showcasing basic features, development and testing purposes.

A Deployment section will be provided in the future with notes on how to deploy the project on a live and distributed system.

### Prerequisites
- JDK 8/9
- Maven 3.x

### Build from sources
*Approx time: 5'*
1. Enter a directory of your choice (<ELASTXY_HOME>), and clone or download the latest version of ElastXY Framework:

`git clone https://github.com/elastxy/elastxy-framework.git`

2. Change directory to \<ELASTXY_HOME\>/elastxy-framework and issue a Maven build:

`mvn clean install`

It'll take a while, as there are many dependencies for enabling all framework features. Next, you should see something like this:

<img src="/img/build-successful.png" />

Now you should have all ElastXY artifacts in your local Maven repo. Congratulations! :)

(And maybe you already spotted an ASCII representations of system evolution as long as tests were executed..)

## Single Colony Tutorial
*Approx time: 10-15'*

### Build your own local StartApp
 
The example will show you a simple showcase `Application`, and how to create your own project for a local (non distributed) env.

This simple showcase project is a variation of a well known Genetic Algorithm problem (and the first the author did, BTW), where the goal is to find an arithmetic expression resulting in a given target number.
> Example: 500=250 * 2, or 1000 / 2, or -500 + 1000, etc.

Here `Target` is a 64bit integer belonging to [-1000000;+1000000] interval, so with a brute-force approach you could explore potentially 4000000000000 (4*10^12) solutions! Let's have a look on how ElastXY can help here.

### Step 1: Create the `Application`

The core concept of ElastXY is `Application`.
You can start defining your own by creating a Maven project from a blueprint by running following command in an empty directory of your choice:

`mvn archetype:generate -DarchetypeGroupId=org.elastxy -DarchetypeArtifactId=elastxy-app-archetype -DarchetypeVersion=0.1.0-SNAPSHOT`

When prompted, insert "com.acme" as groupId and "ElastXYApp" as artifactId, leaving default package and version.

Now you should see a simple Maven project structure, bundling an initial example `Application`.

### Step 2: Change default `Target` value

Every `Application` comes with a `Benchmark` configuration for testing algorithm performance after any parameters change. Think of it as our default values, for now.

Default `Target` for this `Application` is 235000, as you can see in configuration file here:
`src/main/resources/benchmark.json`

For the sake of curiosity, you can also set your own magic number, opening configuration file and putting your number (say 777) near target parameter:

```json
"applicationSpecifics": {
	"target" : {
		"TARGET_EXPRESSION_RESULT" : 235000
	},
...
```

And, of course you can start playing with other parameters around, if you please ;)


### Step 3: Execute the `Experiment`

The `Experiment` is where the execution `Environment` resides, controls program flow until ended, and returns outcomes as `Results` and `ExperimentStats`.

It's time to build and test an execution: let's look it at work! Type:

```mvn test```

Application is bootstrapped and launched:
```
23:45:50.200 [main] INFO ... .AppBootstrap - Applications found: [StartApp]
23:45:50.200 [main] INFO ... .AppBootstrap - >> Bootstrapping application 'StartApp'
23:45:50.200 [main] INFO ... .AppBootstrap -    Building components..
23:45:50.225 [main] INFO ... .AppBootstrap -    Wiring components..
23:45:50.225 [main] INFO ... .AppBootstrap -    Initializing components..
23:45:50.226 [main] INFO ... .AppBootstrap -    Welcome to 'StartApp' application! <!!!>o
23:45:50.226 [main] INFO ... .AppBootstrap - Bootstrap ElastXY DONE.
```

### Step 4: See the results

After about 3" execution, you will notice an output similar to this one, much depending on how much you were lucky:
```
[1] elastxy>         |0---10---20---30---40---50---60---70---80---90---100
[1] elastxy>        1|--------------------------------------------------- | 0.9999999945 |SOL:Ge[[(P:0,M:operand)-158441, (P:1,M:operator)+, (P:2,M:operand)387971]] > Ph[(NumberPhenotype) 229530] > F[Value: 0.99999999453000128545, Check: true]
[1] elastxy>        2|--------------------------------------------------- | 0.9999999945 |SOL:Ge[[(P:0,M:operand)-158441, (P:1,M:operator)+, (P:2,M:operand)387971]] > Ph[(NumberPhenotype) 229530] > F[Value: 0.99999999453000128545, Check: true]
...
[1] elastxy>     3000|--------------------------------------------------- | 0.9999999986 |SOL:Ge[[(P:0,M:operand)-112897, (P:1,M:operator)-, (P:2,M:operand)-347896]] > Ph[(NumberPhenotype) 234999] > F[Value: 0.99999999999900000023, Check: true]
[1] elastxy>     4000|--------------------------------------------------- | 0.9999999986 |SOL:Ge[[(P:0,M:operand)-112897, (P:1,M:operator)-, (P:2,M:operand)-347896]] > Ph[(NumberPhenotype) 234999] > F[Value: 0.99999999999900000023, Check: true]
[1] elastxy>
[1] elastxy> ******* SUCCESS *******
[1] elastxy>
[1] elastxy> Best match:
[1] elastxy> SOL:Ge[[(P:0,M:operand)582896, (P:1,M:operator)+, (P:2,M:operand)-347896]] > Ph[(NumberPhenotype) 235000] > F[Value: 1.00000000000000000000, Check: true]
[1] elastxy> Number of generations: 4637
[1] elastxy> Total execution time (ms): 2327

```
Basically, ElastXY is saying:
> After having run 4637 generations, I found an optimal solution:
> 
> 582896 + (-346896) = 235000

This is your first ElastXY `Application`! Good job! :sparkles:

### Bonus Step 5: Play with `Genes` metadata
If you had a look to following file, you may have seen an interesting feature:

`src/main/resources/genes.json`

Representation of this problem schemata* is entirely based on metadata: for example, you can see definition of genes composing the chromosomes as two user-defined type: "operand" and "operator".

If you reduce available operators `Gene` to "*" and "\\" and relaunch the execution, you may notice how it's more difficult the job, even converging.

`mvn test`

**: schemata are minimal evolving genetic material strings the algorithm is based on, typically group of genes, like chromosomes*

## Multi Colony Tutorial
*Approx time: 20-30'*

### Build your first distributed ElastXY app

The example will show you a little more complex showcase `Application`, and how to run it on a local **Apache Spark** cluster.

As a prerequisite, we assume you have cloned ElastXY and compiled modules to a `ELASTXY_HOME` directory as a part of first example.

Please note that to keep example as simple as possible, we will run Apache Spark cluster with a minimal setup in a Standalone mode cluster with all production settings turned off (e.g. security, cluster managemet or async messaging). For a meaningful setup, please look at Documentation, "Setting up a Cluster" (TODO). 

### Step 1: Install and run Apache Spark

Please follow [detailed instructions here](sparkforelastxy.md).

### Step 2: Run ElastXY in distributed mode

Two words explanation: ElastXY web application API will run a simple benchmark application requesting execution to Spark Driver, which in turn will delegate Workers the hard job until an end condition or a best match `Solution` are found.

Then, results is collected by Spark Driver from Workers and made it available to web application to be returned to User.
(We haven't configured messaging system for this example, so we communicate by means of temp messages on shared local directory on file system).

- Open file `elastxy-web/src/main/resources/distributed.properties`
- Change properties accordingly to your environment settings. Please note that for results to be returned by APIs, the `webapp.inbound.path` must be identical to `driver.outbound.path`, as explained above.
- Execute following Maven command:

`mvn exec:java -Dexec.mainClass="org.elastxy.web.application.ElastXYWebApplication"`

You should see SpringBoot web application bootstrapped with all showcase applications running on port 8080:
```
... : Tomcat started on port(s): 8080 (http)
... : Started ElastXYWebApplication in 19.14 seconds (JVM running for 20.543)
```

- A [Swagger UI](http://localhost:8080/swagger-ui.html#/) is available when running locally (to be further polished)

- Call following APIs to run your local cluster, from a web browser address bar or your favourite HTTP client:

`http://localhost:8080/distributed/local/experiment/expressions_d`

Note: you can also import ready-to-go ElastXY Postman collection from `<ELASTXY_HOME>/src/main/resources`.

Now, you should see the raw `Solution` result as a reponse, like this:

```javascript
...
"phenotype": {
	"value": 235000
},
"fitness": {
	"value": 1,
...
```

There are other means to get results, and resulting response could be more friendly either, but, ehy!, this is just a simple example :)

Go on reading for a further description of how ElastXY works, capabilities, and so on.

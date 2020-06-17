# Exercise 1: Introduction to Appsody and Codewind

In this exercise, we will introduce Appsody, which is the underpinning development flow in Kabanero, along with its integration into IDEs using Codewind. In particular you will become experienced with:

- the components of the Appsody development toolbox
- the concept of pre-configured "stacks" and templates for popular open source runtimes (such as Node.js and Spring Boot) on which to build applications
- the Appsody command-line interface to develop containerized applications, how to run and test them locally.

## Prequisites

You should have already carried out the tasks from `Prerequisites`. Check that you have access to the Appsody CLI, by executing the following command:

```bash
appsody version
```

The output should be:

```bash
$ appsody version
appsody 0.6.4
```

## Configure the Appsody CLI

In this section we'll configure our Appsody CLI to pull in Collections.

### List existing Appsody stacks

The Appsody CLI gives you access to stacks, which are stored in stack repositories. These can be local, private to the Enterprise or public. To get the list of available repos, run this command.

```
appsody repo list
```

You should see output similar to the following:

```bash
$ appsody repo list
NAME            URL
*incubator      https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
```

The exact repo list may be different to the above. `incubator` is one of the repos in the appsody project public hub (`appsodyhub`). For this workshop we are going to use the private enterprise-grade collection of stacks that come with the Kabanero open source project (which is part of Cloud Pak for Applications). So the first thing we need to do is to tell the CLI about this.

### Add Collection to Appsody

From the Cloud Pak for Applications instance page get the `Appsody URL`, for example:

https://github.com/kabanero-io/kabanero-stack-hub/releases/download/0.6.5/kabanero-stack-hub-index.yaml

Use the appsody CLI to add the Collection repo.

```bash
appsody repo add kabanero https://github.com/kabanero-io/kabanero-stack-hub/releases/download/0.6.5/kabanero-stack-hub-index.yaml
```

Now when we get our list of repos, we should see Kabanero listed:

```bash
appsody repo list
```

You should see output similar to the following:

```bash
$ appsody repo list

kabanero-0.6.5	https://github.com/kabanero-io/kabanero-stack-hub/releases/download/0.6.5/kabanero-stack-hub-index.yaml
*incubator      	https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
```

We can now list the appsody stacks available in the Collection:

```bash
appsody list kabanero
```

You should see output similar to the following:

```bash
REPO          	ID                            	VERSION  	TEMPLATES        	DESCRIPTION
kabanero	java-microprofile [Deprecated]	0.2.26   	*default         	Eclipse MicroProfile on Open Liberty & OpenJ9 using Maven
kabanero	java-openliberty              	0.2.3    	*default         	Open Liberty & OpenJ9 using Maven
kabanero	java-spring-boot2             	0.3.28   	*default, kotlin 	Spring Boot using OpenJ9 and Maven
kabanero	nodejs                        	0.3.3    	*simple          	Runtime for Node.js applications
kabanero	nodejs-express                	0.2.10   	scaffold, *simple	Express web framework for Node.js
```

Given that we'll exclusively be using the kabanero stacks in this workshop, for ease of use we can set the kabanero repository to be the default for the CLI:

```bash
appsody repo set-default kabanero
```

Now is we get the list of repos, we should see kabanero is the default:

```bash
$ appsody repo list

*kabanero-0.6.5	https://github.com/kabanero-io/kabanero-stack-hub/releases/download/0.6.5/kabanero-stack-hub-index.yaml
incubator      	https://github.com/appsody/stacks/releases/latest/download/incubator-index.yaml
```

## Use Appsody CLI to build, test, run, and debug

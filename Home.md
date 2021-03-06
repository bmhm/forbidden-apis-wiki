# Policeman's Forbidden API checker #
This project implements the ANT / Gradle task and Maven Mojo announced in the [Generics Policeman Blog](https://blog.thetaphi.de/2012/07/default-locales-default-charsets-and.html).
It checks Java byte code against a list of "forbidden" API signatures.

[![Maven Central](https://img.shields.io/maven-central/v/de.thetaphi/forbiddenapis.svg)](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22de.thetaphi%22%20AND%20a%3A%22forbiddenapis%22)
[![Build Status](https://jenkins.thetaphi.de/job/Forbidden-APIs/badge/icon)](https://jenkins.thetaphi.de/job/Forbidden-APIs/)

## A new Tool for the Policeman ##
I started to hack a tool as a custom [Apache Ant](http://ant.apache.org/) task using [ASM](http://asm.ow2.org/) (Lightweight Java Bytecode Manipulation Framework). The idea was to provide a list of methods signatures, field names and plain class names that should fail the build, once bytecode accesses it in any way. A first version of this task was published in as [Apache Lucene](http://lucene.apache.org/core/) issue [LUCENE-4199](https://issues.apache.org/jira/browse/LUCENE-4199), later improvements was to add support for fields ([LUCENE-4202](https://issues.apache.org/jira/browse/LUCENE-4202)) and a sophisticated signature expansion to also catch calls to subclasses of the given signatures ([LUCENE-4206](https://issues.apache.org/jira/browse/LUCENE-4206)).

## About the Github project ##
This project was started as a fork of the internal [Apache Ant](http://ant.apache.org/) Task. It additionally provides a [Apache Maven](http://maven.apache.org/) [Mojo](http://maven.apache.org/guides/introduction/introduction-to-plugins.html), that can check your application classes against forbidden signatures, too. For simple use-cases, a command line interface is available, too. In version 2.0 Gradle support was added.

The Apache Ant task and the Apache Maven Mojo are available for download or use with Maven/Ivy through [Maven Central](http://repo1.maven.org/maven2/de/thetaphi/forbiddenapis/) and [Sonatype](http://oss.sonatype.org/content/repositories/releases/de/thetaphi/forbiddenapis/) repositories. The Gradle plugin is available through the [Gradle Plugin portal](https://plugins.gradle.org/plugin/de.thetaphi.forbiddenapis), but it's also hosted on Maven/Sonatype. Nightly snapshot builds are done by the [Policeman Jenkins Server](https://jenkins.thetaphi.de/job/Forbidden-APIs/) and can be downloaded from the [Sonatype Snapshot](https://oss.sonatype.org/content/repositories/snapshots/de/thetaphi/forbiddenapis/) repository.

## News ##
**The current version is 3.0.1, released on 2020-06-03**. Changes for each released version are listed on the following page: [Changes](Changes)

## Documentation ##
  * [Apache Ant Usage Instructions](AntUsage)
  * [Apache Maven Usage Instructions](MavenUsage)
  * [Gradle Usage Instructions](GradleUsage) (version 2.0+)
  * [Command Line Usage Instructions](CliUsage) (version 1.1+)
  * [Syntax of Forbidden Signatures](SignaturesSyntax)

  * [Nightly Snapshot Documentation](https://jenkins.thetaphi.de/job/Forbidden-APIs/javadoc/) (detailed)
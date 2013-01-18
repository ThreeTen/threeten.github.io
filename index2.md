---
layout: default
title: ThreeTen
subtitle: The ThreeTen project
abstract: The ThreeTen project
---

The ThreeTen project is providing a new date and time API for JDK 1.8 as part of JSR-310.

#### Main project

The main strand of active development for JDK 1.8 is in [OpenJDK](http://openjdk.java.net/projects/threeten/).
Source code was [originally located](https://github.com/ThreeTen/threeten) here at GitHub but is now in Mercurial at OpenJDK.
The [issue tracker](https://github.com/ThreeTen/threeten/issues) is currently still located here at GitHub.

#### Backport

A [backport](https://github.com/ThreeTen/threetenbp) has been provided for JDK 1.7 hosted here at GitHub.
The backport is also available in the [Maven Central repository](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.threeten%22%20AND%20a%3A%22threetenbp%22).

The backport [Javadoc](http://threeten.github.com/threetenbp/apidocs) is available for browsing.

The backport is NOT an implementation of JSR-310, as that would involve many complex legal/procedural hoops.
However, it is designed to allow an easy move from JDK 1.7 to JDK 1.8.

The backport is used in projects such as [OpenGamma](https://github.com/OpenGamma/OG-Platform).

#### Documentation

This site holds [reference documentation](articles/index.html) for ThreeTen and JSR-310.
This supplements the [Javadoc](http://threeten.github.com/threetenbp/apidocs), providing a broader user guide.
The documentation is applicable to both the backport and JDK 1.8 - only the package name changes.

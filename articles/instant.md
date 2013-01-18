---
layout: article
title: Instant
subtitle: User guide for the Instant class
categories:
- articles
---

The `Instant` class represents an instant in time.

It is an ideal object for storing time-stamps of log events.
For example, the instant of an event such as saving a file to GitHub can be recorded in an `Instant`.

The time-line of human experience consists of a continuous set of instants.
In ThreeTen, these instants can be recorded at nanosecond precision in the `Instant` class.


#### Creating an Instant

There are a number of ways to create an `Instant`.

##### Now

Creating the current instant is achieved using one of the `now` methods:

{% highlight java %}
Instant a = Instant.now();
Instant b = Instant.now(aClock);
{% endhighlight %}

The first method (a) obtains the current instant from the system clock, which is a wrapper
around `System.currentTimeMillis()`.
The second method (b) uses a `Clock` object to provide further control over the current instant.

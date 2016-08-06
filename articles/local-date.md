---
layout: article
title: LocalDate
subtitle: User guide for the LocalDate class
categories:
- articles
---

The `LocalDate` class represents a date.
It contains no representation of the time-of-day.

It is an ideal object for representing birth dates, or dates on a wall calendar.
For example, the date "18th January 2013" could be stored in a `LocalDate`.

The "local" part of the name refers to the local time-line.
Specifically, a `LocalDate` has no reference to a time-zone or offset from UTC/Greenwich.

#### Creating a LocalDate

There are a number of ways to create a `LocalDate`.

##### Today

Creating the date "today" can be achieved using one of the three `now` methods:

{% highlight java %}
LocalDate a = LocalDate.now();
LocalDate b = LocalDate.now(ZoneId.of("Europe/Paris"));
LocalDate c = LocalDate.now(aClock);
{% endhighlight %}

The key point is that determining today's date requires knowledge of the time-zone.
The first method (a) uses the Java default time-zone, as per `TimeZone.getDefault()`.
The second method (b) allows the time-zone to be explcitly controlled.
The third method (c) uses a [Clock](clock.html) object, which provides further control over the current instant and time-zone.

##### Using fields

If you want to hard code the creation of a date, or have fields available, these factory
methods can be used:

{% highlight java %}
LocalDate a = LocalDate.of(2013, 1, 18);              // 18th January 2013
LocalDate b = LocalDate.of(2013, Month.JANUARY, 18);  // 18th January 2013
LocalDate c = LocalDate.ofYearDay(2013, 32);          // 1st February 2013 (day-of-year 32)
{% endhighlight %}

The first two methods, (a) and (b), create a date from the year, month-of-year and day-of-month.
The last method (c) creates a date from the year and day-of-year.

The month accepts values from 1 to 12, as in common daily use.
The enum `Month` can be used instead of the numeric value to make code more readable.
The day-of-year value runs from 1 to 365, or to 366 in a leap year.
If an invalid date is passed in, such as the 31st April or the 30th February, then an exception is thrown.




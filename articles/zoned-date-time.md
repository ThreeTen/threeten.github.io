---
layout: article
title: ZonedDateTime
subtitle: User guide for the ZonedDateTime class
categories:
- articles
tags: zone date time hour minute second nano
---

The `ZonedDateTime` class represents a date-time with a time-zone. This class stores all date and time fields, to a precision of nanoseconds, and a time-zone, with a zone offset used to handle ambiguous local date-times. For example, the value "2nd October 2007 at 13:45.30.123456789 +02:00 in the Europe/Paris time-zone" can be stored in a ZonedDateTime.

This class handles conversion from the local time-line of LocalDateTime to the instant time-line of Instant. The difference between the two time-lines is the offset from UTC/Greenwich, represented by a ZoneOffset.

A ZonedDateTime holds state equivalent to three separate objects, a LocalDateTime, a ZoneId and the resolved ZoneOffset. The offset and local date-time are used to define an instant when necessary. The zone ID is used to obtain the rules for how and when the offset changes. The offset cannot be freely set, as the zone controls which offsets are valid.

#### Creating a ZonedDateTime

There are a number of ways to create a `ZonedDateTime`.

##### Current Date Time with Zone information

Current date-time with zone information can be created using the following `now` methods:

{% highlight java %}
// output is based on code run in Europe/London time zone

ZonedDateTime a = ZonedDateTime.now();                          //2013-03-02T21:52:25.371+01:00[Europe/London]
ZonedDateTime b = ZonedDateTime.now(ZoneId.of("Europe/Paris")); //2013-03-02T22:52:25.374+02:00[Europe/Paris]
	
ZonedDateTime c = ZonedDateTime.now(aClock );                   //2013-07-02T16:52:25.374-04:00[America/Indiana/Indianapolis]
// where Clock aClock = Clock.fixed(Instant.now(), ZoneId.of("America/Indiana/Indianapolis"));
{% endhighlight %}

The first method (a) uses the Java default time-zone, as per `TimeZone.getDefault()`.
The second method (b) allows the time-zone to be explicitly controlled.
The third method (c) uses a [`Clock`](clock.html) object, which provides further control over the current instant and time-zone. This will come handy in testing scenarios as we can provide an alternate clock. 

##### Using fields

If you want to hard code the creation of ZonedDateTime, or have fields available, these factory
methods can be used:

{% highlight java %}


ZonedDateTime a = ZonedDateTime.of(aLocalDateTime, ZoneId.of("Europe/Paris"));                               // 2013-12-18T14:30+01:00[Europe/Paris]
// where LocalDateTime aLocalDateTime = LocalDateTime.of(2013, 12, 18, 14, 30) 
 
ZonedDateTime b = ZonedDateTime.ofInstant(Instant.ofEpochSecond(1),  ZoneId.of("Europe/Paris"));             // 1970-01-01T01:00:01+01:00[Europe/Paris

ZonedDateTime c = ZonedDateTime.ofInstant(aLocalDateTime, preferredZoneOffset, ZoneId.of("Europe/Paris"));   // 2013-12-18T14:30+01:00[Europe/Paris]
// where ZoneOffset preferredZoneOffset = ZoneOffset.ofHours(1)

ZonedDateTime d = ZonedDateTime.ofLocal(aLocalDateTime, ZoneId.of("Europe/Paris"), preferredZoneOffset);     // 2013-12-18T14:30+01:00[Europe/Paris]
ZonedDateTime e = ZonedDateTime.ofStrict(aLocalDateTime, preferredZoneOffset, ZoneId.of("Europe/Paris"));  // 2013-12-18T14:30+01:00[Europe/Paris]
                                     
{% endhighlight %}

Remember that preferredZoneOffset can be applied only if is available in the valid offset from UTC/GMT of the localDateTime as defined by zoneRules of given Zone.
If it is invalid then it is corrected to available offset and that value is used except for method (e) which throws an exception in this case.  

Although the methods (d) and (e) has got similar arguments, there is a fundamental difference in the way offset is applied. 
For localDateTime in Europe/London the valid offset with paris is one hour for this time. 
For method (d) if we provide an invalid offset say 2 hours, it will provide correct the offset and use one hour as the offset while method (e) will throw an exception

ZonedDateTime f = ZonedDateTime.ofLocal(aLocalDateTime, ZoneId.of("Europe/Paris"), ZoneOffset.ofHours(2));  // 2013-12-18T14:30+01:00[Europe/Paris]
ZonedDateTime g = ZonedDateTime.ofStrict(aLocalDateTime, ZoneOffset.ofHours(2), ZoneId.of("Europe/Paris")); // throws DateTimeException

##### By Parsing Text

{% highlight java %}

ZonedDateTime a = ZonedDateTime.parse("2012-06-30T12:30:40Z[GMT]");               
ZonedDateTime b = ZonedDateTime.parse("2012-06-30T12:30:40Z[UT]");              
ZonedDateTime c = ZonedDateTime.parse("2012-06-30T12:30:40Z[UTC]");              
ZonedDateTime d = ZonedDateTime.parse("2012-06-30T12:30:40+01:00[+01:00]");       
ZonedDateTime e = ZonedDateTime.parse("2012-06-30T12:30:40+01:00[GMT+01:00]");    
ZonedDateTime f = ZonedDateTime.parse("2012-06-30T12:30:40+01:00[UT+01:00]");     
ZonedDateTime g = ZonedDateTime.parse("2012-06-30T12:30:40+01:00[UTC+01:00]");    
ZonedDateTime h = ZonedDateTime.parse("2012-06-30T12:30:40-01:00[-01:00]");       
ZonedDateTime i = ZonedDateTime.parse("2012-06-30T12:30:40-01:00[GMT-01:00]");    
ZonedDateTime j = ZonedDateTime.parse("2012-06-30T12:30:40-01:00[UT-01:00]");     
ZonedDateTime k = ZonedDateTime.parse("2012-06-30T12:30:40-01:00[UTC-01:00]");    
ZonedDateTime l = ZonedDateTime.parse("2012-06-30T12:30:40+01:00[Europe/London]");
                                        
ZonedDateTime m = ZonedDateTime.parse("2012 06 30 12 30 40 Europe/London", aDateTimeFormatter); //2012-06-30T12:30:40+01:00[Europe/London]    

// where DateTimeFormatter aDateTimeFormatter = DateTimeFormatter.ofPattern("y M d H m s VV");

{% endhighlight %}

# What is UID?
The `UID` purpose is to define a unique identifier across all the calendar components. 
It is basically a random generated sequence that will assure the uniqueness of every calendar component.
This property is mandatory in defining calendar components. 
Everyone  must assure that this unique identifier is added at the Creation Time of his calendar component, to be sure of the uniqueness of all the components.
An update of the component will not determine an update of the `UID` property value.
In order to have certainty of the uniqueness of every `UID`, a good algorithm should be created for generating.

An example of an Event containing the `UID` Property
<pre><code>
BEGIN:VEVENT
SUMMARY:Lunchtime meeting
<strong>UID:ff808181-1fd7389e-011f-d7389ef9-00000003</strong>
DTSTART;TZID=America/New_York:20160420T120000
DURATION:PT1H
END:VEVENT
</code></pre>

# How should an UID be generated
The `UID` globally identifies a calendar component inside a calendaring system.
Because it is randomly generated by the system, the generating mechanism should be created taking into account as many variable as it can in order to generate a unique value.
[RFC7986](https://tools.ietf.org/html/rfc7986) now recommends that UID values be formed from hex-encoded UUID values.
In addition, if other types of opaque UID values are used, they must have a length less than 255 octets and must use a restricted set of ascii characters.
Previous recommendations in [RFC5545](https://tools.ietf.org/html/rfc5545) about how to construct the UID value are no longer considered valid, in particular because they represent potential security and privacy threats.

# Consequences of not doing it right
Because the `UID` globally identifies a calendar component, not having a good way of generating this property can cause a lot of problems.
Using a simple `UID` generation algorithm might lead to overridden events with the same `UID` generated by others using the same generation mechanism. 
Every creation of a calendar component with a UID that is already contained by other component will lead in the end to the update of the older component.
Once generated, the calendar component should keep the same `UID` all the time, so that no matter which part of the event is changed, the UID-Reference is the same and the calendar can be updated until that event will be deleted.

# UID usage in VEVENT, VTODO, and VJOURNAL
UID Property is very important for interoperability between scheduling applications that MUST generate it in `VEVENT`, `VTODO` and `VJOURNAL` calendar components.
This property is mandatory for each of the calendar components listed above and should be defined as consistent as possible for correlating scheduling messages.


# Handling UIDs inside Recurrent Events
The `UID` is a property that uniquely defined a calendar component at the CREATION Time. No further changes made on that component should affect the value of the `UID`.
One of the cases is a `RECURRENT-EVENT`, which contains a recurrence set of instances and a master event. 
In the case one of the instances gets modified a RECURRENT-ID VEVENT is created. The `RECURRENCE-ID` field is pointing to the date within the recurring rule but the `UID` will remain the same as the master event.
In this scenario, the uniqueness will be assured by the `UID` together with the `RECURRENCE-ID` values.

An example can be seen below:
<pre><code>
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//ABC Corporation//NONSGML My Product//EN
BEGIN:VEVENT
SUMMARY:Lunchtime meeting
<strong>UID:ff808181-1fd7389e-011f-d7389ef9-00000003</strong>
DTSTART;TZID=America/New_York:20160223T110000
DTEND;TZID=America/New_York:20160226T120000
RRULE:FREQ=DAILY
LOCATION:Mo's bar - back room
END:VEVENT
BEGIN:VEVENT
SUMMARY:Lunchtime meeting
<strong>UID:ff808181-1fd7389e-011f-d7389ef9-00000003
RECURRENCE-ID;TZID=America/New_York:20160225T120000</strong>
DTSTART;TZID=America/New_York:20160223T110000
DTEND;TZID=America/New_York:20160226T120000
LOCATION:Mo's bar - back room
END:VEVENT
END:VCALENDAR
</code></pre>

On the second event the uniqueness will consist on: 
<pre><code>UID:ff808181-1fd7389e-011f-d7389ef9-00000003
RECURRENCE-ID;TZID=America/New_York:20160225T120000</code></pre>
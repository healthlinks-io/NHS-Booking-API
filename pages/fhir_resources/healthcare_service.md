---
title: Healthcare Service | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: healthcare_service.html
summary: A Healthcare Service delivered by an Organisation at a Location which provides a number of Schedules containing Slots in Appointment booking.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is optionally returned linked to a <a href='schedule.html'>Schedule resource</a> which provides one or more <a href='slot.html'>Slots</a>. For more clarification see <a href='resources_overview.html#urgent--emergency-care-appointments-apis'>the diagram on the FHIR Resources overview page</a>.

The HealthcareService.id is a key search field used when <a href='search_free_slots.html'>searching for Slots</a>. HealthcareService.id is used to discriminate between multiple clinical services which may be administered by one <a href='organisation.html'>organisation</a>, using one instance of a system. This allows the search to specify a detailed clinical service which has been located in the DoS following a clinical assessment.

{% include custom/fhir.reference.html resource="HealthcareService" page="CareConnect-HealthcareService-1" fhirname="HealthcareService" fhirlink="healthcare_service.html" content="-" userlink="" %}

## Key FHIR Elements ##

The following FHIR elements are key to this implementation :

| Element | Cardinality | Description | Example(s) |
| --- | --- | --- | --- |
| id | [1..1] | An id which has been retrieved from the DoS, this value is also the ASID of the Service, issued to the service when it is approved to go live by NHS Digital. This id will be the link to this resource from any Schedules. | 1231231234 |
| providedBy | [0..1] | An optional reference to the <a href='organisation.html'>Organisation</a> which delivers this service | `{ "reference": "https://directory.spineservices.nhs.uk/STU3/Organization/GD2" }` |
| location | [0..1] | An optional reference to a <a href='location.html'>Location</a> resource, which describes where the service is. **NB: The 'user friendly' location of the service will already have been determined from the DoS when selecting this service, so this Location is far less important** | `{ "reference": "/Location/1237654" }` |

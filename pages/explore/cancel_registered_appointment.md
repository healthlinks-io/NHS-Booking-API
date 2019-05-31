---
title: Cancel a registered appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: cancel_registered_appointment.html
summary: "Details the Cancel a Registered Appointment interaction"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A system wishes to cancel an Appointment which was previously <a href='register_an_appointment.html'>registered</a> and has since been <a href='cancel_an_appointment.html'>cancelled</a>.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Utilises TLS Mutual Authentication for system level authentication.

## Request Body ##

The request body is sent using an http `PUT` method.

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource MUST be <a href='get_an_appointment.html'>retrieved from the Registry</a> in order to ensure that no data is lost.**

- The registry WILL reject cancellation requests where differences (other than those specified) are detected between the original and updated resource.

## Response ##

### Success ###
Where the request succeeded, the response WILL include a status of `200` **OK**.
The response WILL include a Location header giving the absolute URL of the created Appointment. This URL WILL remain stable, and the resource WILL support RESTful updates using a PUT request to this URL.
The response body WILL include the updated Appointment.

### Failure ###
- If the request fails because of a business rule (for example if differences are detected between the existing and updated Appointment), the response **WILL** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This WILL be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because the request body failed validation against the relevant profiles, the response **WILL** include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This **WILL** be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response **WILL** include a status of `403` **Forbidden**.
This **WILL** be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the request body was simply invalid, the response **WILL** include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response **WILL** include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status **SHOULD NOT** be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status **MAY** be retried.

## Sample request body ##

```xml
<Appointment xmlns="http://hl7.org/fhir">
    <meta>
        <profile value="https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"></profile>
    </meta>
    <id value="b7e99463-00a1-45fc-98aa-02301c103aba"></id>
    <identifier>
        <system value="urn:ietf:rfc:3986"></system>
        <value value="http://test.nhs.uk/test3"></value>
    </identifier>
    <status value="cancelled"></status>
    <start value="2019-02-01T10:51:23.620+00:00"></start>
    <created value="2019-02-01T10:51:23+00:00"></created>
    <participant>
        <actor>
            <identifier>
                <use value="official"></use>
                <system value="https://fhir.nhs.uk/Id/nhs-number"></system>
                <value value="1234554321"></value>
            </identifier>
        </actor>
    </participant>
</Appointment>
```
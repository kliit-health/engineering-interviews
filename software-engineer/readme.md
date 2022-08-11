# Software Engineer Position

For the Software Engineering candidate interview, we will be implementing a sample user story in a pair programming session. The goal of the session is not to necessarily complete the whole story, but for us to understand how you tackle problems, how you communicate and work with others, and how comfortable you are with the tools of the trade. During the session, ask as many questions as you need as well as speak to what you're doing/thinking as opposed to just coding in silence.

## User Story: Schedule an Appointment (New Patient) (REST API)

As a new patient to Kiira Health, I would like to be able to schedule an appointment with the provider of my choosing, so that I can receive quality medical care from one of Kiira's team of physicians.

### Requirements

The Kiira Health API should support the following endpoint to allow for the creation of appointments. It will take the patient's first name, last name, and email address, as well as the physician identifier, appointment date/time and reason. If the email provided matches does not match our records, we should also create a new patient record.

Only authorized applications are allowed to use this API.

#### Sample Request

```http
POST /appointments HTTP/1.1
Accept: application/json
Authorization: Bearer B3@r3rT0k3n=
Host: localhost:3000

{
    "first_name": "Serena",
    "last_name": "Williams",
    "email": "swilliams@kiira.io",
    "provider_id": "E8B85839-5F1B-4419-AD57-F878570A0891",
    "appointment_date": "2022-08-02T13:00:00Z",
    "appointment_type": "virtual",
    "appointment_reason": "tennis elbow"
}
```

A successful request should return the following response:

```http
HTTP/1.1 202 Accepted
Date: Thu, 2 Aug 2022 00:44:11 GMT
Content-Type: application/json
Content-Length: 206
Connection: close
vary: Accept-Encoding,User-Agent
content-encoding: gzip
x-content-type-options: nosniff
strict-transport-security: max-age=63072000; preload

{
    "id": "088C2F52-E79A-41AD-BBF9-85930332453A"
}
```

An invalid request should return the following response:

```http
HTTP/1.1 400 Bad Request
Date: Thu, 2 Aug 2022 00:44:11 GMT
Content-Type: application/json
Content-Length: 206
Connection: close
vary: Accept-Encoding,User-Agent
content-encoding: gzip
x-content-type-options: nosniff
strict-transport-security: max-age=63072000; preload

{
    "errors": [
        {
            "status": 400,
            "reason": "Provider does not exist"
        }
    ]
}
```

### Acceptance Criteria

- Given a _valid_ request, when I submit the request to `POST: /appointments`, then the response contains the newly created identifier for the appointment.
- Given a _valid_ request, when I submit the request to `POST: /appointments`, then the response code returned is `202 Accepted`.
- Given a _valid_ request, and the specified patient data corresponds to an existing patient, when I submit the request to `POST: /appointments`, then the response code returned is `202 Accepted`.
- Given a _valid_ request, and the specified patient data can't be correlated to any existing patient, when I submit the request to `POST: /appointments`, then a new patient record is added to the system.
- Given a _valid_ request, but the Authorization header contains an invalid bearer token, when I submit the request to `POST: /appointments`, then the response code returned is `401 Unauthorized`.
- Given an _invalid_ request, when I submit the request to `POST: /appointments`, then the response code returned is `400 Bad Request`.

### Assumptions

- Assume that a database already exists for patient and appointment data.
- The database schema can be whatever is needed to fit this scenario.
- Kiira applications already know how to obtain an OAuth token from an identity service in order to make calls to this API. (OAuth Client Credentials Flow)

### Definition of Done

- All unit and integration tests are passing.
- Source code is committed to the `main` branch in the __engineering-interviews__ repository via a pull request from a feature branch.
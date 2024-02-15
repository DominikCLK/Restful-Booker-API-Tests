# API tests report (UpdateBooking endpoint) using Postman 

This API test report is a summary of testing the UpdateBooking endpoint using Postman. The report contains information such as the test environment, tools used, test results and recommendations.

## QA:
- [Dominik Calak](https://github.com/DominikCLK)

  
## Date:
- 02.02.2024 - 06.02.2024

## Tests scope:
- Manual testing of the UpdateBooking endpoint and verifying compliance with the documentation
  
## Environment:
- Test [Restful-Booker-TEST]

# The API endpoint UpdateBooking tests showed a number of documentation irregularities and other defects. Below is a list of bugs.

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>

## 1. [BUG] Fields "firstname" and “lastname” accepts invalid data type (Boolean - false) instead String #001 _Priority High | Severity Major_ | [TEST env]

- Repro steps:
> 1. Use Auth - CreateToken request to send token
> 2. Use Booking - CreateBooking request to create booking (use body request) and send request:

```
{
    "firstname": "Dominik",
    "lastname": "Tester",
    "totalprice": 111,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "Breakfast"
}
```
> 3. Use Booking - UpdateBooking request to update data, replace body request “firstname” and “lastame” fields to boolean false

```
{
   "firstname": false,
   "lastname": false,
}
```

- Actual Result:
> 1. Response return boolean type
> 2. Status is 200 OK
   
- Expected result:
> 1. According to the documentation, these fields only accept Strings
> 2. Status 400 Bad request

> Documentation:

![image](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/4ac8c640-c245-414a-8da6-bb73d2bd3c3b)

> Recommendations
```
{
  "error": "Bad Request",
  "message": "Invalid data type. Expected string, but received boolean."
}
```

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>

## 2. [BUG] Floating-point variables in the body request (“totalprice” field ) are rounded to integers in the response #002 _Priority High | Severity Major_ | [TEST env]

- Repro steps:
> 1. Use Auth - CreateToken request to send token
> 2. Use Booking - CreateBooking request to create booking (use body request) and send request:

```
{
    "firstname": "Dominik",
    "lastname": "Tester",
    "totalprice": 100,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "Breakfast"
}
```
> 3. Use Booking - UpdateBooking request to update data, replace body request “totalprice” to float “ 100.58 “

```
{
    "totalprice": 100.58,
}
```
- Actual Result:
> 1. Totalprice in response is rounded to integers type

![2024-02-15_22h06_50](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/5eaebd87-4d4a-4666-8d55-3e8f3eee2bb4)

> 2. Status is 200 OK

- Expected result:
> 1. Totalprice in response should be the same as request
> Recommendations
Specify in the documentation whether the totalprice value should be rounded to integer

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>


## 3. [BUG] Field "totalprice" accepts invalid data type (String) instead Number and return null #003 _Priority High | Severity Major_  [TEST env]

- Repro steps:
> 1. Use Auth - CreateToken request to send token
> 2. Use Booking - CreateBooking request to create booking (use body request) and send request

```
{
    "firstname": "Dominik",
    "lastname": "Tester",
    "totalprice": 100,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "Breakfast"
}
```
> 3. Use Booking - UpdateBooking request to update data, replace body request “totalprice” to String

```
{
    "totalprice": "Text",
}
```
- Actual Result:
> 1. Response totalprice returns “null”
> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request

```
 { "error": "Bad Request",
  "message": "Invalid data type. Expected Number, but received String."
}
```
> Documentation

![2024-02-15_22h21_55](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/5f62534d-e87f-47b7-b11e-2a351b185533)

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>


## 4. [BUG] Only Boolean type should be accepted in the 'depositpaid' field. Missing validation #004 _Priority High | Severity Major_  [TEST env]

- Repro steps:
> 1. Use Auth - CreateToken request to send token
> 2. Use Booking - CreateBooking request to create booking (use body request) and send request

```
{
    "firstname": "Dominik",
    "lastname": "Tester",
    "totalprice": 100,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "Breakfast"
}
```
> 3. Use Booking - UpdateBooking request to update data, replace body request “depositpaid” to Number“

```
{
    "depositpaid": 1000,
}
```
- Actual Result:
> 1. Response depositpaid returns true
> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request
> 2. A depositpaid field should accept only Boolean type

> Documentation

![2024-02-15_22h28_46](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/5909f6b3-3374-4455-b9e4-c439245f9165)

> Recommendations
```
{
   "error": "Bad Request",
   "message": "Invalid data type. Expected Boolean, but received Number."} (or String)
}
```

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>

## 5. [BUG] Lack of verification of compliance of check in and check out dates #005 _Priority High | Severity Major_  [TEST env]

- Repro steps:
> 1. Use Auth - CreateToken request to send token
> 2. Use Booking - CreateBooking request to create booking (use body request) and send request

```
{
    "firstname": "Dominik",
    "lastname": "Tester",
    "totalprice": 100,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2025-01-01"
    },
    "additionalneeds": "Breakfast"
}
```
> 3. Use Booking - UpdateBooking request to update data, replace body request “bookingdates” to checkin: "2024-01-01" and checkout in past: "2023-01-01"

```
    "bookingdates": {
        "checkin": "2024-01-01",
        "checkout": "2023-01-01"
    }
```

- Actual Result:
> 1. The 'checkout' date is earlier than the 'check in' date in the response. Lack of verification of compliance of check in and check out dates
> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request
> 2. bookingdates fields should be verified. The checkout date cannot be earlier than the checkin date

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
  </a>
</p>




































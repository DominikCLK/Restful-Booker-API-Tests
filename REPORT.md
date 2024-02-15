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

1. ## [BUG] Fields "firstname" and “lastname” accepts invalid data type (Boolean - false) instead String #001 _Priority High | Severity Major_ | [TEST env]

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

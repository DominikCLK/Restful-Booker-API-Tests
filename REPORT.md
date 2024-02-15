# API tests report (_UpdateBooking_ endpoint) using Postman 

This API test report is a summary of testing the UpdateBooking endpoint using Postman. The report contains information such as the test environment, tools used, test results and recommendations.

## QA:
- [Dominik Calak](https://github.com/DominikCLK)

  
## Date:
- 02.02.2024 - 06.02.2024

## Tests scope:
- Manual testing of the _UpdateBooking_ endpoint and verifying compliance with the documentation

## Testware

- API (UpdateBooking endpoint) Documentation: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-UpdateBooking
- Collection file with requests (Postman) [Collection](https://github.com/DominikCLK/Restful-Booker-API-Tests/files/14298612/Restful-Booker.postman_collection.QA.Dominik.Calak.json) <-- file to download
- Environment file with variables (Postman) [Environment variables](https://github.com/DominikCLK/Restful-Booker-API-Tests/files/14298626/Restful-Booker-TEST.postman_environment.Dominik.Calak.QA.json) <-- file to download
  
## Environment:
- Test [Restful-Booker-TEST]

## The API endpoint _UpdateBooking_ tests showed a number of documentation irregularities and other defects. Below is a list of bugs: <a id="list"></a>

1. [[BUG] Fields "firstname" and “lastname” accepts invalid data type (Boolean - false) instead String](#001)
2. [[BUG] Floating-point variables in the body request (“totalprice” field ) are rounded to integers in the response](#002)
3. [[BUG] Field "totalprice" accepts invalid data type (String) instead Number and return null](#003)
4. [[BUG] Only Boolean type should be accepted in the 'depositpaid' field. Missing validation](#004)
5. [[BUG] Lack of verification of compliance of check in and check out dates](#005)
6. [[BUG] Incorrect response status for types other than Date for checkin checkout](#006)
7. [[BUG] Check In and checkout take values of the Number type. Incorrect conversion of number to date in 'checkin' and 'checkout' fields](#007)
8. [[BUG] Invalid data type in 'additionalneeds' field - expected text, not numbers](#008)
9. [[BUG] The 'totalprice' field accepts negative values](#009)
10. [[BUG] Incorrect response status when not authorized](#010)
11. [[BUG] Incorrect response status after sending a request with the Number type for firstname field](#011)
12. [[BUG] Attempting to update a deleted resource should return 404 "Not Found" instead of 405 "Method Not Allowed"](#012)

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 1. [BUG] Fields "firstname" and “lastname” accepts invalid data type (Boolean - false) instead String #001 _Priority High | Severity Major_ | [TEST env] <a id="001"></a> ([Back to list](#list))

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
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 2. [BUG] Floating-point variables in the body request (“totalprice” field ) are rounded to integers in the response #002 _Priority High | Severity Major_ | [TEST env] <a id="002"></a> ([Back to list](#list))

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
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>


## 3. [BUG] Field "totalprice" accepts invalid data type (String) instead Number and return null #003 _Priority High | Severity Major_  [TEST env] <a id="003"></a> ([Back to list](#list))

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
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>


## 4. [BUG] Only Boolean type should be accepted in the 'depositpaid' field. Missing validation #004 _Priority High | Severity Major_  [TEST env] <a id="004"></a> ([Back to list](#list))

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
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 5. [BUG] Lack of verification of compliance of check in and check out dates #005 _Priority High | Severity Major_  [TEST env] <a id="005"></a> ([Back to list](#list))

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
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 6. [BUG] Incorrect response status for types other than Date for checkin checkout #006 _Priority High | Severity Major_ [TEST env] <a id="006"></a> ([Back to list](#list))

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
> 3. Use Booking - UpdateBooking request to update data, replace body request “bookingdates” to checkin: String "text" and checkout: "text"

```
    "bookingdates": {
        "checkin": "text",
        "checkout": "text"
}
```

- Actual Result
> 1. Response:

```
    "bookingdates": {
        "checkin": "0NaN-aN-aN",
        "checkout": "0NaN-aN-aN"
    }
```
> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request

```
{
    "error": {
        "code": 400,
        "message": "Invalid date format in 'checkin' or 'checkout' field. Expected format: 'YYYY-MM-DD'."
    }
}
```

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 7. [BUG] Check In and checkout take values of the Number type. Incorrect conversion of number to date in 'checkin' and 'checkout' fields #007 _Priority High | Severity Major_ [TEST env] <a id="007"></a> ([Back to list](#list))

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

> 3. Use Booking - UpdateBooking request to update data, replace body request “bookingdates” to checkin: Number 1 and checkout 1

```
    "bookingdates": {
        "checkin": 1,
        "checkout": 1
}
```
- Actual Result
> 1. Response:

```
  "bookingdates": {
        "checkin": "1970-01-01",
        "checkout": "1970-01-01"
    }
```
> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request

```
{
    "error": {
        "code": 400,
        "message": "Invalid data format in 'checkin' or 'checkout' fields. Expected date in 'YYYY-MM-DD' format."
    }
}
```
> 2. Documentation

![2024-02-15_22h50_48](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/1b816b6e-58be-46d9-8e83-f1dcd3675baa)

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 8. [BUG] Invalid data type in 'additionalneeds' field - expected text, not numbers #008 _Priority High | Severity Major_ [TEST env] <a id="008"></a> ([Back to list](#list))

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

> 3. Use Booking - UpdateBooking request to update data, replace body request value “'additionalneeds” to Number: 1300

```
"additionalneeds": 1333
```

- Actual Result
> 1. Response:

```
"additionalneeds": 1333
```

> 2. Status is 200 OK

- Expected result:
> 1. Status 400 Bad request
> 2. Invalid data type in 'additionalneeds' field - expected text, not numbers or boolean

```
{
    "error": {
        "code": 400,
        "message": "Invalid data type in 'additionalneeds' field. Expected String."
    }
}
```
> Documentation

![2024-02-15_22h58_10](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/65625ac3-2b47-4110-829a-679e03fb0aad)

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 9. [BUG] The 'totalprice' field accepts negative values #009 _Priority High | Severity Major_  [TEST env] <a id="009"></a> ([Back to list](#list))

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

> 3. Use Booking - UpdateBooking request to update data, replace body request “totalprice” to -1000

```
"totalprice": -1000,
```

- Actual Result
> 1. Response:

```
"totalprice": -1000,
```
> 2. Status code is 200

- Expected result:
> 1. Status 400 Bad request
> 2. Field should not take negative values

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 10. [BUG] Incorrect response status when not authorized #010 _Priority High | Severity Major_  [TEST env] <a id="010"></a> ([Back to list](#list))

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

> 3. Uncheck Cookie Headers with token value in Postman and send PUT request

![2024-02-15_23h31_57](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/c7d5385f-8804-4e2c-bf89-4ec5bbe422d2)

- Actual Result
> 1. Status 403 Forbidden

- Expected result:
> 1. Should be Status 403 Forbidden. This error code is commonly used in the context of failure to authenticate.

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 11. [BUG] Incorrect response status after sending a request with the Number type for firstname field #011 _Priority High | Severity Major_ [TEST env] <a id="011"></a> ([Back to list](#list))

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

> 3. Use Booking - UpdateBooking request to update data, replace body request value “firstname” to Number: 122 f.e.

```
"firstname": 122
```

- Actual Result
> 1. Response code is 500 Internal Server Error

![2024-02-15_23h36_51](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/7c7f4652-f50c-4fd9-8a5c-b3ae40b2ce87)

- Expected result:
> 1. Should be Status 400 Bad request.. This error code is commonly used in the context of failure to authenticate.

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

## 12. [BUG] Attempting to update a deleted resource should return 404 "Not Found" instead of 405 "Method Not Allowed" #012 _Priority High | Severity Major_ [TEST env] <a id="012"></a> ([Back to list](#list))

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

> 3. Use Booking - DeleteBooking  request to delete resource
> 4. Use Booking - UpdateBooking  request to update resource


- Actual Result
> Status code is 405 Method Not Allowed

![2024-02-15_23h40_24](https://github.com/DominikCLK/Restful-Booker-API-Tests/assets/75272795/6bd78b94-2cf5-4aec-87e1-f200254036e7)

- Expected result:
> 1. Status 404 Not found

> Recommendations:
> The resource that was deleted does not exist on the server, so trying to update it should result in a 404. A 405 status would mean that the method you are trying to use is not allowed, which is not an ideal choice for trying to update a resource again that has been deleted. deleted.

<p align="center">
    <img src="https://skillicons.dev/icons?i=github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github,postman,github" />
</p>

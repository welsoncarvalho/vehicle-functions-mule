## Vehicle API - with Mule ##

This is a simple API to demonstrate the integration between Mule and AWS Lambda and DynamoDB. The API contains the actions:

- List Vehicles
- Fetch Vehicle
- Create Vehicle
- Update Vehicle

### List Vehicle ###

An HTTP API call using GET method. It's an integration using Amazon DynamoDB Connector. It's a simple scan in DynamoDB table using Transform Message in Mule to convert the JSON.

<p align="center">
    <img src="https://github.com/welsoncarvalho/vehicle-functions-mule/blob/main/images/vehicle-list.png" alt="Vehicle List" style="width: 50%;">
</p>

Request: GET http://localhost:8080/vehicle

Response:
```json
{
  "count": 1,
  "lastKey": null,
  "total": 1,
  "items": [
    {
      "brand": "vw",
      "model": "polo",
      "plate": "gtw-6354",
      "detail": "/vehicle/f7137545-13e4-43af-a94c-a85fa78a5a5f"
    }
  ]
}
```

### Fetch Vehicle ###

An HTTP API call using GET method with the vehicle identification as path parameter. Integration using Amazon Lambda Connector that returns all vehicle data.

<p align="center">
    <img src="https://github.com/welsoncarvalho/vehicle-functions-mule/blob/main/images/vehicle-fetch.png" alt="Vehicle Fetch" style="width: 50%;">
</p>

Request: GET http://localhost:8080/vehicle/{id}

Response:
```json
{
"id": "f7137545-13e4-43af-a94c-a85fa78a5a5f",
"brand": "vw",
"model": "polo",
"year": 2019,
"plate": "gtw-6354",
"createdAt": "2021-11-12T12:04:30.065637"
}
```

### Create Vehicle ###

An HTTP API call using POST method with vehicle data in payload. It's an integration using Amazon Lambda Connector that sends all vehicle data and returns only the id generated.

<p align="center">
    <img src="https://github.com/welsoncarvalho/vehicle-functions-mule/blob/main/images/vehicle-create.png" alt="Vehicle Create" style="width: 50%;">
</p>

Request: POST http://localhost:8080/vehicle
```json
{
  "brand": "vw",
  "model": "polo",
  "year": 2019,
  "plate": "gtw-6354"
}
```
Response:
```json
{
  "id": "f7137545-13e4-43af-a94c-a85fa78a5a5f"
}
```

### Update Vehicle ###

An HTTP API call using PUT method with vehicle data to be updated. It's an integration using Amazon Lambda Connector that sends all vehicle data and returns only the HTTP status code 204.

<p align="center">
    <img src="https://github.com/welsoncarvalho/vehicle-functions-mule/blob/main/images/vehicle-update.png" alt="Vehicle Update" style="width: 50%;">
</p>

Request: PUT http://localhost:8080/vehicle/{id}
```json
{
  "brand": "vw",
  "model": "polo",
  "year": 2019,
  "plate": "gtw-6354"
}
```
Response: HTTP status code 204

---
#### Setup to Run ####

To run the project, follow the steps:

1. Create an access key and secret key in AWS console to be configured in vehicle.xml for Lambda Connector and DynamoDB Connector.
2. Create a DynamoDB table called **vehicle**
3. Create 3 functions (these lambdas must have permission to access the vehicle table in DynamoDB):
   1. **vehicle-create** - to create vehicle
   2. **vehicle-update** - to update vehicle
   3. **vehicle-fetch** - to fetch vehicle

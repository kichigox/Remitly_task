# SWIFT Codes API

A simple API to store and retrieve SWIFT codes of banks using Python Flask and SQLite - all run from Jupyter Notebook.

This API allows you to:
- Add a SWIFT code
- Get a SWIFT code by its code or by country
- Delete a SWIFT code

## How to Run

1. Install Flask (if needed)
   
   In a Jupyter notebook cell, run:
   
   !pip install flask

2. Import and Run the Flask App
   
  In another Jupyter notebook cell, set up and run the Flask app like this:

  Example for DELETE API:
  
``` python
from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

@app.route('/v1/swift-codes/<string:swift_code>', methods=['DELETE'])
def delete_swift(swift_code):
   con = sqlite3.connect("swift_data.db")
   cursor = con.cursor()
   cursor.execute('DELETE FROM swift_codes WHERE swift_code = ?', (swift_code,))
   con.commit()
   return jsonify({'message': 'SWIFT code deleted successfully'})
    
if __name__ == "__main__":
    app.run(host="localhost", port=8080)
 ```
   
Once the server is running in a cell, you can access it from: http://localhost:8080


## API Endpoints

### Get by SWIFT Code
GET /v1/swift-codes/{swift-code}

Body:

```python
{
    "address": string,
    "bankName": string,
    "countryISO2": string,
    "countryName": string,
    "isHeadquarter": bool,
    "swiftCode": string
    "branches": [
          {
                "address": string,
                "bankName": string,
                "countryISO2": string,
                "isHeadquarter": bool,
                "swiftCode": string
          },
          {
                "address": string,
                "bankName": string,
                "countryISO2": string,
                "isHeadquarter": bool,
                "swiftCode": string
          }, . . .
      ]
}
```
### Get by Country
GET /v1/swift-codes/country/{countryISO2code}

Body:

```python
{
    "countryISO2": string,
    "countryName": string,
    "swiftCodes": [
        {
                "address": string,
                "bankName": string,
                "countryISO2": string,
                "isHeadquarter": bool,
                "swiftCode": string
          },
          {
                "address": string,
                "bankName": string,
                "countryISO2": string,
                "isHeadquarter": bool,
                "swiftCode": string
          }, . . .
      ]
}
```
### Add SWIFT Code
POST /v1/swift-codes

Body:

``` python
{
    "address": string,
    "bankName": string,
    "countryISO2": string,
    "countryName": string,
    "isHeadquarter": bool,
    "swiftCode": string,
}
```

### Delete SWIFT Code
DELETE /v1/swift-codes/{swift-code}

## Running Tests (Jupyter Notebook)
In a Jupyter cell, add your test logic like this:

```python
from app import app  
client = app.test_client()

response = client.post("/v1/swift-codes", json={...})
print("POST status:", response.status_code)
print("Response:", response.json)
```

## Response Format

Success:

```python
{
  "data": {...},
  "message": "Success",
  "error": null
}
```

Error:

```python
{
  "data": null,
  "message": "Something went wrong",
  "error": "Error message here"
}
```

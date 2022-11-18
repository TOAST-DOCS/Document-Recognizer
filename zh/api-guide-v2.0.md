## AI Service > Document Recognizer > API v2.0 Guide

### Overview of v2.0 API

#### Changes from v1.0

1. Enhanced security with the electronic envelope method.

#### Domain

| Name | Domain |
| --- | --- |
| OCR Public API Domain | [https://ocr.api.nhncloudservice.com](https://ocr.api.nhncloudservice.com) |

#### Prerequisites (AppKey, SecretKey)

* AppKey and SecretKey are required to use the API.
* You can find the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

#### Caution

* Check whether the request or response is Base64 encoded.
* Check the detailed mode of encryption and decryption (eg AES-256/CBC/PKCS7Padding).
* The symmetric key used for encryption must be generated as a 32-byte random number.

### Issue Public Key

#### Request

* You can find the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
|-----| --- |
| GET | /v2.0/appkeys/{appKey}/public-keys/{serviceName} |

[Request Header]

| Name | Value | Description              |
| --- | --- |-----------------|
| Authorization | {secretKey} | Security key issued from the console |

[Path Variable]

| Name | Value | Description              |
| --- | --- |-----------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |
| serviceName | {serviceName} | credit-card (when issuing the public key used for calling the credit card API),<br> id-card (when issuing the public key used for calling the ID card API)  |

[Request Body]

```
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'Authorization: ${secretKey}'
```

#### Response

[Response Body]

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "key": "String",
        "version": "0"
     }
}
```

[Header]

| Name | Type | Description |
| --- | --- | --- |
| isSuccessful | Boolean | API success or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error content on failure) |

[Field]

| Name | Type | Description                 |
| --- | --- |--------------------|
| result | Object | Public key required for encryption      |
| result.key | String | Public key (Base64 encoded) |
| result.version | String | version of public key           |

* The public key is **Base64** encoded.

### Credit Card API

#### Credit Card Analysis API

#### Request

* You can find the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/credit-card |

[Request Header]

| Name | Value | Description                    |
| --- | --- |-----------------------|
| Authorization | {secretKey} | Security key issued from the console       |
| X-Key-Version | {x-key-version} | Version of the public key issued        |
| Symmetric-Key | {symmetricKey} | Symmetric key encrypted with the issued public key |

* {symmetricKey} must be created as a 32- **byte random number**.
* {symmetricKey} must be encrypted with the**RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name | Value | Description              |
| --- | --- |-----------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name | Type | Description | Encryption Description         |
| --- | --- | --- |----------------|
| image | multipart/form-data | Image file | Image encrypted with a symmetric key |

* Image files must be encrypted with the**AES-256/CBC/PKCS7Padding** method (using a symmetric key).

[Request Body]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### Response

[Response Body]

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "fileType": "png",
        "resolution": "low",
        "cardNums": [
                    {
                        "value": "1111",
                        "conf": 0.87
                    },
                    {
                        "value": "2222",
                        "conf": 0.99
                    },
                    {
                        "value": "3333",
                        "conf": 0.97
                    },
                    {
                        "value": "4444",
                        "conf": 0.89
                    }
        ],
        "totalCardNum": "111222233334444",
        "cardNumBoxes": [
            {
                "x1": 62,
                "y1": 256,
                "x2": 192,
                "y2": 256,
                "x3": 192,
                "y3": 301,
                "x4": 62,
                "y4": 301
            },
            ...
        ],
        "validThru": {
            "value": "04/19",
            "conf": 0.53
        },
        "validThruBox": {
            "x1": 316,
            "y1": 315,
            "x2": 426,
            "y2": 315,
            "x3": 426,
            "y3": 347,
            "x4": 316,
            "y4": 347
        }
    }
}
```

[Header]

| Name | Type | Description |
| --- | --- | --- |
| isSuccessful | Boolean | Analysis API success or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error content on failure) |

[Field]

| Name | Type | Description | Whether encrypted or not |
| --- | --- | --- | --- |
| fileType | String | File extension (.jpg, .png) |  |
| resolution | String | normal: the resolution is the recommended resolution (760\*480px) or above, low: the resolution is below the recommended resolution |  |
| cardNums | List | List of card number recognition results |  |
| cardNums[0].value | String | Recognition result | O |
| cardNums[0].conf | Double | Confidence of the recognition result |  |
| totalCardNum | List | Full card number recognition result | O |
| cardNumBoxes | List | List of coordinates of the card number recognition area (bounding box) |  |
| cardNumBoxes[0] | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 } |  |
| validThru.value | String | Expiration date recognition content | O |
| validThru.conf | Double | Confidence of expiration date recognition result |  |
| validThruBox | Object | Coordinates of the expiration date recognition area { x1, y1, x2, y2, x3, y3, x4, y4 } |  |

* Encrypted items (cardNums[0].value, totalCardNum, etc.) are encrypted with the **AES-256/CBC/PKCS7Padding** method (using symmetric key).

* boxes[0]
![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### ID Card Analysis API

#### Request

* You can find the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card |

[Request Header]

| Name | Value | Description |
| --- | --- | --- |
| Authorization | {secretKey} | Security key issued from the console |
| X-Key-Version | {x-key-version} | Version of the public key issued |
| Symmetric-Key | {symmetricKey} | Symmetric key encrypted with the issued public key |

* {symmetricKey} must be created as a 32- **byte random number**.
* {symmetricKey} must be encrypted with the**RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name | Value | Description              |
| --- | --- |-----------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name | Type | Description | Encryption Description |
| --- | --- | --- | --- |
| image | multipart/form-data | Image file | Image encrypted with a symmetric key |

* Image files must be encrypted with the**AES-256/CBC/PKCS7Padding** method (using a symmetric key).

[Request Body]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### Response

[Response Header]

| Name | Description |
| --- | --- |
| Request-Key | Request-Key to be used when calling the Verify Authenticity API |

* **If you use the Request-Key to make a Authenticity API call and get a normal response, the Request-Key used cannot be reused.**
* **Request-Key is valid for 1 hour after issuance and cannot be used after that.**

[Response Body]

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "fileType": "png",
        "resolution": "low",
        "idType": "resident",
        "keyValues": [
            {
                "key": "name",
                "value": "String",
                "conf": 0.67
            },
            {
                "key": "residentNumber",
                "value": "String",
                "conf": 0.91
            },
            {
                "key": "issueDate",
                "value": "String",
                "conf": 0.86
            },
            {
                "key": "issuer",
                "value": "String",
                "conf": 0.8
            }
        ],
        "boxes": [
            {
                "x1": 280,
                "y1": 271,
                "x2": 354,
                "y2": 271,
                "x3": 354,
                "y3": 305,
                "x4": 280,
                "y4": 305
            },
            ...
        ]
    }
}
```

[Header]

| Name | Type | Description |
| --- | --- | --- |
| isSuccessful | Boolean | Analysis API success or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error content on failure) |

[Field]

| Name | Type | Description | Whether encrypted or not |
| --- | --- | --- | --- |
| fileType | String | File extension (.jpg, .png) |  |
| resolution | String | normal: the resolution is the recommended resolution (760\*480px) or above, low: the resolution is below the recommended resolution |  |
| idType | String | resident(resident registration certificate), driver(driver license) |  |
| keyValues | List |  |  |
| keyValues[0].key | String |  |  |
| keyValues[0].value | String |  | O |
| keyValues[0].conf | Double |  |  |
| boxes | List | List of bounding box coordinates |
| boxes[0] | Object  | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 } |

* **List included in KeyValues when "idType" is recognized as "resident"**

| key | value type | description |
| --- | --- | --- |
| **name** | string | Recognized name |
| **residentNumber** | string | Recognized resident registration number |
| **issueDate** | string | Recognized issued date |
| **issuer** | string | Recognized issuer |

* **List to be included in KeyValues when "idType" is recognized as "driver"**

| key | value type | description |
| --- | --- | --- | 
| **driverLicenseNumber** | string | Recognized driver license number |
| **licenseType** | string | Recognized driver license type (Class 1 Normal, etc.)<br>When the values are 2 or more, separate them with “/” |
| **name** | string | Recognized name |
| **residentNumber** | string | Recognized resident registration number |
| **condition** | string | Recognized driver license condition<br>(If the field does not exist according to the driver's license, the value of the field is none) |
| **serialNum** | string | Recognized serial number |
| **issueDate** | string | Recognized issued date |
| **issuer** | string | Recognized issuer |

* Encrypted items (keyValues[0].value, etc.) are encrypted with the **AES-256/CBC/PKCS7Padding** method (using symmetric key).
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### Verify Authenticity API

#### Request

* You can find the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card/authenticity |

[Request Header]

| Name | Value | Description |
| --- | --- | --- |
| Authorization | {secretKey} | Security key issued from the console |
| X-Key-Version | {x-key-version} | Version of the public key issued |
| Symmetric-Key | {symmetricKey} | Symmetric key encrypted with the issued public key |
| Request-Key | {Request-Key} | Request-Key issued after ID card analysis |

* {symmetricKey} must be created as a 32- **byte random number**.
* {symmetricKey} must be encrypted with the**RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name | Value | Description              |
| --- | --- |-----------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name | Type | Description | idType | Whether encrypted or not |
| --- | --- | --- | --- | --- |
| idType | String | resident(resident registration certificate), driver(driver license) |  | X |
| name | String | Name |  | O |
| residentNumber | String | Resident registration number<br>- For resident (resident registration certificate), 13 digits of resident registration number<br>- For a driver (driver's license), 7 digits that comprise of the first 6 digits and the first 1 digit of  resident registration number |  | O |
| issueDate | String | Issued date f resident registration certificate (YYYYMMDD) | resident | O |
| driverLicenseNumber | String | 12 digits of driver license number | driver | O |
| serialNum | String | 5 and 6 digits of serial number | driver | O |

* A field that requires encryption must be encrypted with the**AES-256/CBC/PKCS7Padding** method (using a symmetric key).

[Request Body]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card/authenticity' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}' \
-H 'Request-Key: ${Request-Key}' \
-H 'Content-Type: application/json' \
--data-raw '{
  "idType": "driver",
  "name": "홍길동",
  "residentNumber": "8001011",
  "driverLicenseNumber": "112233333344",
  "serialNum": "A1B2C3"
}'
```

#### Response

[Response Body]

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "isAuthenticity": false
    }
}
```

[Header]

| Name | Type | Description |
| --- | --- | --- |
| isSuccessful | Boolean | Whether the Verify Authenticity API succeeds or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error content on failure) |

[Field]

| Name | Type | Description |
| --- | --- | --- |
| isAuthenticity | Boolean | Whether it is authentic or not |
## AI Service > Document Recognizer > API Guide

### Business Registration Certificate Analysis API

#### Request

- You can check the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
|---|---|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business |

[Request Header]

| Name | Value | Description |
|---|---|---|
| Authorization | {secretKey} | Security key issued from the console |

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[Request Body]

- Put binary data of the image file.

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}'
```

[Field]

| Name | Type | Description |
|---|---|---|
| image | multipart/form-data | Image file |

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
        "unitType": "pixel",
        "keyValues": [
            {
                "key":"구분",
                "value":" 간이과세자",
                "conf":0.93
            },
            {
                "key":"등록번호",
                "value":"123-45-67890",
                "conf":1
            },
            ...
        ],
        "boxes": [
            {
                "x1": 340,
                "y1": 3231,
                "x2": 523,
                "y2": 3231,
                "x3": 523,
                "y3": 3297,
                "x4": 340,
                "y4": 3297
            },
            ...
        ],
        "resolution": "normal"
    }
}
```

[Header]

| Name | Type | Description |
|---|---|---|
| isSuccessful | Boolean | Analysis API success or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error details on failure) |

[Field]

| Name | Type | Description |
|---|---|---|
| fileType | String | File extension (.pdf, .jpg, .png) |
| keyValues | List | List of recognition results |
| keyValues[0].key | String | Recognized item name |
| keyValues[0].value | String | Recognized content |
| keyValues[0].conf | Double | Confidence of the recognition result |
| resolution | String | normal: the resolution is the recommended resolution (HD 1280*720px) or above, low: the resolution is below the recommended resolution |
| unitType | String | Coordinate unit for boxes (pixel by default, point for PDF) |
| boxes | List | List of recognized area (bounding box) coordinates |
| boxes[0] | Object  | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 } |

* boxes[0]

    ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### Credit Card Analysis API

#### Request

- You can check the {appKey} and {secretKey} in the **URL & Appkey** menu at the top of the console.

[URI]

| Method | URI |
|---|---|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card |

[Request Header]

| Name | Value | Description |
|---|---|---|
| Authorization | {secretKey} | Security key issued from the console |

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[Request Body]

- Put binary data of the image file.

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}'
```

[Field]

| Name | Type | Description |
|---|---|---|
| image | multipart/form–data | Image file |

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
                    },
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
}
```

[Header]

| Name | Type | Description |
|---|---|---|
| isSuccessful | Boolean | Analysis API success or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (success on success, error details on failure) |

[Field]

| Name | Type | Description |
|---|---|---|
| fileType | String | File extension (.jpg, .png) |
| resolution | String | normal: the resolution is the recommended resolution (760*480px) or above, low: the resolution is below the recommended resolution |
| cardNums | List | List of card number recognition results |
| cardNums[0].value | String | Recognition result |
| cardNums[0].conf | Double | Confidence of the recognition result |
| totalCardNum | List | Full card number recognition result |
| cardNumBoxes | List | List of coordinates of the card number recognition area (bounding box) |
| cardNumBoxes[0] | Object  | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 } |
| validThru.value | String | Expiration date recognition content |
| validThru.conf | Double | Confidence of expiration date recognition result |
| validThruBox | Object  | Coordinates of the expiration date recognition area { x1, y1, x2, y2, x3, y3, x4, y4 } |

* boxes[0]

    ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)


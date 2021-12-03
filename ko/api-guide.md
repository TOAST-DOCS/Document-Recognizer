## AI Service > Document Recognizer > API 가이드

### 사업자등록증 분석 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business |

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |

[요청 본문]

- 이미지 파일의 Binary Data를 넣습니다.

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business' \
-F 'image=@sample.png' 
-H 'Authorization: ${secretKey}'
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| image | multipart/form–data | 이미지 파일 |

#### 응답

[응답 본문]

```
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

[헤더]

| 이름 | 타입 | 설명 |
|---|---|---|
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| fileType | String | 파일 확장자(.pdf, .jpg, .png) |
| keyValues | List | 인식 결과 목록 |
| keyValues[0].key | String | 인식 항목명 |
| keyValues[0].value | String | 인식 내용 |
| keyValues[0].conf | Double | 인식 결과 신뢰도 |
| resolution | String | 권장 해상도(HD 1280*720px) 이상이면 normal, 권장 해상도 미만은 low |
| unitType | String | boxes 좌표 단위(기본 pixel, PDF의 경우 point) |
| boxes | List | 인식 영역(Bounding box) 좌표 목록 |
| boxes[0] | Object  | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |

* boxes[0]
 
    ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### 신용카드 분석 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card |

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |

[요청 본문]

- 이미지 파일의 Binary Data를 넣습니다.

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' 
-H 'Authorization: ${secretKey}'
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| image | multipart/form–data | 이미지 파일 |

#### 응답

[응답 본문]

```
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

[헤더]

| 이름 | 타입 | 설명 |
|---|---|---|
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| fileType | String | 파일 확장자(.jpg, .png) |
| resolution | String | 권장 해상도(HD 760*480px) 이상이면 normal, 권장 해상도 미만은 low |
| cardNums | List | 카드 번호 인식 결과 목록 |
| cardNums[0].value | String | 인식 결과 |
| cardNums[0].conf | Double | 인식 결과 신뢰도 |
| totalCardNum | List | 카드 번호 전체 인식 결과 |
| cardNumBoxes | List | 카드 번호 인식 영역(Bounding box) 좌표 목록 |
| cardNumBoxes[0] | Object  | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |
| validThru.value | String | 유효 기간 인식 내용 |
| validThru.conf | Double | 유효 기간 인식 결과 신뢰도 |
| validThruBox | Object  | 유효 기간 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |

* boxes[0]
 
    ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)


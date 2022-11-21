## AI Service > Document Recognizer > API v2.0 가이드

### v2.0 API 소개

#### v1.0과 달라진 점

1. 전자 봉투 방식으로 보안이 강화되었습니다.

#### 도메인

| 이름 | 도메인 |
| --- | --- |
| OCR Public API 도메인 | [https://ocr.api.nhncloudservice.com](https://ocr.api.nhncloudservice.com) |

#### 사전 준비(AppKey, SecretKey)

* API를 사용하려면 AppKey와 SecretKey가 필요합니다.
* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인할 수 있습니다.

#### 주의 사항

* 요청, 응답 시 Base64 인코딩 여부를 확인하십시오.
* 암호화, 복호화의 상세 모드(예: AES-256/CBC/PKCS7Padding)를 확인하십시오.
* 암호화에 사용되는 대칭 키는 반드시 32byte 난수로 생성합니다.

### 공개 키 발급

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인할 수 있습니다.

[URI]

| 메서드 | URI |
|-----| --- |
| GET | /v2.0/appkeys/{appKey}/public-keys/{serviceName} |

[요청 헤더]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |
| serviceName | {serviceName} | credit-card(신용카드 API 호출 시 사용할 공개 키 발급 시),<br> id-card(신분증 API 호출 시 사용할 공개 키 발급 시)  |

[요청 본문]

```
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'Authorization: ${secretKey}'
```

#### 응답

[응답 본문]

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

[헤더]

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| isSuccessful | Boolean | API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명                 |
| --- | --- |--------------------|
| result | Object | 암호화에 필요한 공개 키      |
| result.key | String | 공개 키(Base64로 인코딩됨) |
| result.version | String | 공개 키의 버전           |

* 공개 키는 **Base64**로 인코딩된 상태입니다.

### 신용카드 API

#### 신용카드 분석 API

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인할 수 있습니다.

[URI]

| 메서드 | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/credit-card |

[요청 헤더]

| 이름 | 값 | 설명                    |
| --- | --- |-----------------------|
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키       |
| X-Key-Version | {x-key-version} | 발급 받은 공개 키의 버전        |
| Symmetric-Key | {symmetricKey} | 발급 받은 공개 키로 암호화된 대칭 키 |

* {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
* {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름 | 타입 | 설명 | 암호화 설명         |
| --- | --- | --- |----------------|
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

* 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).

[요청 본문]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### 응답

[응답 본문]

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

[헤더]

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 | 암호화 여부 |
| --- | --- | --- | --- |
| fileType | String | 파일 확장자(.jpg, .png) |  |
| resolution | String | 권장 해상도(760\*480px) 이상이면 normal, 권장 해상도 미만은 low |  |
| cardNums | List | 카드 번호 인식 결과 목록 |  |
| cardNums[0].value | String | 인식 결과 | O |
| cardNums[0].conf | Double | 인식 결과 신뢰도 |  |
| totalCardNum | List | 카드 번호 전체 인식 결과 | O |
| cardNumBoxes | List | 카드 번호 인식 영역(Bounding box) 좌표 목록 |  |
| cardNumBoxes[0] | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |  |
| validThru.value | String | 유효 기간 인식 내용 | O |
| validThru.conf | Double | 유효 기간 인식 결과 신뢰도 |  |
| validThruBox | Object | 유효 기간 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |  |

* 암호화된 항목들(cardNums[0].value, totalCardNum등)은 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어 있습니다(대칭 키 이용).

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### 신분증 분석 API

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL &amp; Appkey** 메뉴에서 확인할 수 있습니다.

[URI]

| 메서드 | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card |

[요청 헤더]

| 이름 | 값 | 설명 |
| --- | --- | --- |
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |
| X-Key-Version | {x-key-version} | 발급 받은 공개 키의 버전 |
| Symmetric-Key | {symmetricKey} | 발급 받은 공개 키로 암호화된 대칭 키 |

* {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
* {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름 | 타입 | 설명 | 암호화 설명 |
| --- | --- | --- | --- |
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

* 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).

[요청 본문]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### 응답

[응답 헤더]

| 이름 | 설명 |
| --- | --- |
| Request-Key | 신분증 진위 확인 API 호출 시 사용할 Request-Key |

* **Request-Key를 진위 확인 API 호출에 사용하여 정상 응답을 받은 경우 사용된 Request-Key는 다시 사용할 수 없습니다.**
* **Request-Key는 발급 이후 1시간 동안 유효하며 그 이후에는 사용할 수 없습니다.**

[응답 본문]

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

[헤더]

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 | 암호화 여부 |
| --- | --- | --- | --- |
| fileType | String | 파일 확장자(.jpg, .png) |  |
| resolution | String | 권장 해상도(760\*480px) 이상이면 normal, 권장 해상도 미만은 low |  |
| idType | String | resident(주민등록증), driver(운전면허증) |  |
| keyValues | List |  |  |
| keyValues[0].key | String |  |  |
| keyValues[0].value | String |  | O |
| keyValues[0].conf | Double |  |  |
| boxes | List | 인식 영역(Bounding box) 좌표 목록 |
| boxes[0] | Object  | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |

* **"idType"이 "resident"로 인식될 경우 KeyValues에 포함되는 목록**

| key | value type | description |
| --- | --- | --- |
| **name** | string | 인식된 이름 |
| **residentNumber** | string | 인식된 주민등록번호 |
| **issueDate** | string | 인식된 발급 일자 |
| **issuer** | string | 인식된 발급기관 |

* **"idType"이 "driver"로 인식될 경우 KeyValues에 포함되는 목록**

| key | value type | description |
| --- | --- | --- | 
| **driverLicenseNumber** | string | 인식된 운전면허번호 |
| **licenseType** | string | 인식된 면허 종류(1종 보통 등)<br>2개 이상일 경우 문자열 내 "/"로 구분 |
| **name** | string | 인식된 이름 |
| **residentNumber** | string | 인식된 주민등록번호 |
| **condition** | string | 인식된 면허 조건<br>(운전면허증에 따라 해당 필드가 존재하지 않는 경우 해당 필드의 value는 none) |
| **serialNum** | string | 인식된 암호 일련번호 |
| **issueDate** | string | 인식된 발급 일자 |
| **issuer** | string | 인식된 발급기관 |

* 암호화된 항목들(keyValues[0].value 등)은 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어 있습니다(대칭 키 이용).
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### 신분증 진위 확인 API

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL &amp; Appkey** 메뉴에서 확인할 수 있습니다.

[URI]

| 메서드 | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card/authenticity |

[요청 헤더]

| 이름 | 값 | 설명 |
| --- | --- | --- |
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |
| X-Key-Version | {x-key-version} | 발급 받은 공개 키의 버전 |
| Symmetric-Key | {symmetricKey} | 발급 받은 공개 키로 암호화된 대칭 키 |
| Request-Key | {Request-Key} | 신분증 분석 후 발급받은 Request-Key |

* {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
* {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름 | 타입 | 설명 | idType | 암호화 여부 |
| --- | --- | --- | --- | --- |
| idType | String | resident(주민등록증), driver(운전면허증) |  | X |
| name | String | 이름 |  | O |
| residentNumber | String | 주민등록번호<br>- resident(주민등록증)의 경우 주민등록번호 숫자 13자리<br>- driver(운전면허증)의 경우 주민등록번호 앞 6자리와 뒤 첫 번째 1자리를 조합한 숫자 7자리 |  | O |
| issueDate | String | 주민등록증 발급 일자(YYYYMMDD) | resident | O |
| driverLicenseNumber | String | 12자리 운전면허번호 | driver | O |
| serialNum | String | 5~6자리 암호 일련번호 | driver | O |

* 암호화가 필요한 필드는 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).

[요청 본문]

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

#### 응답

[응답 본문]

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "isAuthenticity":false
    }
}
```

[헤더]

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| isSuccessful | Boolean | 진위 확인 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| isAuthenticity | Boolean | 진위 여부 |
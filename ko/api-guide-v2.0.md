## AI Service > Document Recognizer > API v2.0 가이드

## v2.0 API 소개

### v1.0과 달라진 점

1. 전자 봉투 방식으로 보안이 강화되었습니다.

### 도메인

| 이름 | 도메인 |
| --- | --- |
| OCR Public API 도메인 | [https://ocr.api.nhncloudservice.com](https://ocr.api.nhncloudservice.com) |

### 사전 준비(AppKey, SecretKey)

* API를 사용하려면 AppKey와 SecretKey가 필요합니다.
* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인할 수 있습니다.

### 주의 사항

* 요청, 응답 시 Base64 인코딩 여부를 확인하십시오.
* 암호화, 복호화의 상세 모드(예: AES-256/CBC/PKCS7Padding)를 확인하십시오.
* 암호화에 사용되는 대칭 키는 반드시 32bit 난수로 생성합니다.

## 공개 키 발급

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인할 수 있습니다.

[URI]

| 메서드 | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/public-keys/credit-card |

[요청 헤더]

| 이름 | 값 | 설명              |
| --- | --- |-----------------|
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |

[요청 본문]

* 이미지 파일의 바이너리 데이터를 입력합니다.

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/credit-card' \
-H 'Authorization: ${secretKey}'
```

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
        "key": "String",
        "version": "1.0"
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

## 신용카드 API

### 신용카드 분석 API

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

* {symmetricKey}는 반드시 **32bit 난수**로 생성해야 합니다.
* {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[필드]

| 이름 | 타입 | 설명 | 암호화 설명         |
| --- | --- | --- |----------------|
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

* 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).
* 대칭 키로 암호화된 이미지 파일의 바이너리 데이터를 **Base64**로 인코딩하여 입력합니다.

[요청 본문]

```
curl -X POST '[https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card](https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card)' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

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
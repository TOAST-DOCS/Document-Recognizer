## AI Service > Document Recognizer > 콘솔 사용 가이드

콘솔을 통해 사업자등록증 파일을 올리고 분석 결과를 얻을 수 있습니다. 


## 사업자등록증 분석


### 분석을 위한 사진 업로드

분석할 사업자등록증 이미지를 업로드합니다.

- 이미지는 다음 2가지 방법으로 업로드할 수 있습니다.
    1. **이미지 업로드** 버튼 클릭
    2. 이미지 드래그 앤드 드롭

### 분석

사진을 업로드한 후 **분석** 버튼을 클릭하면 분석 결과가 화면 오른쪽에 나타납니다.

![Business Registration](http://static.toastoven.net/prod_document_ocr/business_ocr_console_ko.png)

* [텍스트(Key Value)] 분석된 사업자등록증의 내용을 Key/Value 형태로 표시합니다.
* [JSON] 분석한 결과를 JSON 형태로 표시합니다.
    * [success] 분석 성공/실패 여부
    * [fileType] 파일 확장자(.pdf, .jpg, .png)
    * [keyValues] 분석 결과
        * [key] 사업자등록증 내 key에 해당하는 값(구분, 상호명, 주소 등)
        * [value] 특정 key에 해당하는 값
        * [conf] 분석 결과에 대한 신뢰도
    * [resolution] 권장 해상도(HD 1280*720px) 이상이면 normal, 권장 해상도 미만은 low
    * [unitType] boxes 좌표 단위(기본 pixel, PDF의 경우 point)
    * [boxes] 인식 영역에 대한 이미지 상의 좌표값(box 별 {x1, y1, x2, y2, x3, y3, x4, y4} 형태)
![bbox](http://static.toastoven.net/prod_document_ocr/bbox.png)
    * [JSON Sample]
    ```
    {
            "success": true,
            "resultMessage": "success",
            "fileType": "jpg",
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
    ```
  
* 분석 결과 복사 및 다운로드(Excel, JSON) 기능을 제공합니다.


## 신용카드 분석


### 분석을 위한 사진 업로드

분석할 신용카드 이미지를 업로드합니다.

- 이미지는 다음 2가지 방법으로 업로드할 수 있습니다.
    1. **이미지 업로드** 버튼 클릭
    2. 이미지 드래그 앤드 드롭

### 분석

사진을 업로드한 후 **분석** 버튼을 클릭하면 분석 결과가 화면 오른쪽에 나타납니다.

![Credit Card](http://static.toastoven.net/prod_document_ocr/credit_card_ocr_console_ko.png)

* [텍스트(Key Value)] 분석된 신용카드의 내용을 Key/Value 형태로 표시합니다.
* [JSON] 분석한 결과를 JSON 형태로 표시합니다.
    * [fileType] 파일 확장자(.jpg, .png)
    * [resolution] 권장 해상도(760*480px) 이상이면 normal, 권장 해상도 미만은 low
    * [cardNums] 카드 번호 분석 결과 목록
        * [value] 카드 번호 인식 결과
        * [conf] 카드 번호 인식 결과 신뢰도
    * [totalCardNum] 카드 번호 전체 인식 결과
    * [validThru] 유효 기간 인식 내용
        * [value] 유효 기간 인식 결과
        * [conf] 유효 기간 인식 결과 신뢰도
    * [cardNumBoxes] 카드 번호 인식 영역에 대한 이미지 상의 좌표값(box 별 {x1, y1, x2, y2, x3, y3, x4, y4} 형태)
    * [validThruBox] 유효 기간 인식 영역에 대한 이미지 상의 좌표값
![bbox](http://static.toastoven.net/prod_document_ocr/bbox.png)
    * [JSON Sample]
    ```
    {
      "fileType": "jpg",
      "resolution": "low",
      "cardNums": [
        {
          "value": "1111",
          "conf": 0.67
        },
        {
          "value": "2222",
          "conf": 0.66
        },
        {
          "value": "3333",
          "conf": 0.67
        },
        {
          "value": "4444",
          "conf": 0.34
        }
      ],
      "totalCardNum": "4330288628448618",
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
    ```
  
* 분석 결과 복사 및 다운로드(JSON) 기능을 제공합니다.
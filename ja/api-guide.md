## AI Service > Document Recognizer > APIガイド

### 事業者登録証分析API

#### リクエスト

- {appKey}と{secretKey}はコンソール上部の**URL & Appkey**メニューで確認が可能です。

[URI]

| メソッド | URI |
|---|---|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
|---|---|---|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[リクエスト本文]

- 画像ファイルのBinary Dataを入れます。

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/business' \
-F 'image=@sample.png' 
-H 'Authorization: ${secretKey}'
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| image | multipart/form–data | 画像ファイル |

#### レスポンス

[レスポンス本文]

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
                "key":"区分",
                "value":"簡易課税者",
                "conf":0.93
            },
            {
                "key":"登録番号",
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

[ヘッダ]

| 名前 | タイプ | 説明 |
|---|---|---|
| isSuccessful | Boolean | 分析API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| fileType | String | ファイル拡張子(.pdf、.jpg、.png) |
| keyValues | List | 認識結果リスト |
| keyValues[0].key | String | 認識項目名 |
| keyValues[0].value | String | 認識内容 |
| keyValues[0].conf | Double | 認識結果の信頼度 |
| resolution | String | 推奨解像度(HD 1280*720px)以上の場合はnormal、推奨解像度未満はlow |
| unitType | String | boxes座標単位(基本pixel、PDFの場合point) |
| boxes | List | 認識領域(Bounding box)座標リスト |
| boxes[0] | Object  | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |

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
| resolution | String | 권장 해상도(760*480px) 이상이면 normal, 권장 해상도 미만은 low |
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


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
                "value":"270-06-01309",
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

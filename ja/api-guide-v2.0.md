## AI Service > Document Recognizer > API v2.0ガイド

### v2.0 API紹介

#### v1.0と違なる点

1. セキュリティがデジタルエンベロープ方式に強化されました。

#### ドメイン

| 名前 | ドメイン |
| --- | --- |
| OCR Public APIドメイン | [https://ocr.api.nhncloudservice.com](https://ocr.api.nhncloudservice.com) |

#### 事前準備(AppKey, SecretKey)

* APIを使用するにはAppKeyとSecretKeyが必要です。
* {appKey}と{secretKey}はコンソール上部の**URL & Appkey** メニューで確認できます。

#### 注意事項

* リクエスト、レスポンス時にBase64エンコードされているかどうかを確認してください。
* 暗号化、復号の詳細モード(例：AES-256/CBC/PKCS7Padding)を確認してください。
* 暗号化に使用される対称鍵は、必ず32byte乱数で作成します。

### 公開鍵の発行

#### リクエスト

* {appKey}と{secretKey}はコンソール上部の**URL & Appkey** メニューで確認できます。

[URI]

| メソッド | URI |
| -- | --- |
| GET | /v2.0/appkeys/{appKey}/public-keys/credit-card |

[リクエストヘッダ]

| 名前 | 値 | 説明            |
| --- | --- |-----------------|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[リクエスト本文]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/credit-card' \
-H 'Authorization: ${secretKey}'
```

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
        "key": "String",
        "version": "1.0"
     }
}
```

[ヘッダ]

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| isSuccessful | Boolean | API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明               |
| --- | --- |--------------------|
| result | Object | 暗号化に必要な公開鍵    |
| result.key | String | 公開鍵(Base64でエンコードされている) |
| result.version | String | 公開鍵のバージョン         |

* 公開鍵は **Base64**でエンコードされている状態です。

### クレジットカードAPI

#### クレジットカード分析API

#### リクエスト

* {appKey}と{secretKey}はコンソール上部の**URL & Appkey** メニューで確認できます。

[URI]

| メソッド | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/credit-card |

[リクエストヘッダ]

| 名前 | 値 | 説明                  |
| --- | --- |-----------------------|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー      |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン      |
| Symmetric-Key | {symmetricKey} | 発行された公開鍵で暗号化された対称鍵 |

* {symmetricKey}は必ず**32byte乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化されている必要があります(公開鍵利用)。

[フィールド]

| 名前 | タイプ | 説明 | 暗号化説明       |
| --- | --- | --- |----------------|
| image | multipart/form–data | イメージファイル | 対称鍵で暗号化されたイメージ |

* イメージファイルは必ず **AES-256/CBC/PKCS7Padding**方式で暗号化されている必要があります(対称鍵利用)。

[リクエスト本文]

```
curl -X POST '[https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card](https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/credit-card)' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

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

[ヘッダ]

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| isSuccessful | Boolean | 分析API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明 | 暗号化するかどうか |
| --- | --- | --- | --- |
| fileType | String | ファイル拡張子(.jpg, .png) |  |
| resolution | String | 推奨解像度(760\*480px)以上の場合はnormal、推奨解像度未満はlow |  |
| cardNums | List | カード番号認識結果リスト |  |
| cardNums[0].value | String | 認識結果 | O |
| cardNums[0].conf | Double | 認識結果の信頼度 |  |
| totalCardNum | List | カード番号全体認識結果 | O |
| cardNumBoxes | List | カード番号認識領域(Bounding box)座標リスト |  |
| cardNumBoxes[0] | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |  |
| validThru.value | String | 有効期限認識内容 | O |
| validThru.conf | Double | 有効期限認識結果の信頼度 |  |
| validThruBox | Object | 有効期限認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |  |

* 暗号化された項目(cardNums[0].value, totalCardNumなど)は **AES-256/CBC/PKCS7Padding**方式で暗号化されています(対称鍵を利用)。

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

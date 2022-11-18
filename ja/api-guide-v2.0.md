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
| GET | /v2.0/appkeys/{appKey}/public-keys/{serviceName} |

[リクエストヘッダ]

| 名前 | 値 | 説明            |
| --- | --- |-----------------|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[Path Variable]

| 名前 | 値 | 説明           |
| --- | --- |-----------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |
| serviceName | {serviceName} | credit-card(クレジットカードAPI呼び出し時に使用する公開鍵発行時)、<br> id-card(身分証API呼び出し時に使用する公開鍵発行時)  |

[リクエスト本文]

```
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'Authorization: ${secretKey}'
```

#### レスポンス

[レスポンス本文]

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

[Path Variable]

| 名前 | 値 | 説明           |
| --- | --- |-----------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前 | タイプ | 説明 | 暗号化説明       |
| --- | --- | --- |----------------|
| image | multipart/form–data | イメージファイル | 対称鍵で暗号化されたイメージ |

* イメージファイルは必ず **AES-256/CBC/PKCS7Padding**方式で暗号化されている必要があります(対称鍵利用)。

[リクエスト本文]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### レスポンス

[レスポンス本文]

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

### 身分証分析API

#### リクエスト

* {appKey}と{secretKey}はコンソール上部の**URL &amp; Appkey**メニューで確認できます。

[URI]

| メソッド | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
| --- | --- | --- |
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン |
| Symmetric-Key | {symmetricKey} | 発行された公開鍵で暗号化された対称鍵 |

* {symmetricKey}は必ず**32byte乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化される必要があります(公開鍵利用)。

[Path Variable]

| 名前 | 値 | 説明           |
| --- | --- |-----------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前 | タイプ | 説明 | 暗号化説明 |
| --- | --- | --- | --- |
| image | multipart/form–data | 画像ファイル | 対称鍵で暗号化された画像 |

* 画像ファイルは必ず**AES-256/CBC/PKCS7Padding**方式で暗号化される必要があります(対称鍵利用)。

[リクエスト本文]

```
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### レスポンス

[レスポンスヘッダ]

| 名前 | 説明 |
| --- | --- |
| Request-Key | 身分証真偽確認API呼び出し時に使用するRequest-Key |

* **Request-Keyを真偽確認API呼び出しに使用して正常レスポンスを得た場合、使用されたRequest-Keyは再使用できません。**
* **Request-Keyは発行後1時間有効で、その後は使用できません。**

[レスポンス本文]

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
| resolution | String | 推奨解像度(760\*480px)以上はnormal、推奨解像度未満はlow |  |
| idType | String | resident(住民登録証)、driver(運転免許証) |  |
| keyValues | List |  |  |
| keyValues[0].key | String |  |  |
| keyValues[0].value | String |  | O |
| keyValues[0].conf | Double |  |  |
| boxes | List | 認識領域(Bounding box)座標リスト |
| boxes[0] | Object  | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |

* **"idType"が"resident"と認識される場合のKeyValuesに含まれるリスト**

| key | value type | description |
| --- | --- | --- |
| **name** | string | 認識された名前 |
| **residentNumber** | string | 認識された住民登録番号 |
| **issueDate** | string | 認識された発行日時 |
| **issuer** | string | 認識された発行機関 |

* **"idType"が"driver"と認識される場合のKeyValuesに含まれるリスト**

| key | value type | description |
| --- | --- | --- | 
| **driverLicenseNumber** | string | 認識された運転免許番号 |
| **licenseType** | string | 認識された免許の種類(1種普通など)<br>2つ以上の場合は文字列内の"/"で区分 |
| **name** | string | 認識された名前 |
| **residentNumber** | string | 認識された住民登録番号 |
| **condition** | string | 認識された免許条件<br>(運転免許証に当該フィールドが存在しない場合、当該フィールドのvalueはnone) |
| **serialNum** | string | 認識された暗号一連番号 |
| **issueDate** | string | 認識された発行日時 |
| **issuer** | string | 認識された発行機関 |

* 暗号化された項目(keyValues[0].valueなど)は**AES-256/CBC/PKCS7Padding**方式で暗号化されています(対称鍵利用)。
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_document_ocr/bbox.png)

### 身分証真偽確認API

#### リクエスト

* {appKey}と{secretKey}はコンソール上部の**URL &amp; Appkey**メニューで確認できます。

[URI]

| メソッド | URI |
| --- | --- |
| POST | /v2.0/appkeys/{appKey}/id-card/authenticity |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
| --- | --- | --- |
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン |
| Symmetric-Key | {symmetricKey} | 発行された公開鍵で暗号化された対称鍵 |
| Request-Key | {Request-Key} | 身分証分析後に発行されたRequest-Key |

* {symmetricKey}は必ず**32byteの乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化される必要があります(公開鍵利用)。

[Path Variable]

| 名前 | 値 | 説明           |
| --- | --- |-----------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前 | タイプ | 説明 | idType | 暗号化するかどうか |
| --- | --- | --- | --- | --- |
| idType | String | resident(住民登録証)、driver(運転免許証) |  | X |
| name | String | 名前 |  | O |
| residentNumber | String | 住民登録番号<br>\- resident\(住民登録証\)の場合、住民登録番号の数字13桁<br>\- driver\(運転免許証\)の場合、住民登録番号の前の6桁と後ろの最初の1桁を組み合わせた数字7桁 |  | O |
| issueDate | String | 住民登録証発行日時(YYYYMMDD) | resident | O |
| driverLicenseNumber | String | 12桁の運転免許番号 | driver | O |
| serialNum | String | 5～6桁の暗号一連番号 | driver | O |

* 暗号化が必要なフィールドは必ず**AES-256/CBC/PKCS7Padding**方式で暗号化される必要があります(対称鍵利用)。

[リクエスト本文]

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

#### レスポンス

[レスポンス本文]

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

[ヘッダ]

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| isSuccessful | Boolean | 真偽確認API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| isAuthenticity | Boolean | 真偽 |

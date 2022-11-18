## AI Service > Document Recognizer > コンソール使用ガイド

コンソールに事業者登録証、クレジットカード、身分証の画像ファイルをアップロードし、分析結果を取得できます。


## 事業者登録証の分析


### 分析のための画像アップロード

分析する事業者登録証の画像をアップロードします。

- 画像は次の2つの方法でアップロードできます。
    1. **画像アップロード**ボタンをクリック
    2. 画像をドラッグアンドドロップ

### 分析

画像をアップロードした後、**分析**ボタンをクリックすると、分析結果が画面右側に表示されます。

![Business Registration](http://static.toastoven.net/prod_document_ocr/business_ocr_console_ja.png)

* [テキスト(Key Value)]分析された事業者登録証の内容をKey/Value形式で表示します。
* [JSON]分析した結果をJSON形式で表示します。
    * [success]分析成功/失敗
    * [fileType]ファイル拡張子(.pdf、.jpg、.png)
    * [keyValues]分析結果
        * [key]事業者登録証内のkeyに該当する値(区分、商号名、住所など)
        * [value]特定keyに該当する値
        * [conf]分析結果の信頼度
    * [resolution]推奨解像度(HD 1280*720px)以上の場合はnormal、推奨解像度未満はlow
    * [unitType] boxes座標単位(基本pixel、PDFの場合point)
    * [boxes]認識領域の画像上の座標値(box別{x1, y1, x2, y2, x3, y3, x4, y4}形式)
![bbox](http://static.toastoven.net/prod_document_ocr/bbox.png)
    * [JSON Sample]

    ```json
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
  
* 分析結果のコピーおよびダウンロード(Excel, JSON)機能を提供します。



## クレジットカード分析


### 分析のための画像アップロード

分析するクレジットカード画像をアップロードします。

- 画像は、次の2つの方法でアップロードできます。
    1. **画像アップロード** ボタンをクリック
    2. 画像をドラッグアンドドロップ

### 分析

画像をアップロードした後、**分析**ボタンをクリックすると分析結果が画面右側に表示されます。

![Credit Card](http://static.toastoven.net/prod_document_ocr/credit_card_ocr_console_ja.png)

* [テキスト(Key Value)]分析されたクレジットカードの内容をKey/Value形式で表示します。
* [JSON]分析した結果をJSON形式で表示します。
    * [fileType]ファイル拡張子(.jpg, .png)
    * [resolution]推奨解像度(760*480px)以上の場合はnormal、推奨解像度未満はlow
    * [cardNums]カード番号分析結果リスト
        * [value]カード番号認識結果
        * [conf]カード番号認識結果の信頼度
    * [totalCardNum]カード番号全体認識結果
    * [validThru]有効期限認識内容
        * [value]有効期限認識結果
        * [conf]有効期限認識結果の信頼度
    * [cardNumBoxes]カード番号認識領域の画像上の座標値(box別{x1, y1, x2, y2, x3, y3, x4, y4}形式)
    * [validThruBox]有効期限認識領域の画像上の座標値
![bbox](http://static.toastoven.net/prod_document_ocr/bbox.png)
    * [JSON Sample]

    ```json
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
  
* 分析結果のコピーおよびダウンロード(JSON)機能を提供します。

## 身分証分析


### 分析のための写真アップロード

分析する身分証画像をアップロードします。

- 画像は次の2つの方法でアップロードできます。
    1. **画像アップロード**ボタンをクリック
    2. 画像ドラッグ&ドロップ

### 分析

写真をアップロードした後、**分析**ボタンをクリックすると、分析結果が画面右側に表示されます。

![ID Card](http://static.toastoven.net/prod_document_ocr/id_card_ocr_console_ja.png)


* [テキスト(Key Value)]分析された身分証の内容をKey/Value形式で表示します。
* [JSON]分析した結果をJSON形式で表示します。
    * [fileType]ファイル拡張子(.jpg, .png)
    * [resolution]推奨解像度(760*480px)以上はnormal、推奨解像度未満はlow
    * [idType]住民登録証の時はresident、運転免許証の時はdriver
    * [keyValues]分析結果
      * [key]身分証内のkeyに該当する値(名前、住民登録番号など)
      * [value]特定keyに該当する値
      * [conf]分析結果の信頼度
    * [boxes]認識領域の画像上の座標値(box別{x1, y1, x2, y2, x3, y3, x4, y4}形式)
      ![bbox](http://static.toastoven.net/prod_document_ocr/bbox.png)
    * [JSON Sample]
    
    ```json
    {
      "fileType": "png",
      "resolution": "low",
      "idType": "driver",
      "keyValues": [
        {
          "key": "driverLicenseNumber",
          "value": "12-34-567890-01",
          "conf": 0.91
        },
        {
          "key": "licenseType",
          "value": "1종대형/1종보통/1종소형/특수(대형견인,/소형견인,/구난)/2종보통,/2종/소형/원동기",
          "conf": 0.51
        },
        {
          "key": "name",
          "value": "홍길순",
          "conf": 0.94
        },
        ...
      ],
      "boxes": [
        {
          "x1": 23,
          "y1": 15,
          "x2": 65,
          "y2": 15,
          "x3": 65,
          "y3": 28,
          "x4": 23,
          "y4": 28
        },
        {
          "x1": 69,
          "y1": 15,
          "x2": 112,
          "y2": 15,
          "x3": 112,
          "y3": 28,
          "x4": 69,
          "y4": 28
        },
        ...
      ]
    }
    ```
  
* 分析結果のコピーおよびダウンロード(JSON)機能を提供します。

### 修正

身分証を分析した後、分析された身分証の内容を修正できます。**修正**ボタンをクリックすると、分析された身分証の内容を修正できます。 

### 真偽確認

身分証を分析した後、分析された身分証の内容で身分証の真偽確認ができます。 **真偽確認**ボタンをクリックすると真偽確認結果がボタン右側に表示されます。

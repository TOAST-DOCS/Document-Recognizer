## AI Service > Document Recognizer > コンソール使用ガイド

コンソールから事業者登録証ファイルをアップロードして分析結果を取得します。


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
    ```
    {
            "success": true,
            "resultMessage": "success",
            "fileType": "jpg",
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

![Business Registration](http://static.toastoven.net/prod_document_ocr/credit_card_ocr_console_ja.png)

* [テキスト(Key Value)]分析されたクレジットカードの内容をKey/Value形式で表示します。
* [JSON]分析した結果をJSON形式で表示します。
    * [fileType]ファイル拡張子(.jpg, .png)
    * [resolution]推奨解像度(HD 760*480px)以上の場合はnormal、推奨解像度未満はlow
    * [cardNums]カード番号分析結果リスト
        * [value]カード番号認識結果
        * [conf]有効期限認識結果の信頼度
    * [totalCardNum]カード番号全体認識結果
    * [validThru]有効期限認識内容
            * [value]有効期限認識結果
            * [conf]有効期限認識結果の信頼度
    * [cardNumBoxes]カード番号認識領域の画像上の座標値(box別{x1, y1, x2, y2, x3, y3, x4, y4}形式)
    * [validThruBox]有効期限認識領域の画像上の座標値
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
  
* 分析結果のコピーおよびダウンロード(JSON)機能を提供します。

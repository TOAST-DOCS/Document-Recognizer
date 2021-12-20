## AI Service > Document Recognizer > Console User Guide

You can use the console to upload the business registration certificate file and get the analysis results.


## Business Registration Certificate Analysis


### Upload an Image for Analysis

Upload an image of the business registration certificate to analyze.

- Images can be uploaded in the following two methods:
    1. Click the **Upload Image** button
    2. Drag and drop the image

### Analysis

After uploading the image, click the **Analyze** button and the analysis results will appear on the right side of the screen.

![Business Registration](http://static.toastoven.net/prod_document_ocr/business_ocr_console_en.png)

* [Text (Key Value)] Displays the analyzed contents of the business registration certificate in the form of Key/Value.
* [JSON] Displays the analysis results in JSON format.
    * [success] Analysis success/failure
    * [fileType] File extension (.pdf, .jpg, .png)
    * [keyValues] Analysis results
        * [key] Value corresponding to the key in the business registration certificate (classification, shop name, address, etc.)
        * [value] Value corresponding to a specific key
        * [conf] Confidence of an analysis result
    * [resolution] normal: the resolution is the recommended resolution (HD 1280*720px) or above, low: the resolution is below the recommended resolution
    * [unitType] Coordinate unit for boxes (pixel by default, point for PDF)
    * [boxes] Coordinate values of the recognized area on the image ({x1, y1, x2, y2, x3, y3, x4, y4} format for each box)
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

* Provides the feature to copy or download analysis results (Excel, JSON).


## Credit Card Analysis


### Upload an Image for Analysis

Upload an image of the credit card to analyze.

- Images can be uploaded in the following two methods:
    1. Click the **Upload Image** button
    2. Drag and drop the image

### Analysis

After uploading the image, click the **Analyze** button and the analysis results will appear on the right side of the screen.

![Credit Card](http://static.toastoven.net/prod_document_ocr/credit_card_ocr_console_en.png)

* [Text (Key Value)] Displays the analyzed contents of the credit card in the form of Key/Value.
* [JSON] Displays the analysis results in JSON format.
    * [fileType] File extension (.jpg, .png)
    * [resolution] normal: the resolution is the recommended resolution (760*480px) or above, low: the resolution is below the recommended resolution
    * [cardNums] List of card number analysis results
        * [value] Card number recognition result
        * [conf] Confidence of card number recognition result
    * [totalCardNum] Full card number recognition result
    * [validThru] Expiration date recognition content
        * [value] Expiration date recognition result
        * [conf] Confidence of expiration date recognition result
    * [cardNumBoxes] Coordinate values of the card number recognition area on the image ({x1, y1, x2, y2, x3, y3, x4, y4} format for each box)
    * [validThruBox] Coordinate values of the expiration date recognition area on the image
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
  
* Provides the feature to copy or download analysis results (JSON).
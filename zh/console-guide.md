## AI Service > Document Recognizer > Console User Guide

You can use the console to upload the business registration certificate file and get the analysis results.


## Business Registration Certificate Analysis


### Upload an Image for Analysis

Upload the business registration certificate image to be analyzed.

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
        * [conf] Confidence score of an analysis result
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
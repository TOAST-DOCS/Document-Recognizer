## AI Service > Document Recognizer > Overview

Document Recognizer 서비스는 NHN Cloud의 OCR(광학 문자 인식) 기술을 통해 사업자등록증, 신용카드, 신분증의 문자 영역을 인식하고 영역별 문자를 추출하는 기능을 제공합니다. 
인식된 문서를 DB화하거나 문서 처리 자동화를 구현하는 고객사에서 활용할 수 있습니다. 

## Business Registration Certificate Analysis

### Main Features

* **Recognition of text areas in a business registration certificate**
    * Recognizes the text areas (bounding boxes) in a business registration certificate and provides the coordinates of the areas.

* **Extraction and analysis of key data in a business registration certificate**
    * Key data according to the classification of the business registration certificate (individual/corporate) is analyzed as a key/value pair, and provides a confidence for it.

* **Analysis results download**
    * You can download the results extracted from a business registration certificate image file as an Excel or JSON file.

### Input Image Guide

For more accurate business registration analysis, please refer to the guide below.

* File recommendations
    * This service supports analysis of business registration certificate images in .pdf, .jpeg, .png format.
    * Maximum size: 5 MB
    * Recommended resolution: 1280 x 720 or higher
* For PDF, only the analysis results for a single page is provided. (In case of multiple pages, analysis results for the first page is provided.)
* Please use an image taken in a condition where the subject has been laid out as straight as possible on a flat surface.
* Please make the image recognized as a full image of a rectangular shape.
* It might be difficult to extract the correct key/value if the text is difficult to read due to light reflection or shadows caused by the camera flash, etc.
* The service supports result analysis for black-and-white and color images, but color images are recommended for accurate analysis.
* The service provides analysis results for the business registration certificate in Korean only.

## Credit Card Analysis

### Main Features

* **Recognition of text areas in a credit card**
    * Recognizes the text areas (bounding boxes) of card number and expiration date in a credit card image and provides the coordinates of the areas.

* **Extraction and analysis of key data in a credit card**
    * Provides card number and expiration date information in the credit card image, as well as confidence for the information.

* **Analysis results download**
    * You can download the results extracted from a credit card image file as a JSON file.

### Input Image Guide

For more accurate credit card analysis, please refer to the guide below.

* File recommendations
    * File format: Supports analysis of images in .jpeg, .png format.
    * Maximum size: 5 MB
    * Recommended resolution: 760 x 480
* Please use an image taken in a condition where the subject has been laid out as straight as possible on a flat surface.
* Please make the image recognized as a full image of a rectangular shape.
* It might be difficult to extract the correct key/value if the text is difficult to read due to light reflection or shadows caused by the camera flash, etc.
* The service supports result analysis for black-and-white and color images, but color images are recommended for accurate analysis.
* If the card is a vertical card, use an image with the card number and expiration date of the vertical card in the correct orientation for recognition.
* Credit card analysis image example

![Image Example](http://static.toastoven.net/prod_document_ocr/DocumentRecognizer_ex_img_en.png)

## 신분증 분석

### 주요 기능

* **신분증 문자 영역 인식**
	* 신분증의 문자 영역(bounding box)을 인식하고, 해당 영역의 좌표를 제공합니다.

* **신분증 주요 데이터 추출 및 분석**
    * 신분증의 종류(주민등록증/운전면허증)에 따른 주요 데이터는 Key/Value 한 쌍으로 분석되며, 이에 대한 신뢰값(confidence)을 제공합니다.

* **신분증 진위 확인**
    * 신분증 이미지 파일에서 추출한 결과를 바탕으로 신분증의 진위를 확인할 수 있습니다.

* **분석 결과 다운로드**
	* 신분증 이미지 파일에서 추출한 결과를 JSON 파일로 다운로드할 수 있습니다.

### 입력 이미지 가이드

보다 정확한 신분증 분석을 위해 아래의 가이드를 참고 바랍니다.

* 파일 권장 사항
    * 파일 형식: .jpeg, .png 형식의 이미지 분석 기능을 지원합니다.
    * 최대 용량: 5MB
    * 권장 해상도: 760x480
* 이미지 권장 사항
    * 평평한 곳에서 최대한 바르게 펴진 상태로 촬영된 이미지를 사용해 주세요.
    * 사각 형태의 가득 찬 이미지로 인식해 주세요.
    * 카메라 플래쉬 등으로 인한 빛 반사나 그늘로 인해 글자가 잘 안 보이는 경우 정확한 Key/Value 추출이 어려울 수 있습니다.
    * 흑백/컬러 이미지에 대해 결과 분석이 가능하지만, 정확한 분석을 위해서 컬러 이미지를 권장합니다.
    * 신분증(주민등록증/운전면허증)은 한국어에 한해 분석 결과를 제공합니다.

## Service Targets
* 문서(사업자등록증, 신용카드, 신분증)를 자동으로 고객사 시스템에 등록하는 경우
* When you need to implement document processing automation
* When you need to build an accounting/financial management automation solution

## Privacy Policy
* While using the Document Recognizer service, the customer may collect personal and sensitive information of their users. Therefore, the customer of this service must inform a legal notice to their users as per the Personal Information Protection Act and acquire their consent regarding the matter. Also during this process, work consignment relation regarding the processing of personal information may arise between the customer and NHN Cloud. The customer who assumes the position of consignor may enter into a consignment contract with the consignee, NHN Cloud, separately in writing, and post a privacy policy notice by referencing the following:
    - Consignee: NHN Cloud Corp.
    - Consignment Description: Providing Document Recognizer service

## Agreement on technical/administrative level
* The customer must fully implement technical and administrative protection measures considering the sensitive nature of information collected/used while using the Document Recognizer service.
* To receive the information recognized by the Document Recognizer service, the customer must complete the encryption of the communication section before starting to use the Document Recognizer service.
* The original data that the customer requests for recognition to the Document Recognizer service must be stored in a secure location and must not be accessible through a URL that can be exposed externally.
* The customer must adopt the recommended transmission method (dedicated line, IPSecVPN, etc.) to provide secure recognition result data from the Document Recognizer service.
* The customer must comply with relevant laws such as the Personal Information Protection Act when storing/keeping/managing information recognized by the Document Recognizer service.
* The company may request evidence from the customer if it is necessary to verify that the customer prepared all of the technical and administrative measures set out above.
* 위와 같은 사항은 Document Recognizer 서비스를 통하여 고객이 수집/이용하는 정보가 중요한 정보에 해당하기에 고객에게 요청하는 사항입니다.<br>당사는 고객의 요청에 따라 수탁자로서 위탁 받은 범위 내에서 정보를 처리하며, 고객은 정보처리의 주체로서 위 사항의 이행을 보증하고 위반으로 인하여 발생하는 정보주체, 규제기관 등에 대한 모든 책임을 부담하는 것을 확인합니다.
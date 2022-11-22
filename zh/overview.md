## AI Service > Document Recognizer > Overview

Document Recognizer service provides a feature to recognize the text area of a business registration certificate, credit card, and ID card and extract text for each area, based on NHN Cloud's optical character recognition (OCR) technology.
It can be used by customers who need to create a database for recognized documents or implement document processing automation. 

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

## Analyze ID Card

### Main Features

* **Recognition of text areas in an ID card**
	* Recognizes the text areas (bounding boxes) in an ID card and provides the coordinates of the areas.

* **Extraction and analysis of key data in an ID card**
    * Key data according to the types of ID cards (resident registration certificate/driver license) is analyzed as a key/value pair, and provides a confidence for it.

* Verify Authenticity
    * Verifies the authenticity of an ID card based on the result extracted from the image file.

* **Analysis results download**
	* You can download the results extracted from an ID card image file as a JSON file.

### Input Image Guide

For more accurate ID card analysis, please refer to the guide below.

* File recommendations
    * File format: Supports analysis of images in .jpeg, .png format.
    * Maximum size: 5 MB
    * Recommended resolution: 760x480
* Image recommendation
    * Please use an image taken in a condition where the subject has been laid out as straight as possible on a flat surface.
    * Please make the image recognized as a full image of a rectangular shape.
    * It might be difficult to extract the correct key/value if the text is difficult to read due to light reflection or shadows caused by the camera flash, etc.
    * The service supports result analysis for black-and-white and color images, but color images are recommended for accurate analysis.
    * The service provides analysis results for ID cards (resident registration certificate/driver license) in Korean only.

## Service Targets
* When you need to register documents (business registration certificate, credit card, and ID card) in the customer's system automatically
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
* We ask for the items above from the customer because the information collected/used by the customer through the Document Recognizer service is important information. <br>We process information within the scope entrusted as a consignee at the request of the customer, and the customer, as the subject of information processing, guarantees the implementation of the above items and bears all responsibilities for the information subject and regulatory agency arising from violation. 
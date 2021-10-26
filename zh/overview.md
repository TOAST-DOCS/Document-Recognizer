## AI Service > Document Recognizer > Overview

Document Recognizer provides a feature to recognize text areas in business registration certificates and extract text in each area, based on NHN Cloud's optical character recognition (OCR) technology.
It can be used by customers who need to store business registration certificate information in a database or implement document processing automation.

## Main Features

* **Recognition of text areas in a business registration certificate**
    * Recognizes the text area (bounding box) in a business registration certificate and provides the coordinates of the area.

* **Extraction and analysis of key data in a business registration certificate**
    * Key data according to the classification of the business registration certificate (individual/corporate) is analyzed as a key/value pair, and the confidence score for the result is provided.

* **Analysis results download**
    * You can download the results extracted from a business registration certificate file as an Excel or JSON file.

## Input Image Guide

* This service supports analysis of business registration certificate images in .pdf, .jpeg, .png format.
    * Maximum size: 5 MB
    * Recommended resolution: 1280 x 720 or higher
* For PDF, only the analysis results for a single page is provided. (In case of multiple pages, analysis results for the first page is provided.)
* Please use an image taken in a condition where the subject has been laid out as straight as possible on a flat surface.
* Please make the image recognized as a full image of a rectangular shape.
* It might be difficult to extract the correct key/value if the text is difficult to read due to light reflection or shadows caused by the camera flash, etc.
* The service supports result analysis for black-and-white and color images, but color images are recommended for accurate analysis.
* The service provides analysis results for the business registration certificate in Korean only.

## Service Targets
* When you need to register business registration certificates in the customer's system automatically
* When you need to implement document processing automation
* When you need to build an accounting/financial management automation solution

## Privacy Policy
* While using the Document Recognizer service, the customer may collect personal and sensitive information of their users. Therefore, the customer of this service must inform a legal notice to their users as per the Personal Information Protection Act and acquire their consent regarding the matter. Also during this process, work consignment relation regarding the processing of personal information may arise between the customer and NHN. The customer who assumes the position of consignor may enter into a consignment contract with the consignee, NHN, separately in writing, and post a privacy policy notice by referencing the following:
    - Consignee: NHN 
    - Consignment Description: Providing Document Recognizer service

## Agreement on technical/administrative level
* The customer must fully implement technical and administrative protection measures considering the sensitive nature of information collected/used while using the Document Recognizer service.
* To receive the information recognized by the Document Recognizer service, the customer must complete the encryption of the communication section before starting to use the Document Recognizer service.
* The original data that the customer requests for recognition to the Document Recognizer service must be stored in a secure location and must not be accessible through a URL that can be exposed externally.
* The customer must adopt the recommended transmission method (dedicated line, IPSecVPN, etc.) to provide secure recognition result data from the Document Recognizer service.
* The customer must comply with relevant laws such as the Personal Information Protection Act when storing/keeping/managing information recognized by the Document Recognizer service.
* The company may request evidence from the customer if it is necessary to verify that the customer prepared all of the technical and administrative measures set out above.
* The items above are requested for the customer because the information collected/used by the customer through the Document Recognizer service is important information. The customer guarantees the fulfillment of the above and confirms that he/she bears all responsibilities for the data subject and regulatory body arising from the violation.
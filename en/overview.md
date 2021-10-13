## AI Service > Document Recognizer > 개요

Document Recognizer는 NHN Cloud의 OCR(광학 문자 인식) 기술을 통해 사업자등록증의 문자 영역을 인식하고, 영역별 문자를 추출하는 기능을 제공합니다.
사업자등록증 정보를 Database에 저장하거나 문서 처리 자동화를 구현하는 고객사의 경우 활용 할 수 있습니다.

## 주요 기능

* **사업자등록증 문자 영역 인식**
	* 사업자등록증의 문자 영역(Bounding Box)을 인식하고, 해당 영역의 좌표를 제공합니다.
	
* **사업자등록증 주요 데이터 추출 및 분석**
    * 사업자등록증 분류(개인/법인)에 따른 주요 데이터는 Key/Value 한 쌍으로 분석되며, 결과에 대한 정확도(Confidence)를 제공합니다.

* **분석 결과 다운로드**
	* 사업자등록증 파일에서 추출한 결과를 Excel 및 JSON 파일로 다운로드할 수 있습니다.

## 입력 이미지 가이드
    
* .pdf, .jpeg, .png 형식의 사업자등록증 이미지 분석 기능을 지원합니다. 
    * 최대 용량: 5MB
    * 권장 해상도: 1280 x 720 이상
* pdf의 경우 단일 페이지에 대한 분석 결과만 제공합니다. (여러 페이지인 경우 첫 페이지에 대한 분석 결과가 제공됩니다.)
* 평평한 곳에서 최대한 바르게 펴진 상태로 촬영된 이미지를 사용해 주세요.
* 사각 형태의 가득 찬 이미지로 인식해 주세요.
* 카메라 플래쉬 등으로 인한 빛 반사나 그늘로 인해 글자가 잘 안 보이는 경우 정확한 Key-Value 추출이 어려울 수 있습니다.
* 흑백/컬러 이미지에 대해 결과 분석이 가능하지만, 정확한 분석을 위해서 컬러 이미지를 권장합니다.
* 사업자등록증은 한국어에 한해 분석 결과를 제공합니다.

## 서비스 대상
* 사업자등록증을 자동으로 고객사 시스템에 등록하는 경우
* 문서 처리 자동화를 구현하는 경우
* 회계/재무 관리 자동화 솔루션을 구축하는 경우

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

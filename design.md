# Design: NurScribe System Architecture

## System Components

### 1. Data Intake & Extraction
- Amazon Textract: Used for high-accuracy extraction of handwritten medical text and table data from prescription photos.
- Amazon Comprehend Medical: Analyzes the extracted text to identify medical entities (medications, dosage, conditions) and link them to standard codes (ICD-10, RXNORM).

### 2. Clinical Intelligence
- Amazon Bedrock (Claude 3.5 Sonnet): Acts as the reasoning engine to summarize the patient’s history and flag potential drug-to-drug interactions or urgent risks.
- Kiro Integration: Used to define "Steering Files" that ensure the AI understands Indian medical abbreviations and local contexts (e.g., specific fever patterns like Malaria/Dengue common in the region).

### 3. Patient Engagement & Storage
- Amazon Connect: Triggers automated, localized outbound calls/SMS to remind patients of their dosage or follow-up dates.
- AWS HealthLake: Stores the final, structured patient records in a searchable, HIPAA-eligible central repository.

## Data Flow
1. Health worker uploads a photo of the clinical note via a mobile frontend.
2. S3 Bucket trigger launches a Lambda function.
3. Textract + Comprehend Medical extract the clinical data.
4. Bedrock analyzes risk and summarizes the case.
5. Structured data is sent to HealthLake; follow-up tasks are queued in Amazon Connect.

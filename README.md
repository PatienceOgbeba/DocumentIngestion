#  Automated Document Ingestion and Compliance Notification System

**Use Case:** Document Ingestion and Compliance Alerting for a Financial/Lending Institution

This system automates document intake, validation, and compliance notifications using serverless AWS architecture.

---

## Architecture Diagram

![Architecture Diagram](blank1-diagram.jpeg)

---

##  Architecture Overview

### Services Used
- **Amazon S3** – Storage for incoming and validated client documents.
- **AWS Lambda** – Processes, validates, and routes documents.
- **Amazon SNS** – Sends alerts and notifications to compliance teams.

---

##  Roles Involved
- **Client** – Uploads documents (e.g., loan applications, ID proofs).
- **AWS Lambda** – Executes the document validation and routing.
- **Amazon SNS** – Sends notifications.
- **Compliance Officer** – Receives notifications and takes appropriate actions.

---

##  Workflow Summary

1. **Client Uploads Document**  
   A document is uploaded (e.g., PDF, DOCX) to an S3 bucket named `client-submissions`.

2. **S3 Triggers Lambda**  
   The S3 bucket is configured to trigger a Lambda function on every `ObjectCreated` event.

3. **Lambda Validation Logic**  
   - Fetches the file from S3.
   - Validates:
     - File type (PDF, DOCX)
     - Metadata (if required fields exist)
     - Structural rules (e.g., number of pages)

4. **Document Routing:**
   - **Valid Document:** Moved to the `validated-submissions` S3 bucket.
   - **Invalid Document:** Publishes a message to an SNS topic with file details and errors.

5. **SNS Notification:**
   - An SNS topic called `DocumentComplianceAlerts` sends real-time emails to compliance and operations teams.

---

##  Step-by-Step Deployment (via AWS CloudFormation)

> This entire system can be deployed in under 10 minutes using an AWS CloudFormation template.

### 1. **Create S3 Buckets**
- `client-submissions`
- `validated-submissions`

### 2. **Create SNS Topic**
```bash
aws sns create-topic --name DocumentComplianceAlerts
```

### 3. **Subscribe Compliance Email**
```bash
aws sns subscribe --topic-arn arn:aws:sns:REGION:ACCOUNT_ID:DocumentComplianceAlerts \
  --protocol email --notification-endpoint compliance@example.com
```

### 4. **Deploy Lambda with S3 Trigger**
- Write a Lambda function that:
  - Validates file type & metadata
  - Moves valid files
  - Publishes alerts to SNS for invalid files

- Use CloudFormation to automate creation and S3 event bindings.

### 5. **Set Permissions**
- Allow Lambda to:
  - Read/write from both S3 buckets
  - Publish to the SNS topic

---

##  Business Value

-  **Efficiency** – No manual checks; everything is automated.
-  **Compliance** – Real-time alerts ensure policy enforcement.
-  **Scalability** – Serverless design handles increasing document volumes.
-  **Auditability** – All events are logged and trackable.

---

## Note
> This solution was fully deployed using **AWS CloudFormation** in under 10 minutes.

With this architecture, compliance isn’t just achieved—it’s enforced instantly and consistently.

---

**License:** MIT

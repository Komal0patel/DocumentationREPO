#INTERNSHIP DOCUMENTATION


# Camunda DMN Project Documentation

## Introduction
This documentation outlines my journey of learning and implementing Camunda for processing kidney test results and calculating kidney scores using DMN (Decision Model and Notation) tables. The project involves integrating Camunda with external applications to automate decision-making based on clinical guidelines.

## Learning Camunda
Initially, I explored Camunda BPM to understand its workflow automation and decision management capabilities. I started by deploying simple BPMN workflows and DMN tables to get hands-on experience with the platform. Through this learning phase, I discovered that for my use case—processing kidney test results—DMN tables would be the best approach for structured decision-making.

## Understanding DMN Tables
I researched DMN and realized that I could define decision logic using tables, making it easier to standardize and automate medical test result evaluations. I learned how to:
- Create DMN tables using Camunda Modeler.
- Deploy DMN tables to Camunda Cockpit.
- Test decision tables using Postman.

## Implementing DMN for Kidney Test Results
After understanding DMN, I designed individual decision tables for each kidney test based on standard clinical guidelines:

### 1. Creatinine DMN Table
This table evaluates creatinine levels based on age and gender to determine if they fall within normal, borderline, or abnormal ranges.
- **Image:** [Creatinine DMN Table](C:\Users\hp\Pictures\Screenshots\Screenshot%202025-03-12%20150141.png)

### 2. BUN (Blood Urea Nitrogen) DMN Table
The BUN table checks urea nitrogen levels in the blood and classifies them based on standard thresholds.
- **Image:** [BUN DMN Table](C:\Users\hp\Pictures\Screenshots\Screenshot%202025-03-12%20150151.png)

### 3. GFR (Glomerular Filtration Rate) DMN Table
GFR is an important indicator of kidney function. This table calculates GFR based on input parameters and determines the stage of kidney function.
- **Image:** [GFR DMN Table](C:\Users\hp\Pictures\Screenshots\Screenshot%202025-03-12%20150201.png)

### 4. UACR (Urine Albumin-to-Creatinine Ratio) DMN Table
This table evaluates the albumin-to-creatinine ratio in urine to assess kidney damage and classify it into normal, microalbuminuria, or macroalbuminuria.
- **Image:** [UACR DMN Table](C:\Users\hp\Pictures\Screenshots\Screenshot%202025-03-12%20150213.png)

### 5. Electrolytes (Potassium, Sodium) DMN Table
Electrolyte balance is crucial for kidney function. This table checks potassium and sodium levels to determine if they are within normal ranges.
- **Image:** [Electrolytes DMN Table](C:\Users\hp\Pictures\Screenshots\Screenshot%202025-03-12%20150224.png)

## Deployment and Testing
I successfully deployed a single DMN table in Camunda and tested it using Postman. Then, I deployed all individual DMN tables directly from Camunda Modeler to Cockpit and tested the results.

## Integration with External Application
To fully automate the workflow, I planned the following integration steps:
1. **Data Extraction**: A user uploads kidney test results in PDF or image format.
2. **Processing**: A Java program extracts relevant test data.
3. **Decision Making**: The extracted data is sent to Camunda DMN tables via REST API.
4. **Final Decision**: Camunda processes the data and returns a decision.
5. **User Interaction**: The final decision is sent back to the user via the webpage.

## Conclusion
By integrating Camunda DMN with external applications, I automated the kidney test evaluation process. This structured approach ensures accurate, standardized, and efficient decision-making in medical diagnostics.

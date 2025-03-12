#INTERNSHIP DOCUMENTATION


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
1. **Creatinine**
2. **BUN (Blood Urea Nitrogen)**
3. **GFR (Glomerular Filtration Rate)**
4. **UACR (Urine Albumin-to-Creatinine Ratio)**
5. **Electrolytes (Potassium, Sodium)**
6. **Main DMN Table** to combine results and determine overall kidney health.

Each table classifies test results based on age and gender.

## Deployment and Testing
I successfully deployed a single DMN table in Camunda and tested it using Postman. Then, I deployed the main DMN table (combining all linked tables) directly from Camunda Modeler to Cockpit and tested the results.

## Integration with External Application
To fully automate the workflow, I planned the following integration steps:
1. **Data Extraction**: A user uploads kidney test results in PDF or image format.
2. **Processing**: A Java program extracts relevant test data.
3. **Decision Making**: The extracted data is sent to Camunda DMN tables via REST API.
4. **Final Decision**: Camunda processes the data and returns a decision.
5. **User Interaction**: The final decision is sent back to the user via the webpage.

## Conclusion
By integrating Camunda DMN with external applications, I automated the kidney test evaluation process. This structured approach ensures accurate, standardized, and efficient decision-making in medical diagnostics.


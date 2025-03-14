# INTERNSHIP DOCUMENTATION


## Introduction
This documentation outlines our journey of learning and implementing Camunda for processing kidney test results and calculating kidney scores using DMN (Decision Model and Notation) tables. The project involves integrating Camunda with external applications to automate decision-making based on clinical guidelines.

## Learning Camunda
Initially, we explored Camunda BPM to understand its workflow automation and decision management capabilities. We started by deploying simple BPMN workflows and DMN tables to get hands-on experience with the platform. Through this learning phase, we discovered that for our use case—processing kidney test results—DMN tables would be the best approach for structured decision-making.

## Understanding DMN Tables
We researched DMN and realized that we could define decision logic using tables, making it easier to standardize and automate medical test result evaluations. We learned how to:
- Create DMN tables using Camunda Modeler.
- Deploy DMN tables to Camunda Cockpit.
- Test decision tables using Postman.

## Understanding DMN XML Structure
Each DMN table is stored as an XML file, defining the decision logic, input/output parameters, and rules. The common structure across all five DMN XML files includes:
- **Definitions Element (`<definitions>`)**: Contains all DMN elements and their namespaces.
- **Decision Element (`<decision>`)**: Defines the DMN table, including input and output parameters.
- **InputData Element (`<inputData>`)**: Specifies input variables like test values.
- **DecisionTable Element (`<decisionTable>`)**: Contains the rules and logic to determine outcomes.
- **Rule Element (`<rule>`)**: Defines conditions and corresponding outputs for each test.

### Breakdown of DMN XML for Each Table
Below is an explanation of the key XML elements for each kidney test DMN table:

### 1. Creatinine DMN Table XML
**Image:** ![Creatinine DMN Table](createnine.png)

- **Defines input as `creatinine_level` and `age_group`**.
- **Decision logic determines kidney risk categories** based on thresholds.
- **Example XML Code:**
```xml
<decision id="CreatinineDecision" name="Creatinine Evaluation">
  <decisionTable>
    <input id="input1" label="Creatinine Level"/>
    <input id="input2" label="Age Group"/>
    <output id="output1" label="Risk Category"/>
    <rule>
      <inputEntry> ... </inputEntry>
      <outputEntry> ... </outputEntry>
    </rule>
  </decisionTable>
</decision>
```

### 2. BUN (Blood Urea Nitrogen) DMN Table XML
**Image:** ![BUN DMN Table](bun.png)

- **Takes `BUN_level` as input**.
- **Classifies results into dietary factors and kidney function status**.
- **Example XML Code:**
```xml
<decision id="BUNDecision" name="BUN Evaluation">
  <decisionTable>
    <input id="input1" label="BUN Level"/>
    <output id="output1" label="BUN Classification"/>
    <rule>
      <inputEntry> ... </inputEntry>
      <outputEntry> ... </outputEntry>
    </rule>
  </decisionTable>
</decision>
```

### 3. UACR (Urine Albumin-to-Creatinine Ratio) DMN Table XML
**Image:** ![UACR DMN Table](uacr.png)

- **Uses `UACR_value` as input**.
- **Determines kidney disease progression**.
- **Example XML Code:**
```xml
<decision id="UACRDecision" name="UACR Evaluation">
  <decisionTable>
    <input id="input1" label="UACR Value"/>
    <output id="output1" label="Disease Stage"/>
    <rule>
      <inputEntry> ... </inputEntry>
      <outputEntry> ... </outputEntry>
    </rule>
  </decisionTable>
</decision>
```

### 4. Electrolytes (Potassium, Sodium) DMN Table XML
**Image:** ![Electrolytes DMN Table](electrolytes.png)

- **Takes `potassium_level` and `sodium_level` as inputs**.
- **Checks for normal or abnormal electrolyte levels**.
- **Example XML Code:**
```xml
<decision id="ElectrolytesDecision" name="Electrolytes Evaluation">
  <decisionTable>
    <input id="input1" label="Potassium Level"/>
    <input id="input2" label="Sodium Level"/>
    <output id="output1" label="Electrolyte Status"/>
    <rule>
      <inputEntry> ... </inputEntry>
      <outputEntry> ... </outputEntry>
    </rule>
  </decisionTable>
</decision>
```

### 5. GFR (Glomerular Filtration Rate) DMN Table XML
**Image:** ![GFR DMN Table](gfr.png)

- **Uses `GFR_value` as input**.
- **Classifies kidney function into stages**.
- **Example XML Code:**
```xml
<decision id="GFRDecision" name="GFR Evaluation">
  <decisionTable>
    <input id="input1" label="GFR Value"/>
    <output id="output1" label="Kidney Stage"/>
    <rule>
      <inputEntry> ... </inputEntry>
      <outputEntry> ... </outputEntry>
    </rule>
  </decisionTable>
</decision>
```

## Deployment and Testing
We successfully deployed a single DMN table in Camunda and tested it using Postman. Then, we deployed all individual DMN tables directly from Camunda Modeler to Cockpit and tested the results.

## Integration with External Application
To fully automate the workflow, we planned the following integration steps:
1. **Data Extraction**: A user uploads kidney test results in PDF or image format.
2. **Processing**: A Java program extracts relevant test data.
3. **Decision Making**: The extracted data is sent to Camunda DMN tables via REST API.
4. **Final Decision**: Camunda processes the data and returns a decision.
5. **User Interaction**: The final decision is sent back to the user via the webpage.

## Deploying DMN in Camunda

### What is Deployment?
Deployment in Camunda Modeler makes a DMN table available for execution in the Camunda Engine, allowing it to be accessed via Cockpit, REST API, or external applications.

### What Happens During Deployment?
#### 1.Model Creation

You design a DMN table in Camunda Modeler.
Define input variables (e.g., creatinine levels, age, gender) and output decisions (e.g., kidney health risk level).
Save the file as a .dmn file.

#### 2.Deployment Process

When you deploy the .dmn file from Camunda Modeler, it gets uploaded to the Camunda Engine (Cockpit or external engine via REST API).
The DMN is now stored in Camunda's database and can be executed when a request is sent.

#### 3.Execution Readiness

Once deployed, the DMN table can be tested using:
Postman API Calls (Camunda REST API)
Camunda Cockpit (Admin Panel)
External Applications (Python, Java, etc.)

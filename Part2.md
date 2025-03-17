# Text Extraction from Various File Formats

## Overview
This project provides Python functions to extract text from different file formats such as images, PDFs, CSVs, and Excel files. The extracted text is then processed to identify and extract medical test results using regular expressions.

## Requirements
To run this script, ensure you have the following Python libraries installed:

```sh
pip install opencv-python pandas pytesseract pdfplumber pillow
```

Additionally, install Tesseract OCR and update the path if required:
- Download Tesseract OCR: [https://github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki)
- Update the path in the script:
  ```python
  pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
  ```

## Functions Explanation

### 1. Extract Text from an Image
```python
def extract_text_from_image(image_path):
    image = cv2.imread(image_path)
    text = pytesseract.image_to_string(image).lower()
    return text
```
- Uses `cv2.imread()` to read the image.
- Applies OCR using `pytesseract.image_to_string()` to extract text.
- Converts text to lowercase for uniform processing.

### 2. Extract Text from a PDF
```python
def extract_text_from_pdf(file_path):
    extracted_text = []
    with pdfplumber.open(file_path) as pdf:
        for page in pdf.pages:
            text = page.extract_text()
            text = text.lower()
            if text:
                extracted_text.append(text)
            else:
               print("No text found")
    return "\n".join(extracted_text)
```
- Uses `pdfplumber.open()` to read the PDF.
- Iterates through pages, extracting text and converting it to lowercase.
- Returns the concatenated text.

### 3. Extract Text from CSV
```python
def extract_text_from_csv(csv_path):
    df = pd.read_csv(csv_path)
    text = df.to_string().lower()
    return text
```
- Reads the CSV file into a DataFrame.
- Converts all data into a string for processing.

### 4. Extract Text from Excel
```python
def extract_text_from_excel(excel_path):
    df = pd.read_excel(excel_path)
    text = df.to_string().lower()
    return text
```
- Reads the Excel file into a DataFrame.
- Converts all content into a string for further analysis.

### 5. Extract Medical Test Results from Image and PDF using Regex
```python
def extract_test_results(text):
    pattern = r"^([a-z\s/()-]*\b(?:creatinine|sodium|potassium|chloride|electrolytes|blood urea nitrogen|bun|glomerular filtration rate|gfr)\b[a-z\s]*)\s+(\d+\.?\d*)[^\n]*$"
    matches = re.findall(pattern, text, re.MULTILINE)
    data = [{"Test Name": match[0].strip(), "Result": match[1]} for match in matches]
    df = pd.DataFrame(data)
    if not df.empty:
        print("Extracted Test Results:")
        print(df)
        df.to_csv("extracted_test_results.csv", index=False)
    else:
        print("No test results found.")
```
- Extracts medical test results specifically from images and PDFs.
- Uses a regex pattern to extract test names and values.
- Stores extracted data in a DataFrame and saves it to a CSV file if found.

### 6. Extract Medical Test Results from CSV and Excel using Regex
```python
def extract_test_results1(text1):
    pattern = r"([a-z\s\(\),-]+(?:creatinine|sodium|potassium|chloride|electrolytes|blood urea nitrogen|bun|glomerular filtration rate|gfr)[a-z\s\(\),-]*)\s+(\d+\.?\d*)?"
    matches = re.findall(pattern, text1, re.MULTILINE)
    data1 = [{"Test Name": match[0].strip(), "Result": match[1]} for match in matches]
    df1 = pd.DataFrame(data1)
    if not df1.empty:
        print("Extracted Test Results:")
        print(df1)
        df1.to_csv("extracted_test_results.csv", index=False)
        print("Extracted test results saved to 'extracted_test_results.csv'.")
    else:
        print("No test results found.")
```
- Extracts medical test results specifically from CSV and Excel files.
- Uses a regex pattern to extract test names and values.
- Stores extracted data in a DataFrame and saves it to a CSV file if found.

## Main Function Execution
```python
def main():
    file_path = input("Enter the file path (image, PDF, CSV, or Excel): ")
    if not os.path.exists(file_path):
        print(f"File not found: {file_path}")
        return
    _, file_extension = os.path.splitext(file_path)
    if file_extension.lower() in ['.jpg', '.jpeg', '.png', '.webp']:
        print("Processing image...")
        text = extract_text_from_image(file_path)
        extract_test_results(text)
    elif file_extension.lower() == '.pdf':
        print("Processing PDF...")
        text = extract_text_from_pdf(file_path)
        extract_test_results(text)
    elif file_extension.lower() == '.csv':
        print("Processing CSV...")
        text1 = extract_text_from_csv(file_path)
        extract_test_results1(text1)
    elif file_extension.lower() in ['.xls', '.xlsx']:
        print("Processing Excel...")
        text1 = extract_text_from_excel(file_path)
        extract_test_results1(text1)
    else:
        print("Unsupported file type.")
        return

if __name__ == "__main__":
    main()
```
- Asks the user for the file path.
- Identifies file type and calls the appropriate function.
- Extracts text and extracts medical test results.

## Usage
Run the script:
```sh
python script.py
```
Provide the path of the file when prompted.

## Output
- Extracted test results will be printed in the console.
- The results are saved in `extracted_test_results.csv`.

## Conclusion
This script automates the extraction of medical test results from various file formats, making data processing more efficient.






# Kidney Test Results Extraction - Java Code Documentation

## Overview

This Java program processes different types of files containing kidney test results and extracts relevant data. It supports:

- **Images (JPG, PNG, WEBP, etc.)** using OCR (Tesseract)
- **PDF files** using Apache PDFBox
- **Excel files (XLSX, XLS)** using Apache POI
- **CSV files** using OpenCSV

The extracted results are printed to the console or stored in a CSV file.

## Dependencies

Ensure the following libraries are included in your project:

- `org.apache.poi.ss.usermodel.*` (for Excel processing)
- `net.sourceforge.tess4j.Tesseract` (for OCR image processing)
- `com.opencsv.CSVWriter` (for CSV processing)
- `org.apache.pdfbox.pdmodel.PDDocument` (for PDF processing)

## Code Breakdown

### 1. **Main Method**

- Prompts the user to enter a file path.
- Identifies the file type based on the extension.
- Calls the appropriate method to extract data based on the file type.
- Displays the extracted test results.

### 2. **Extracting Text from Images (OCR)**

```java
public static String extractTextFromImage(String imagePath)
```

- Uses the Tesseract OCR engine to extract text from images.
- Converts extracted text to lowercase.

### 3. **Extracting Test Results from OCR Text**

```java
public static void extractTestResults(String text)
```

- Uses regex to identify test names and results.
- Saves the extracted results into a CSV file (`test_results.csv`).

### 4. **Extracting Text from PDF**

```java
private static String extractTextFromPDF(String filePath)
```

- Uses Apache PDFBox to extract text from a given PDF file.
- Converts text to lowercase.

### 5. **Extracting Kidney Test Values from PDF Text**

```java
public static Map<String, String> extractTestValues(String text)
```

- Uses regex to match test names and values.
- Filters out unwanted ratio values.
- Returns a `Map<String, String>` of test names and their corresponding values.

### 6. **Extracting Data from Excel Files**

```java
public static Map<String, String> extractFromExcel(String filePath)
```

- Reads an Excel file using Apache POI.
- Identifies relevant test names in the spreadsheet and extracts their values.

### 7. **Helper Method to Get Next Value in Excel**

```java
private static String getNextValue(Row row, int columnIndex)
```

- Extracts numerical or string values from the next cell in the row.

### 8. **Extracting Data from CSV Files**

```java
public static Map<String, String> extractFromCSV(String filePath)
```

- Reads a CSV file.
- Identifies kidney test names and extracts their corresponding values.

## Output

- If valid test results are found, they are displayed in the console.
- If processing an image, the results are stored in `test_results.csv`.
- If an unsupported file type is provided, an error message is displayed.

## Future Enhancements

- **Support for JSON files** for better integration with APIs.
- **GUI Interface** for easier user interaction.
- **Database Integration** to store results for future analysis.

## Conclusion

This Java program efficiently extracts kidney test results from various file formats, allowing seamless processing of medical reports. The extracted data can be used for further analysis, integration with decision-making engines, or visualization in a frontend application.




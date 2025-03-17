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



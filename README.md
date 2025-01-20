# **Amazon Data Processor**

A Flask-based web application that processes Amazon SQP (Search Query Performance) and Ads Campaign CSV files, calculates metrics, and provides a consolidated dataset for download.

---

## **Features**

```
- Upload **SQP Brand Analytics** and **Ads Campaign** CSV files.
- Standardize column names for consistency.
- Merge datasets based on `Search_Query` and `Date`.
- Perform key metrics calculations:
- **CTR (%)**: Click-through rate.
- **ROAS (%)**: Return on ad spend.
- **Opportunity Score**: Remaining search volume after subtracting organic impressions.
- Export the processed data as a downloadable CSV file.
```

---

## **Prerequisites**

Ensure you have the following installed:

```
- **Python 3.7 or higher**
- Required Python packages:
- Flask
- pandas
- numpy
```

Install dependencies using:

bash

```
`pip install flask pandas numpy `
```

---

## **Folder Structure**

plaintext

```
project/
├── app.py                  # Main Flask app
├── uploads/                # Temporary folder for uploaded files
├── templates/
│   └── upload.html         # HTML template for file uploads
└── requirements.txt        # Optional: List of dependencies`
```

---

## **Setup Instructions**

### 1\. Clone the Repository

Clone the repository or download the source code:

bash

```
`git clone <repository_url>
cd project`
```

### 2\. Install Dependencies

Install the required Python libraries:

bash

```
`pip install -r requirements.txt`
```

### 3\. Run the Application

Start the Flask development server:

bash

```
`python app.py`
```

The server will run at `http://127.0.0.1:5000`.

### 4\. Upload and Process Files

```
1.  Open the app in your browser: `http://127.0.0.1:5000`.
2.  Upload the **SQP Brand Analytics** and **Ads Campaign** CSV files.
3.  Click the **Process Files** button to generate the consolidated dataset.
4.  Download the processed CSV file.
```

---

## **How It Works**


1.  **File Upload**:
```
    - Two CSV files are uploaded: one for SQP Brand Analytics and one for Ads Campaign.
    - The files are saved temporarily in the `uploads/` folder.
```
2.  **Data Processing**:
```
    - Both datasets are loaded into pandas DataFrames.
    - Columns are standardized to ensure consistency.
    - The datasets are merged on `Search_Query` and `Date`.
```
3.  **Metrics Calculation**:
```
    - **CTR (%)**: `(Ad_Clicks / Ad_Impressions) * 100`
    - **ROAS (%)**: `(Ad_Revenue / Ad_Spend) * 100`
    - **Opportunity Score**: `(Search_Volume - Organic_Impressions) / Search_Volume`
```
4.  **Export**:
```
    - The processed data is saved as `processed_data.csv` in the `uploads/` folder.
    - The file is offered as a downloadable link to the user.
```

---

## **Key Code Components**

```
### **HTML Template (`upload.html`)**
```

This template provides the user interface for uploading files:

html

```
`<form action="/process" method="post" enctype="multipart/form-data">
<label for="sqpFile">Upload SQP Brand Analytics Report File:</label><br>
<input type="file" id="sqpFile" name="sqpFile" accept=".csv"><br><br>

    <label for="adsFile">Upload Ads Campaign Report File:</label><br>
    <input type="file" id="adsFile" name="adsFile" accept=".csv"><br><br>

    <button type="submit">Process Files</button>

</form>`
```

### **Flask App Code (`app.py`)**

Handles file upload, processing, and metrics calculation:


python
```
@app.route('/process', methods=['POST'])
def process_files(): # Get uploaded files
sqp_file = request.files.get('sqpFile')
ads_file = request.files.get('adsFile')

# Save files and process using pandas
  sqp_data = pd.read_csv(sqp_path)
  ads_data = pd.read_csv(ads_path)

  # Standardize columns and merge data
  consolidated_data = pd.merge(sqp_data, ads_data, on=["Search_Query", "Date"], how="inner")

  # Calculate metrics
  consolidated_data['CTR (%)'] = (consolidated_data['Ad_Clicks'] / consolidated_data['Ad_Impressions']) * 100
  consolidated_data['ROAS (%)'] = (consolidated_data['Ad_Revenue'] / consolidated_data['Ad_Spend']) * 100

  # Save and return the file
  consolidated_data.to_csv(output_path, index=False)
  return send_file(output_path, as_attachment=True, download_name="processed_data.csv")`
```

---

## **Example Processed Dataset**

Here's a preview of what the processed CSV file might look like:

```
| Date       | Search_Query               | Ad_Impressions | Ad_Clicks | Ad_Revenue | ROAS (%) | Opportunity_Score |
| ---------- | -------------------------- | -------------- | --------- | ---------- | -------- | ----------------- |
| 2025-01-01 | puppy play pen for indoors | 12,000         | 120       | $500       | 150.0    | 0.12              |
| 2025-01-01 | dog house for outdoors     | 10,000         | 100       | $300       | 130.0    | 0.10              |
```

---

## **Troubleshooting**

- **Error: Missing Columns**: Ensure the uploaded files contain the required columns:

```
  - **SQP File**: `Search Query Volume`, `Impressions`, `Clicks`, `Search Query`, `Date`.
  - **Ads File**: `Impressions`, `Clicks`, `Revenue`, `Spend`, `Search Query`, `Date`.
  - **File Format Error**: Ensure both files are saved in **CSV** format.
  - **Deployment Warning**: Use a production server (like Gunicorn) instead of Flask's development server for deploying the app.
```
---

## **Future Enhancements**

```
- Add support for additional file formats like Excel.
- Implement user authentication for a secure environment.
- Visualize metrics with interactive charts.
- Save processed datasets in a database for future analysis.
```

---

## **License**

```
This project is open-source and free to use. Feel free to contribute or modify it as needed. 🚀
```

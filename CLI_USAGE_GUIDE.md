# Monsoon Crop Predictor CLI Usage Guide

## Installation

```powershell
pip install monsoon-crop-predictor
```

## Basic Commands

### 1. Get Package Information

```powershell
# Check version
crop-predictor --version

# Get system info and model status
crop-predictor info
```

### 2. Data Validation

Validate your rainfall data before making predictions:

```powershell
# Basic validation
crop-predictor validate --data-file "C:\path\to\your\rainfall_data.csv" --crop rice

# Validation with JSON output
crop-predictor validate --data-file "C:\path\to\your\rainfall_data.csv" --crop wheat --format json

# Save validation report
crop-predictor validate --data-file "C:\path\to\your\rainfall_data.csv" --crop maize --output "validation_report.json"
```

### 3. Rainfall Data Analysis

Analyze monsoon patterns and rainfall trends:

```powershell
# Comprehensive rainfall analysis
crop-predictor analyze --type comprehensive --data-file "C:\path\to\your\rainfall_data.csv"

# Monsoon pattern analysis
crop-predictor analyze --type monsoon --data-file "C:\path\to\your\rainfall_data.csv"

# Trends analysis for specific location
crop-predictor analyze --type trends --data-file "C:\path\to\your\rainfall_data.csv" --location "West Bengal"

# Analysis for specific time period
crop-predictor analyze --type patterns --data-file "C:\path\to\your\rainfall_data.csv" --start-year 2020 --end-year 2023

# Save analysis results
crop-predictor analyze --type comprehensive --data-file "C:\path\to\your\rainfall_data.csv" --output "analysis_report.json"
```

### 4. Crop Yield Prediction

**Note**: The prediction feature requires specific seasonal rainfall data (Jan-Feb, Mar-May, Jun-Sep, Oct-Dec) and currently has some limitations in the CLI interface.

```powershell
# Prediction using rainfall data file
crop-predictor predict --crop rice --rainfall-data "C:\path\to\your\rainfall_data.csv" --confidence

# Interactive prediction mode
crop-predictor predict --crop wheat --interactive
```

### 5. Crop Recommendations

**Note**: Like predictions, recommendations require detailed seasonal rainfall data.

```powershell
# Basic crop recommendation
crop-predictor recommend --annual 1200 --monsoon 800 --year 2023 --location "West Bengal"

# Specific crops consideration
crop-predictor recommend --annual 1200 --monsoon 800 --year 2023 --crops "rice,wheat"
```

### 6. Advanced Options

```powershell
# Enable verbose logging
crop-predictor -v info

# Custom log file
crop-predictor --log-file "crop_predictor.log" validate --data-file "data.csv" --crop rice

# Get help for any command
crop-predictor predict --help
crop-predictor analyze --help
crop-predictor validate --help
```

## Required Data Format

Your rainfall data CSV should have the following columns:

- `Year`: Year of the data
- `State Name`: State/region name
- `Dist Name`: District name
- `Annual`: Total annual rainfall (mm)
- `Jan-Feb`: Rainfall from January to February (mm)
- `Mar-May`: Rainfall from March to May (mm)
- `Jun-Sep`: Monsoon rainfall from June to September (mm)
- `Oct-Dec`: Post-monsoon rainfall from October to December (mm)

Example data format:

```csv
Year,State Name,Dist Name,Annual,Jan-Feb,Mar-May,Jun-Sep,Oct-Dec
2023,West Bengal,Kolkata,1280.5,78.6,156.2,825.4,220.3
2023,Punjab,Ludhiana,650.8,45.2,89.7,420.5,95.4
```

## Working Examples

### Data Validation Example

```powershell
# Create sample data file
echo "Year,State Name,Dist Name,Annual,Jan-Feb,Mar-May,Jun-Sep,Oct-Dec
2023,West Bengal,Kolkata,1280.5,78.6,156.2,825.4,220.3
2023,Punjab,Ludhiana,650.8,45.2,89.7,420.5,95.4" | Out-File -FilePath "sample_data.csv" -Encoding UTF8

# Validate the data
crop-predictor validate --data-file "sample_data.csv" --crop rice
```

### Analysis Example

```powershell
# Analyze rainfall patterns
crop-predictor analyze --type comprehensive --data-file "sample_data.csv"
```

## Supported Features

✅ **Data Validation**: Comprehensive validation with quality scores and recommendations
✅ **Rainfall Analysis**: Pattern analysis, trend detection, and statistical insights
✅ **Model Information**: View loaded models and system configuration
✅ **Verbose Logging**: Detailed operation logs for debugging
✅ **Multiple Output Formats**: JSON and text output options

⚠️ **Known Limitations**:

- Prediction and recommendation features require complete seasonal rainfall data
- CLI prediction interface has some compatibility issues with simplified input parameters
- Batch predictions work better with properly formatted CSV files

## Troubleshooting

1. **Missing columns error**: Ensure your data file has all required columns as specified above
2. **Model loading issues**: Run `crop-predictor info` to verify model availability
3. **Permission errors**: Run PowerShell as administrator if needed
4. **Path issues**: Use absolute paths for data files

## Python API Alternative

For more advanced usage and better error handling, consider using the Python API directly:

```python
from monsoon_crop_predictor import CropYieldPredictor
from monsoon_crop_predictor.utils import DataLoader, DataValidator

# Load and validate data
loader = DataLoader()
data = loader.load_data("your_data.csv")

validator = DataValidator()
report = validator.validate_data(data)
print(f"Data quality score: {report['quality_score']}")

# Make predictions (if data is valid)
if report['is_valid']:
    predictor = CropYieldPredictor()
    results = predictor.predict_batch(data, crop_type="rice")
    print(results)
```

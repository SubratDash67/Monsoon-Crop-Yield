# Monsoon Crop Predictor

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PyPI version](https://badge.fury.io/py/monsoon-crop-predictor.svg)](https://badge.fury.io/py/monsoon-crop-predictor)

An advanced machine learning-based crop yield prediction system that leverages monsoon patterns, rainfall data, and agricultural parameters to provide accurate crop yield forecasts for Rice, Wheat, and Maize.

## 🌾 Features

- **Multi-Crop Prediction**: Support for Rice, Wheat, and Maize yield prediction
- **Advanced ML Models**: Ensemble models combining LightGBM, XGBoost, Random Forest, and other algorithms
- **Monsoon Pattern Analysis**: Comprehensive analysis of rainfall patterns and trends
- **Risk Assessment**: Agricultural risk evaluation with mitigation recommendations
- **Crop Recommendations**: Optimal crop selection based on weather conditions
- **RESTful API**: Production-ready FastAPI backend with comprehensive endpoints
- **Command Line Interface**: Easy-to-use CLI for predictions and analysis
- **Data Validation**: Robust input validation and quality assessment
- **Confidence Intervals**: Prediction uncertainty quantification

## 📦 Installation

### Basic Installation

```bash
pip install monsoon-crop-predictor
```

### Installation with Optional Features

```bash
# With API support
pip install monsoon-crop-predictor[api]

# With CLI support
pip install monsoon-crop-predictor[cli]

# With visualization support
pip install monsoon-crop-predictor[visualization]

# With all optional features
pip install monsoon-crop-predictor[all]
```

### Development Installation

```bash
git clone https://github.com/SubratDash67/Monsoon-Crop-Yield.git
cd monsoon-crop-predictor
pip install -e .[dev]
```

## 🚀 Quick Start

### Python API

```python
from monsoon_crop_predictor import CropYieldPredictor

# Initialize predictor
predictor = CropYieldPredictor()

# Make a prediction
input_data = {
    'Annual': 1200,        # Annual rainfall in mm
    'Jun-Sep': 800,        # Monsoon rainfall in mm
    'Year': 2024,          # Year
    'State Name': 'Punjab'  # Optional location
}

result = predictor.predict_single(
    input_data=input_data,
    crop='RICE',
    include_confidence=True
)

print(f"Predicted yield: {result['predicted_yield']:.2f} kg/ha")
print(f"Confidence: {result['confidence_score']:.2f}")
```

### Command Line Interface

```bash
# Single prediction
crop-predictor predict --crop RICE --annual 1200 --monsoon 800 --year 2024 --confidence

# Batch predictions from file
crop-predictor predict --crop WHEAT --rainfall-data data.csv --output results.json

# Monsoon pattern analysis
crop-predictor analyze --type comprehensive --location "Maharashtra" --start-year 2010 --end-year 2023

# Crop recommendations
crop-predictor recommend --annual 1000 --monsoon 600 --year 2024 --location "Karnataka"

# Data validation
crop-predictor validate --data-file dataset.csv --crop RICE --output validation_report.json
```

### REST API

Start the API server:

```bash
# Install with API support
pip install monsoon-crop-predictor[api]

# Start server
uvicorn monsoon_crop_predictor.api.endpoints:app --host 0.0.0.0 --port 8000
```

Make predictions via HTTP:

```bash
curl -X POST "http://localhost:8000/predict_yield" \
     -H "Content-Type: application/json" \
     -d '{
       "crop": "RICE",
       "Annual": 1200,
       "Jun-Sep": 800,
       "year": 2024,
       "include_confidence": true
     }'
```

## 📊 API Endpoints

### Core Prediction Endpoints

- `POST /predict_yield` - Single crop yield prediction
- `POST /batch_predict` - Batch predictions for multiple inputs
- `POST /crop_recommendations` - Get optimal crop recommendations
- `POST /risk_assessment` - Assess agricultural risk factors

### Analysis Endpoints

- `POST /monsoon_analysis` - Analyze monsoon patterns and trends
- `GET /historical_data` - Retrieve historical rainfall/yield data

### Utility Endpoints

- `GET /health` - API health check
- `GET /docs` - Interactive API documentation
- `GET /redoc` - Alternative API documentation

## 🔧 Configuration

### Environment Variables

```bash
# API Configuration
CROP_PREDICTOR_HOST=0.0.0.0
CROP_PREDICTOR_PORT=8000
CROP_PREDICTOR_WORKERS=4

# Model Configuration
CROP_PREDICTOR_MODEL_DIR=/path/to/models
CROP_PREDICTOR_LOG_LEVEL=INFO
```

### Custom Configuration

```python
from monsoon_crop_predictor.utils.config import Config

# Load custom configuration
config = Config.load_custom_config('config.json')

# Initialize with custom model directory
predictor = CropYieldPredictor(model_dir='/custom/model/path')
```

## 📈 Model Performance

Our ensemble models achieve strong performance across different crops:

| Crop  | R² Score 
| ----- | -------- 
| Rice  | 0.85     
| Wheat | 0.82     
| Maize | 0.88     

## 🗂️ Data Requirements

### Input Data Format

The system expects rainfall data with the following columns:

```csv
Year,Annual,Jan-Feb,Mar-May,Jun-Sep,Oct-Dec,State Name,Dist Name
2020,1200,45,120,800,235,Punjab,Ludhiana
2021,980,30,95,650,205,Punjab,Ludhiana
```

### Required Fields

- `Year`: Year (2000-2030)
- `Annual`: Annual rainfall in mm
- `Jun-Sep`: Monsoon rainfall in mm (required for all crops)

### Optional Fields

- `Jan-Feb`: Winter rainfall in mm
- `Mar-May`: Summer rainfall in mm
- `Oct-Dec`: Post-monsoon rainfall in mm
- `State Name`: State/region name
- `Dist Name`: District name

## 🔬 Advanced Features

### Feature Engineering

The system automatically creates advanced features:

```python
from monsoon_crop_predictor.core.feature_engineer import FeatureEngineer

engineer = FeatureEngineer()

# Create rainfall features
data_with_features = engineer.create_rainfall_features(data)

# Create temporal features
data_with_temporal = engineer.create_temporal_features(data_with_features)

# Complete feature engineering pipeline
engineered_data = engineer.engineer_features_pipeline(
    data=raw_data,
    target_col='RICE YIELD (Kg per ha)',
    create_interactions=True,
    feature_selection=True,
    n_features=25
)
```

### Model Ensemble

```python
from monsoon_crop_predictor.models.ensemble import EnsemblePredictor

ensemble = EnsemblePredictor()

# Create voting ensemble
voting_model = ensemble.create_voting_ensemble(models, X_train, y_train)

# Create stacking ensemble
stacking_model = ensemble.create_stacking_ensemble(models, X_train, y_train)

# Optimize ensemble weights
optimized_weights = ensemble.optimize_ensemble_weights(
    models, X_val, y_val, method='gradient_descent'
)
```

### Data Validation

```python
from monsoon_crop_predictor.core.validator import DataValidator

validator = DataValidator()

# Generate comprehensive validation report
report = validator.generate_validation_report(data, crop='RICE')

print(f"Data quality score: {report['quality_validation']['quality_score']:.2f}")
print(f"Validation passed: {report['overall_assessment']['overall_valid']}")
```

## 🧪 Testing

Run the test suite:

```bash
# Install development dependencies
pip install -e .[dev]

# Run tests
pytest

# Run tests with coverage
pytest --cov=monsoon_crop_predictor --cov-report=html

# Run specific test categories
pytest -m "not slow"  # Skip slow tests
pytest -m "unit"      # Run only unit tests
```

## 📚 Documentation

### Generate Documentation

```bash
# Install documentation dependencies
pip install sphinx sphinx-rtd-theme

# Build documentation
cd docs
make html
```

### Example Notebooks

Check the `examples/` directory for Jupyter notebooks demonstrating:

- Basic usage and predictions
- Advanced feature engineering
- Model training and evaluation
- API usage examples
- Data analysis workflows

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Setup

```bash
# Clone repository
git clone https://github.com/SubratDash67/Monsoon-Crop-Yield.git
cd monsoon-crop-predictor

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in development mode
pip install -e .[dev]

# Install pre-commit hooks
pre-commit install

# Run tests
pytest
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Indian Meteorological Department (IMD) for rainfall data
- ICRISAT for crop yield datasets
- Scikit-learn and XGBoost communities for ML frameworks
- FastAPI team for the excellent web framework

## 📞 Support

- **Documentation**: [https://monsoon-crop-predictor.readthedocs.io/](https://monsoon-crop-predictor.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/SubratDash67/Monsoon-Crop-Yield/issues)
- **Discussions**: [GitHub Discussions](https://github.com/SubratDash67/Monsoon-Crop-Yield/discussions)

## 🗺️ Roadmap

- [ ] Support for additional crops (Cotton, Sugarcane, etc.)
- [ ] Integration with satellite imagery data
- [ ] Real-time weather API integration
- [ ] Mobile app development
- [ ] Multi-language support
- [ ] Advanced visualization dashboard
- [ ] Model interpretability features
- [ ] Integration with IoT sensors

---

**Made with ❤️ for sustainable agriculture**

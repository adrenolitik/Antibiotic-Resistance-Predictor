# 🚀 Installation Guide

## System Requirements

### Minimum Requirements
- **OS**: Linux, macOS, Windows 10+
- **Python**: 3.9 or higher
- **RAM**: 4 GB minimum, 8 GB recommended
- **Storage**: 2 GB free space
- **CPU**: Multi-core processor recommended for faster predictions

### Recommended Requirements
- **RAM**: 16 GB (for large batch processing)
- **GPU**: NVIDIA GPU with CUDA support (optional, for deep learning acceleration)
- **Storage**: 10 GB for full databases and models

---

## Installation Methods

### Method 1: Standard Installation (Recommended)

#### Step 1: Clone Repository
```bash
git clone https://github.com/adrenolitik/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor
```

#### Step 2: Create Virtual Environment
```bash
# Using venv
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Using conda
conda create -n arp python=3.9
conda activate arp
```

#### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

#### Step 4: Verify Installation
```bash
python -c "import streamlit, sklearn, pandas, numpy; print('✅ All core packages installed')"
```

#### Step 5: Run Application
```bash
streamlit run main.py
```

#### Step 6: Access Web Interface
Open browser: `http://localhost:8501`

---

### Method 2: Docker Installation

#### Using Docker
```bash
# Build image
docker build -t antibiotic-predictor .

# Run container
docker run -p 8501:8501 antibiotic-predictor
```

#### Using Docker Compose
```bash
docker-compose up
```

Access at: `http://localhost:8501`

---

### Method 3: Cloud Deployment

#### Deploy to Streamlit Cloud
1. Fork this repository
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Connect your GitHub account
4. Select the repository and branch
5. Click "Deploy"

#### Deploy to Heroku
```bash
heroku create antibiotic-predictor
git push heroku main
heroku open
```

#### Deploy to AWS/Azure/GCP
See `docs/deployment/` for detailed cloud deployment guides.

---

## Optional Dependencies

### Deep Learning Acceleration (GPU)
```bash
# For PyTorch GPU support
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# For TensorFlow GPU support
pip install tensorflow-gpu
```

### Advanced Bioinformatics
```bash
# Install RDKit (molecular descriptors)
conda install -c conda-forge rdkit

# Or using pip
pip install rdkit-pypi
```

### Database Integration
```bash
pip install pymongo sqlalchemy psycopg2-binary
```

---

## Post-Installation Setup

### 1. Download Additional Data (Optional)

```bash
# Download CARD database
python scripts/download_card_database.py

# Download bacterial genomes
python scripts/download_genomes.py

# Download antibiotic structures
python scripts/download_compounds.py
```

### 2. Initialize Models

```bash
# Train/download pre-trained models
python scripts/initialize_models.py
```

### 3. Configure Settings

Edit `config.yaml`:
```yaml
model:
  ensemble_size: 7
  confidence_threshold: 0.8

database:
  card_path: ./data/card_database.tar.bz2
  genomes_path: ./data/genomes/

server:
  host: 0.0.0.0
  port: 8501
```

---

## Troubleshooting

### Common Issues

#### 1. Import Error: No module named 'streamlit'
```bash
# Solution: Reinstall dependencies
pip install --upgrade -r requirements.txt
```

#### 2. Port Already in Use
```bash
# Solution: Use different port
streamlit run main.py --server.port 8502
```

#### 3. Memory Error with Large Sequences
```bash
# Solution: Increase memory limit or use batch processing
python main.py --max-sequence-length 10000
```

#### 4. GPU Not Detected
```bash
# Check CUDA installation
python -c "import torch; print(torch.cuda.is_available())"

# Reinstall PyTorch with CUDA
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

#### 5. BioPython Installation Fails
```bash
# Solution: Install build dependencies
# Ubuntu/Debian
sudo apt-get install python3-dev

# macOS
xcode-select --install

# Then retry
pip install biopython
```

---

## Platform-Specific Instructions

### Windows

```powershell
# Install Python from python.org
# Open PowerShell as Administrator

# Clone repository
git clone https://github.com/adrenolitik/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor

# Create virtual environment
python -m venv venv
.\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Run application
streamlit run main.py
```

### macOS

```bash
# Install Homebrew if not installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Python
brew install python@3.9

# Clone and setup
git clone https://github.com/adrenolitik/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
streamlit run main.py
```

### Linux (Ubuntu/Debian)

```bash
# Update system
sudo apt-get update
sudo apt-get upgrade

# Install Python and dependencies
sudo apt-get install python3.9 python3-pip python3-venv git

# Clone and setup
git clone https://github.com/adrenolitik/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
streamlit run main.py
```

---

## Development Installation

For contributors and developers:

```bash
# Clone with development branches
git clone --recursive https://github.com/adrenolitik/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor

# Install development dependencies
pip install -r requirements-dev.txt

# Install pre-commit hooks
pre-commit install

# Run tests
pytest tests/

# Run linting
flake8 .
black .

# Build documentation
cd docs && make html
```

---

## Verification

### Test Installation

```bash
# Run built-in tests
python -m pytest tests/

# Test with sample data
python scripts/test_installation.py

# Check model performance
python scripts/validate_models.py
```

### Expected Output
```
✅ Python 3.9+ detected
✅ All core packages installed
✅ Streamlit server running
✅ Models loaded successfully
✅ Sample prediction successful
🎉 Installation complete!
```

---

## Update Instructions

```bash
# Pull latest changes
git pull origin main

# Update dependencies
pip install --upgrade -r requirements.txt

# Restart application
streamlit run main.py
```

---

## Uninstallation

```bash
# Deactivate virtual environment
deactivate

# Remove installation
cd ..
rm -rf Antibiotic-Resistance-Predictor

# Remove conda environment (if used)
conda env remove -n arp
```

---

## Support

If you encounter issues:

1. Check [Troubleshooting](#troubleshooting) section
2. Search [GitHub Issues](https://github.com/adrenolitik/Antibiotic-Resistance-Predictor/issues)
3. Create new issue with:
   - Python version
   - Operating system
   - Error message
   - Steps to reproduce

---

**Installation complete! Ready to predict antibiotic resistance! 🧬**

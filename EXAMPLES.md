# 📚 Usage Examples

Comprehensive examples for using the Antibiotic Resistance Predictor.

---

## Table of Contents
1. [Basic Usage](#basic-usage)
2. [Command Line Interface](#command-line-interface)
3. [Python API](#python-api)
4. [Advanced Examples](#advanced-examples)
5. [Batch Processing](#batch-processing)
6. [Clinical Integration](#clinical-integration)

---

## Basic Usage

### Example 1: Simple DNA Sequence Analysis

```python
from perfect_resistance_predictor import PerfectResistancePredictor

# Initialize predictor
predictor = PerfectResistancePredictor()

# DNA sequence (example: E. coli)
sequence = """
ATGCGTACGTAGCTGATCGATCGATCGTAGCTAGCTAGCTAGCTAGC
GCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCT
"""

# Analyze resistance
result = predictor.analyze_sequence(
    sequence=sequence,
    antibiotic='Ciprofloxacin',
    sequence_type='dna'
)

# Print results
print(f"Resistance Probability: {result['resistance_result']['resistance_probability']:.1%}")
print(f"Risk Level: {result['resistance_result']['risk_level']}")
print(f"Confidence: {result['resistance_result']['confidence']:.1%}")
```

**Output:**
```
Resistance Probability: 65.3%
Risk Level: MEDIUM
Confidence: 87.2%
```

---

### Example 2: With Patient Factors

```python
# Patient information
patient_factors = {
    'age': 67,
    'creatinine_clearance': 45.0,  # mL/min
    'allergies': ['Penicillin', 'Sulfonamides'],
    'comorbidities': ['Diabetes', 'Chronic Kidney Disease']
}

# Analyze with patient context
result = predictor.analyze_sequence(
    sequence=sequence,
    antibiotic='Vancomycin',
    sequence_type='dna',
    patient_factors=patient_factors
)

# Clinical recommendations
recommendations = result['clinical_recommendations']
print("\nClinical Recommendations:")
for rec in recommendations['all_recommendations']:
    print(f"  • {rec}")

if 'patient_considerations' in recommendations:
    print("\nPatient-Specific Considerations:")
    for consideration in recommendations['patient_considerations']:
        print(f"  ⚠️ {consideration}")
```

**Output:**
```
Clinical Recommendations:
  • Standard therapy with close monitoring
  • Consider susceptibility testing
  • Monitor for treatment failure

Patient-Specific Considerations:
  ⚠️ Elderly patient - consider dose adjustment and increased monitoring
  ⚠️ Reduced kidney function - dose adjustment may be required
```

---

## Command Line Interface

### Example 3: CLI Usage

```bash
# Basic analysis
python cli.py analyze \
  --sequence "ATGCGTACGTAGCTGATCG..." \
  --antibiotic Amoxicillin \
  --output result.json

# From FASTA file
python cli.py analyze \
  --fasta input.fasta \
  --antibiotic Ciprofloxacin \
  --output results/

# With patient data
python cli.py analyze \
  --sequence "ATGCGTACG..." \
  --antibiotic Vancomycin \
  --patient-age 67 \
  --patient-creatinine 45 \
  --output report.html
```

---

## Python API

### Example 4: Comprehensive Analysis

```python
from perfect_resistance_predictor import (
    PerfectResistancePredictor,
    SequenceValidator,
    AdvancedFeatureExtractor
)

# Initialize components
predictor = PerfectResistancePredictor()
validator = SequenceValidator()
extractor = AdvancedFeatureExtractor()

# Load sequence from FASTA
from Bio import SeqIO
record = SeqIO.read("ecoli_sample.fasta", "fasta")
sequence = str(record.seq)

# Step 1: Validate
validation = validator.validate_dna_sequence(sequence)
if not validation['valid']:
    print(f"Error: {validation['error']}")
    exit(1)

print(f"✅ Sequence valid - Quality Score: {validation['quality_score']:.2f}")

# Step 2: Extract features
features = extractor.extract_comprehensive_features(validation['clean_sequence'])
print(f"📊 Extracted {len(features)} features")
print(f"   GC Content: {features['gc_content']:.1%}")
print(f"   Entropy: {features['entropy']:.2f}")

# Step 3: Test multiple antibiotics
antibiotics = ['Amoxicillin', 'Ciprofloxacin', 'Vancomycin', 'Gentamicin']

results = {}
for antibiotic in antibiotics:
    result = predictor.analyze_sequence(
        sequence=sequence,
        antibiotic=antibiotic,
        sequence_type='dna'
    )
    results[antibiotic] = result['resistance_result']

# Step 4: Generate report
import pandas as pd

report_data = []
for antibiotic, result in results.items():
    report_data.append({
        'Antibiotic': antibiotic,
        'Resistance %': f"{result['resistance_probability']:.1%}",
        'Risk Level': result['risk_level'],
        'Confidence': f"{result['confidence']:.1%}"
    })

df = pd.DataFrame(report_data)
print("\n📋 Resistance Profile:")
print(df.to_string(index=False))

# Export to CSV
df.to_csv('resistance_profile.csv', index=False)
```

**Output:**
```
✅ Sequence valid - Quality Score: 0.92
📊 Extracted 87 features
   GC Content: 50.8%
   Entropy: 3.87

📋 Resistance Profile:
  Antibiotic  Resistance %  Risk Level  Confidence
 Amoxicillin         68.3%        HIGH       89.2%
Ciprofloxacin        45.7%      MEDIUM       82.5%
  Vancomycin         12.4%         LOW       91.8%
  Gentamicin         38.9%      MEDIUM       85.3%
```

---

## Advanced Examples

### Example 5: Custom Feature Engineering

```python
from perfect_resistance_predictor import AdvancedFeatureExtractor

class CustomFeatureExtractor(AdvancedFeatureExtractor):
    def extract_custom_features(self, sequence):
        """Add custom resistance markers"""
        features = super().extract_comprehensive_features(sequence)
        
        # Add custom patterns
        features['has_beta_lactamase'] = 'TEM' in sequence or 'SHV' in sequence
        features['has_efflux_pump'] = 'MEX' in sequence
        features['has_mutation_hotspot'] = self._check_mutation_hotspot(sequence)
        
        return features
    
    def _check_mutation_hotspot(self, sequence):
        """Check for known resistance mutation hotspots"""
        hotspots = ['QRDR', 'PBP']  # Example hotspots
        return any(hs in sequence for hs in hotspots)

# Use custom extractor
extractor = CustomFeatureExtractor()
features = extractor.extract_custom_features(sequence)
print(f"Beta-lactamase detected: {features['has_beta_lactamase']}")
```

---

### Example 6: Ensemble Model Analysis

```python
# Get individual model predictions
result = predictor.analyze_sequence(sequence, 'Ciprofloxacin', 'dna')
individual_preds = result['resistance_result']['individual_predictions']

# Analyze model agreement
import matplotlib.pyplot as plt

models = list(individual_preds.keys())
predictions = list(individual_preds.values())

plt.figure(figsize=(10, 6))
plt.bar(models, predictions)
plt.axhline(y=0.5, color='r', linestyle='--', label='Threshold')
plt.xlabel('Model')
plt.ylabel('Resistance Probability')
plt.title('Individual Model Predictions')
plt.xticks(rotation=45)
plt.legend()
plt.tight_layout()
plt.savefig('model_predictions.png')
```

---

## Batch Processing

### Example 7: Process Multiple Sequences

```python
from Bio import SeqIO
import json

# Load sequences from FASTA
sequences = []
for record in SeqIO.parse("bacterial_genomes.fasta", "fasta"):
    sequences.append({
        'id': record.id,
        'sequence': str(record.seq)
    })

print(f"Loaded {len(sequences)} sequences")

# Batch analysis
results = []
for seq_data in sequences:
    try:
        result = predictor.analyze_sequence(
            sequence=seq_data['sequence'],
            antibiotic='Ciprofloxacin',
            sequence_type='dna'
        )
        
        results.append({
            'sequence_id': seq_data['id'],
            'resistance_probability': result['resistance_result']['resistance_probability'],
            'risk_level': result['resistance_result']['risk_level'],
            'confidence': result['resistance_result']['confidence']
        })
        
    except Exception as e:
        print(f"Error processing {seq_data['id']}: {e}")
        continue

# Save batch results
with open('batch_results.json', 'w') as f:
    json.dump(results, f, indent=2)

print(f"✅ Processed {len(results)} sequences successfully")
```

---

### Example 8: Parallel Batch Processing

```python
from concurrent.futures import ProcessPoolExecutor, as_completed
from tqdm import tqdm

def analyze_single_sequence(seq_data):
    """Analyze single sequence (for parallel processing)"""
    predictor = PerfectResistancePredictor()  # Create in each process
    
    result = predictor.analyze_sequence(
        sequence=seq_data['sequence'],
        antibiotic=seq_data['antibiotic'],
        sequence_type='dna'
    )
    
    return {
        'id': seq_data['id'],
        'result': result['resistance_result']
    }

# Prepare data
tasks = []
for i, record in enumerate(SeqIO.parse("genomes.fasta", "fasta")):
    tasks.append({
        'id': record.id,
        'sequence': str(record.seq),
        'antibiotic': 'Ciprofloxacin'
    })

# Parallel processing
results = []
with ProcessPoolExecutor(max_workers=4) as executor:
    futures = [executor.submit(analyze_single_sequence, task) for task in tasks]
    
    for future in tqdm(as_completed(futures), total=len(futures)):
        try:
            result = future.result()
            results.append(result)
        except Exception as e:
            print(f"Error: {e}")

print(f"✅ Completed {len(results)} analyses")
```

---

## Clinical Integration

### Example 9: Electronic Health Record (EHR) Integration

```python
class EHRIntegration:
    def __init__(self, predictor):
        self.predictor = predictor
    
    def process_patient_sample(self, patient_id, sample_data):
        """Process patient sample with EHR data"""
        
        # Get patient data from EHR
        patient_data = self.get_patient_data(patient_id)
        
        # Analyze sequence
        result = self.predictor.analyze_sequence(
            sequence=sample_data['sequence'],
            antibiotic=sample_data['prescribed_antibiotic'],
            sequence_type='dna',
            patient_factors={
                'age': patient_data['age'],
                'creatinine_clearance': patient_data['kidney_function'],
                'allergies': patient_data['allergies'],
                'comorbidities': patient_data['comorbidities']
            }
        )
        
        # Generate clinical alert if high risk
        if result['resistance_result']['risk_level'] == 'HIGH':
            self.create_clinical_alert(patient_id, result)
        
        # Store result in EHR
        self.store_result_in_ehr(patient_id, result)
        
        return result
    
    def get_patient_data(self, patient_id):
        """Fetch patient data from EHR system"""
        # Implementation depends on EHR system
        return {
            'age': 65,
            'kidney_function': 55.0,
            'allergies': ['Penicillin'],
            'comorbidities': ['Diabetes']
        }
    
    def create_clinical_alert(self, patient_id, result):
        """Create alert in EHR system"""
        alert = {
            'patient_id': patient_id,
            'type': 'ANTIBIOTIC_RESISTANCE',
            'severity': 'HIGH',
            'message': f"High resistance risk detected: {result['resistance_result']['resistance_probability']:.1%}",
            'recommendations': result['clinical_recommendations']['all_recommendations']
        }
        # Send alert to EHR
        print(f"🚨 Alert created for patient {patient_id}")
        return alert

# Usage
ehr = EHRIntegration(predictor)
result = ehr.process_patient_sample(
    patient_id='PT-12345',
    sample_data={
        'sequence': sequence,
        'prescribed_antibiotic': 'Ciprofloxacin'
    }
)
```

---

### Example 10: Export Professional Report

```python
# Generate comprehensive report
result = predictor.analyze_sequence(sequence, 'Vancomycin', 'dna')
comprehensive_report = result['comprehensive_report']

# Export as HTML
html_report = predictor.report_generator.export_report(
    comprehensive_report, 
    format='html'
)

with open('clinical_report.html', 'w') as f:
    f.write(html_report)

print("✅ Report saved: clinical_report.html")

# Export as JSON
json_report = predictor.report_generator.export_report(
    comprehensive_report,
    format='json'
)

with open('clinical_report.json', 'w') as f:
    f.write(json_report)

print("✅ Report saved: clinical_report.json")
```

---

## Real-World Example

### Example 11: Complete Clinical Workflow

```python
#!/usr/bin/env python3
"""
Complete clinical workflow example
"""

from perfect_resistance_predictor import PerfectResistancePredictor
from Bio import SeqIO
import pandas as pd
from datetime import datetime

def clinical_workflow(fasta_file, patient_data_csv, output_dir):
    """Complete clinical workflow"""
    
    # Initialize predictor
    predictor = PerfectResistancePredictor()
    
    # Load patient data
    patients = pd.read_csv(patient_data_csv)
    
    # Load sequences
    sequences = {record.id: str(record.seq) 
                for record in SeqIO.parse(fasta_file, "fasta")}
    
    # Process each patient
    results = []
    
    for _, patient in patients.iterrows():
        sample_id = patient['sample_id']
        
        if sample_id not in sequences:
            print(f"⚠️ Sequence not found for {sample_id}")
            continue
        
        # Analyze
        result = predictor.analyze_sequence(
            sequence=sequences[sample_id],
            antibiotic=patient['prescribed_antibiotic'],
            sequence_type='dna',
            patient_factors={
                'age': patient['age'],
                'creatinine_clearance': patient['creatinine_clearance'],
                'allergies': patient['allergies'].split(';') if pd.notna(patient['allergies']) else [],
                'comorbidities': patient['comorbidities'].split(';') if pd.notna(patient['comorbidities']) else []
            }
        )
        
        # Extract key information
        results.append({
            'patient_id': patient['patient_id'],
            'sample_id': sample_id,
            'antibiotic': patient['prescribed_antibiotic'],
            'resistance_probability': result['resistance_result']['resistance_probability'],
            'risk_level': result['resistance_result']['risk_level'],
            'confidence': result['resistance_result']['confidence'],
            'primary_recommendation': result['clinical_recommendations']['primary_recommendation'],
            'analysis_date': datetime.now().isoformat()
        })
        
        # Generate individual report
        report_file = f"{output_dir}/{patient['patient_id']}_report.html"
        html_report = predictor.report_generator.export_report(
            result['comprehensive_report'],
            format='html'
        )
        with open(report_file, 'w') as f:
            f.write(html_report)
    
    # Generate summary report
    summary_df = pd.DataFrame(results)
    summary_df.to_csv(f"{output_dir}/summary.csv", index=False)
    
    # Statistics
    print(f"\n📊 Analysis Complete!")
    print(f"   Total patients: {len(results)}")
    print(f"   High risk: {sum(1 for r in results if r['risk_level'] == 'HIGH')}")
    print(f"   Medium risk: {sum(1 for r in results if r['risk_level'] == 'MEDIUM')}")
    print(f"   Low risk: {sum(1 for r in results if r['risk_level'] == 'LOW')}")
    print(f"   Average confidence: {summary_df['confidence'].mean():.1%}")
    
    return summary_df

# Run workflow
if __name__ == '__main__':
    summary = clinical_workflow(
        fasta_file='patient_samples.fasta',
        patient_data_csv='patient_data.csv',
        output_dir='./clinical_reports'
    )
```

---

## Tips & Best Practices

### Performance Optimization
```python
# 1. Batch processing for better performance
# Process multiple sequences at once

# 2. Use caching for repeated sequences
from functools import lru_cache

@lru_cache(maxsize=1000)
def analyze_cached(sequence_hash, antibiotic):
    return predictor.analyze_sequence(sequence, antibiotic, 'dna')

# 3. Parallel processing for large datasets
# Use multiprocessing or concurrent.futures
```

### Error Handling
```python
try:
    result = predictor.analyze_sequence(sequence, antibiotic, 'dna')
except ValueError as e:
    print(f"Validation error: {e}")
except Exception as e:
    print(f"Analysis error: {e}")
    # Fallback to alternative method
```

---

**More examples available in `examples/` directory! 🎓**

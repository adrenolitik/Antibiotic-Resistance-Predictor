# 🤝 Contributing to Antibiotic Resistance Predictor

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

---

## Table of Contents
1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Setup](#development-setup)
4. [Making Changes](#making-changes)
5. [Submitting Changes](#submitting-changes)
6. [Coding Standards](#coding-standards)
7. [Testing Guidelines](#testing-guidelines)
8. [Documentation](#documentation)

---

## Code of Conduct

### Our Pledge
We are committed to providing a welcoming and inclusive environment for all contributors.

### Expected Behavior
- Be respectful and constructive
- Welcome newcomers and help them get started
- Focus on collaboration
- Accept constructive criticism gracefully

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or insulting comments
- Personal or political attacks
- Publishing private information

---

## Getting Started

### Ways to Contribute

1. **Report Bugs**: Submit detailed bug reports
2. **Suggest Features**: Propose new features or improvements
3. **Improve Documentation**: Fix typos, add examples, clarify instructions
4. **Write Code**: Fix bugs, implement features, optimize performance
5. **Review Pull Requests**: Provide feedback on others' contributions

### Before You Start

1. Check [existing issues](https://github.com/adrenolitik/Antibiotic-Resistance-Predictor/issues)
2. Read the [README](README.md) and [documentation](docs/)
3. Set up your [development environment](#development-setup)

---

## Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/Antibiotic-Resistance-Predictor.git
cd Antibiotic-Resistance-Predictor
```

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
# Install in development mode
pip install -e .

# Install development dependencies
pip install -r requirements-dev.txt
```

### 4. Set Up Pre-commit Hooks

```bash
pre-commit install
```

### 5. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

---

## Making Changes

### Branch Naming Convention

- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `test/` - Adding or updating tests
- `chore/` - Maintenance tasks

Examples:
```
feature/add-batch-processing
fix/sequence-validation-bug
docs/update-installation-guide
```

### Commit Message Guidelines

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance

**Examples:**
```bash
feat(predictor): add XGBoost ensemble model

- Implement XGBoost classifier
- Add hyperparameter tuning
- Update ensemble voting mechanism

Closes #123

fix(validator): handle edge case in GC content calculation

- Fix division by zero error
- Add unit tests
- Update documentation

Fixes #456

docs(readme): update installation instructions for Windows

- Add PowerShell examples
- Include troubleshooting section
- Update dependency versions
```

---

## Submitting Changes

### 1. Ensure Quality

```bash
# Run tests
pytest tests/

# Check code style
flake8 .
black .

# Type checking (if using type hints)
mypy .
```

### 2. Update Documentation

- Update README if adding features
- Add docstrings to new functions/classes
- Update CHANGELOG.md

### 3. Push Changes

```bash
git add .
git commit -m "feat: your descriptive commit message"
git push origin feature/your-feature-name
```

### 4. Create Pull Request

1. Go to GitHub repository
2. Click "New Pull Request"
3. Select your branch
4. Fill out the PR template
5. Link related issues

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] All tests pass
- [ ] Added new tests
- [ ] Manually tested

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No new warnings
- [ ] Dependent changes merged

## Screenshots (if applicable)

## Related Issues
Closes #<issue_number>
```

---

## Coding Standards

### Python Style Guide

Follow [PEP 8](https://pep8.org/) with these specifics:

```python
# Line length: 100 characters max
# Indentation: 4 spaces
# Imports: grouped and sorted

# Good
def predict_resistance(
    sequence: str,
    antibiotic: str,
    confidence_threshold: float = 0.8
) -> Dict[str, float]:
    """
    Predict antibiotic resistance.
    
    Args:
        sequence: DNA sequence
        antibiotic: Antibiotic name
        confidence_threshold: Minimum confidence level
        
    Returns:
        Dictionary with prediction results
    """
    # Implementation
    pass

# Bad
def predict(s,a,ct=0.8):
    # No docstring, unclear naming
    pass
```

### Type Hints

Use type hints for better code clarity:

```python
from typing import List, Dict, Optional, Union

def analyze_batch(
    sequences: List[str],
    antibiotic: str,
    patient_data: Optional[Dict[str, Union[int, float]]] = None
) -> List[Dict[str, float]]:
    """Analyze multiple sequences."""
    pass
```

### Documentation

```python
def complex_function(param1: str, param2: int) -> Dict:
    """
    Brief one-line description.
    
    Detailed explanation of what the function does,
    including any important details or caveats.
    
    Args:
        param1: Description of param1
        param2: Description of param2
        
    Returns:
        Dictionary containing:
            - key1: Description
            - key2: Description
            
    Raises:
        ValueError: When param1 is invalid
        TypeError: When param2 is not int
        
    Example:
        >>> result = complex_function("ATCG", 42)
        >>> print(result['key1'])
    """
    pass
```

---

## Testing Guidelines

### Writing Tests

```python
import pytest
from perfect_resistance_predictor import SequenceValidator

class TestSequenceValidator:
    """Test suite for SequenceValidator."""
    
    def setup_method(self):
        """Set up test fixtures."""
        self.validator = SequenceValidator()
    
    def test_valid_dna_sequence(self):
        """Test validation of valid DNA sequence."""
        result = self.validator.validate_dna_sequence("ATCG")
        assert result['valid'] == True
        assert result['quality_score'] > 0.5
    
    def test_invalid_characters(self):
        """Test rejection of invalid characters."""
        result = self.validator.validate_dna_sequence("ATCGXYZ")
        assert result['valid'] == False
        assert 'error' in result
    
    @pytest.mark.parametrize("sequence,expected", [
        ("ATCG", True),
        ("atcg", True),
        ("NNNN", False),
        ("", False),
    ])
    def test_various_sequences(self, sequence, expected):
        """Test various sequence types."""
        result = self.validator.validate_dna_sequence(sequence)
        assert result['valid'] == expected
```

### Running Tests

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_validator.py

# Run with coverage
pytest --cov=. --cov-report=html

# Run specific test
pytest tests/test_validator.py::TestSequenceValidator::test_valid_dna_sequence
```

---

## Documentation

### Adding Examples

When adding features, include examples:

```python
# In code
def new_feature(param):
    """
    New feature description.
    
    Example:
        >>> from predictor import new_feature
        >>> result = new_feature("input")
        >>> print(result)
        'output'
    """
    pass
```

```markdown
# In documentation
## Using New Feature

Here's how to use the new feature:

\```python
from predictor import new_feature

result = new_feature("input")
print(result)
\```
```

---

## Review Process

### What Reviewers Look For

1. **Code Quality**
   - Clear and readable
   - Follows style guide
   - No unnecessary complexity

2. **Testing**
   - Adequate test coverage
   - Edge cases handled
   - Tests pass

3. **Documentation**
   - Clear docstrings
   - README updated if needed
   - Examples provided

4. **Performance**
   - No obvious inefficiencies
   - Scalable approach

### Addressing Feedback

- Respond to all comments
- Make requested changes
- Ask questions if unclear
- Be open to suggestions

---

## Community

### Communication Channels

- **GitHub Issues**: Bug reports, feature requests
- **GitHub Discussions**: Questions, ideas, general discussion
- **Pull Requests**: Code contributions

### Getting Help

If you're stuck:

1. Check existing documentation
2. Search closed issues
3. Ask in GitHub Discussions
4. Create a new issue with "help wanted" label

---

## Recognition

Contributors will be:

- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Acknowledged in project documentation

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for contributing to Antibiotic Resistance Predictor! 🧬**

Your contributions help advance medical AI and combat antibiotic resistance worldwide.

# Cybersecurity Risk Assessment Using Markov Chains

A comprehensive cybersecurity risk assessment framework implementing an improved stochastic model using Absorbing Markov Chains (AMC) and Markov Reward Models (MRM) to analyze network vulnerabilities and attack paths.

## Overview

This framework implements a mathematical approach to cybersecurity risk assessment by:
- **Collecting vulnerability data** from the National Vulnerability Database (NVD)
- **Generating attack graphs** based on network topology
- **Modeling attack progression** using Absorbing Markov Chains
- **Assessing impact** using Markov Reward Models
- **Calculating comprehensive risk scores** combining Expected Path Length (EPL) and impact metrics
- **Applying Weibull distribution** to model vulnerability aging
- **Generating actionable remediation plans** based on risk levels

### Key Features

**Advanced Modeling**
- Absorbing Markov Chain for attack path probability
- Markov Reward Model for impact assessment
- Weibull distribution for vulnerability aging
- Time-dependent transition probabilities

**Comprehensive Analysis**
- Mean Time To System Failure (MTTSF)
- System reliability and availability calculations
- Expected Path Length (EPL) analysis
- Risk aggregation across multiple attack paths

**Rich Visualizations**
- Network topology diagrams
- Attack scenario graphs
- Transition probability heatmaps
- Risk level charts
- Reliability and availability comparisons

**Production-Ready**
- API response caching
- Retry logic with exponential backoff
- Comprehensive logging
- Configuration management
- CLI interface

## 📋 Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Output](#output)
- [Examples](#examples)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager
- (Optional) NVD API key for live vulnerability data

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd cybersecurity-risk-assessment-using-markov-chains
```

### Step 2: Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: (Optional) Set Up NVD API Key

For live vulnerability data collection:

```bash
# Create .env file
echo "NVD_API_KEY=your_api_key_here" > .env
```

Get your API key from: https://nvd.nist.gov/developers/request-an-api-key

## Quick Start

Run the framework with default settings (uses paper data):

```bash
python main.py
```

This will:
1. Load vulnerability data from the paper
2. Generate attack graph
3. Build Markov chain model
4. Calculate risks
5. Generate visualizations
6. Create comprehensive report

## Usage

### Basic Usage

```bash
# Run with paper data (default)
python main.py

# Run with live CVE data from NVD
python main.py --mode random

# Use custom configuration
python main.py --config config.yaml

# Skip visualizations for faster execution
python main.py --skip-viz

# Use existing data without re-fetching
python main.py --use-existing-data

# Disable Weibull aging
python main.py --no-weibull

# Custom output directory
python main.py --output-dir results/my_assessment

# Verbose logging
python main.py --verbose
```

### Command-Line Options

| Option | Description | Default |
|--------|-------------|---------|
| `--config PATH` | Path to configuration file (YAML/JSON) | None (uses defaults) |
| `--mode {paper,random}` | Data collection mode | `paper` |
| `--output-dir DIR` | Output directory for results | `results/` |
| `--skip-viz` | Skip visualization generation | False |
| `--use-existing-data` | Use existing vulnerability data | False |
| `--no-weibull` | Disable Weibull aging | False |
| `--verbose` | Enable verbose logging | False |

### Running Individual Modules

#### Data Collection

```bash
cd src
python data_collection.py paper  # Use paper CVEs
python data_collection.py random  # Fetch random CVEs
```

#### Attack Graph Generation

```bash
cd src
python attack_graph_generator.py
```

#### Markov Chain Analysis

```bash
cd src
python markov_chain.py
```

#### Risk Assessment

```bash
cd src
python risk_assessment.py
```

## Configuration

### Configuration File (config.yaml)

```yaml
# Weibull Distribution Parameters
weibull_parameters:
  shape: 1.5      # β (beta) - shape parameter
  scale: 365      # η (eta) - scale parameter (days)
  location: 0     # γ (gamma) - location parameter

# Risk Classification Thresholds
risk_thresholds:
  low: [0, 39]       # Low risk range
  medium: [40, 69]   # Medium risk range
  high: [70, 100]    # High risk range

# NVD API Configuration
nvd_api:
  base_url: "https://services.nvd.nist.gov/rest/json/cves/2.0"
  api_key: "${NVD_API_KEY}"
  rate_limit: 5
  cache_enabled: true
```

### Network Configuration (network_config.json)

```json
{
  "network_topology": {
    "machines": {
      "M1": {
        "name": "Web Server",
        "service": "Apache",
        "ip": "192.168.1.10",
        "os": "Linux",
        "criticality": "high"
      }
    },
    "connections": [
      {"from": "M1", "to": "M2"}
    ],
    "entry_point": "M1",
    "target": "M6"
  }
}
```

## Architecture

### Project Structure

```
cybersecurity-risk-assessment-using-markov-chains/
├── main.py                      # Main pipeline orchestrator
├── config.yaml                  # Framework configuration
├── requirements.txt             # Python dependencies
├── README.md                    # This file
│
├── src/                         # Source code
│   ├── data_collection.py       # CVE data fetching with caching
│   ├── attack_graph_generator.py # Attack graph construction
│   ├── markov_chain.py          # Absorbing Markov Chain
│   ├── reward_model.py          # Markov Reward Model
│   ├── risk_assessment.py       # Risk calculation & remediation
│   ├── visualization.py         # Visualization generation
│   ├── config_manager.py        # Configuration management
│   └── utils.py                 # Utility functions
│
├── data/                        # Data files
│   ├── raw/                     # Raw CVE data
│   ├── processed/               # Processed network configs
│   └── cache/                   # API response cache
│
├── visualizations/              # Generated plots
├── results/                     # Analysis results
├── examples/                    # Example configurations
└── logs/                        # Log files
```

### Data Flow

```
1. Data Collection → 2. Attack Graph → 3. Markov Chain → 4. Reward Model → 5. Risk Assessment → 6. Visualization → 7. Reports
```

### Core Components

#### 1. Data Collection Module
- Fetches CVE data from NVD API
- Implements caching to reduce API calls
- Applies Weibull distribution for vulnerability aging
- Supports both paper data and live fetching

#### 2. Attack Graph Generator
- Builds directed graph from network topology
- Maps vulnerabilities to services
- Identifies attack paths from entry to target
- Visualizes network structure

#### 3. Absorbing Markov Chain
- Calculates transition probability matrix
- Computes Expected Path Length (EPL)
- Determines MTTSF (Mean Time To System Failure)
- Calculates reliability and availability

#### 4. Markov Reward Model
- Assigns impact scores based on CVSS
- Calculates expected cumulative impact
- Evaluates potential damage from attacks

#### 5. Risk Assessment
- Combines probability and impact (Risk = P × I)
- Classifies risks: Low (0-39), Medium (40-69), High (70-100)
- Generates detailed remediation plans
- Compares with baseline AMC model

#### 6. Visualization Module
- Generates 8 key visualizations
- Network topology diagrams
- Attack scenario graphs
- Risk level charts
- Performance comparisons

## Output

### Generated Files

#### Results Directory
- `risk_assessment.csv` - Complete risk analysis
- `epl_results.csv` - Expected Path Length calculations
- `impact_results.csv` - Impact assessment metrics
- `risk_assessment_report.txt` - Comprehensive text report

#### Visualizations Directory
- `network_topology.png` - Network diagram
- `attack_scenarios.png` - Attack paths visualization
- `transition_matrix_heatmap.png` - Probability heatmap
- `impact_analysis.png` - Impact scores
- `risk_levels.png` - Risk classification
- `epl_comparison.png` - EPL analysis
- `reliability_comparison.png` - Reliability metrics
- `availability_comparison.png` - Availability metrics

### Risk Assessment Report

The generated report includes:
- **Risk Summary**: Count of high/medium/low risk machines
- **Detailed Risk Levels**: Probability, impact, and risk score for each machine
- **Remediation Plans**: Specific actions for each risk category
- **Model Performance**: Comparison with baseline AMC model
- **Metrics**: Reliability, availability, and MTTSF values

### Risk Categories & Remediation

| Risk Level | Score Range | Action | Priority |
|------------|-------------|--------|----------|
| **High** | 70-100 | Transfer/Reject - Immediate patching, firewall updates, service hardening | URGENT (24-48h) |
| **Medium** | 40-69 | Control - Security updates, port management, policy enforcement | HIGH (1-2 weeks) |
| **Low** | 0-39 | Accept - Monitor, maintain current controls | NORMAL (ongoing) |

## 💡 Examples

### Example 1: Basic Assessment

```bash
python main.py
```

Output:
```
✓ Collected 6 vulnerabilities
✓ Attack graph built with 6 nodes and 5 edges
✓ Found 4 possible attack paths
✓ MTTSF: 3.45 steps
✓ Reliability: 86.73%
✓ Availability: 93.21%
```

### Example 2: Custom Network

1. Create network configuration in `examples/my_network.json`
2. Update `config.yaml`:
   ```yaml
   network:
     config_path: "examples/my_network.json"
   ```
3. Run:
   ```bash
   python main.py --config config.yaml
   ```

### Example 3: Live CVE Data

```bash
# Requires NVD API key
export NVD_API_KEY="your_key_here"
python main.py --mode random
```

## API Reference

### Key Functions

#### Data Collection

```python
from data_collection import create_vulnerability_dataset

# Fetch vulnerabilities with Weibull aging
vulnerabilities = create_vulnerability_dataset(
    mode='paper',
    apply_aging=True,
    weibull_params={'shape': 1.5, 'scale': 365, 'location': 0}
)
```

#### Attack Graph

```python
from attack_graph_generator import AttackGraphGenerator

graph_gen = AttackGraphGenerator(network_config_path, vulnerabilities)
graph = graph_gen.build_graph()
paths = graph_gen.get_all_paths()
```

#### Markov Chain

```python
from markov_chain import AbsorbingMarkovChain

mc = AbsorbingMarkovChain(graph, vulnerabilities)
mc.calculate_transition_probabilities()
epl_df = mc.calculate_expected_path_length()
mttsf = mc.calculate_mttsf()
reliability = mc.calculate_reliability(time_days=12)
```

#### Risk Assessment

```python
from risk_assessment import RiskAssessment

risk_assessment = RiskAssessment(mc, reward_model)
risk_levels = risk_assessment.calculate_risk_levels()
comparison = risk_assessment.compare_with_baseline()
```

## Troubleshooting

### Common Issues

#### Issue: NVD API Rate Limiting

**Error**: `HTTP 403` or timeout errors

**Solution**:
- Get an API key from NVD (increases rate limit)
- Use `--use-existing-data` to avoid re-fetching
- Enable caching in `config.yaml`

#### Issue: Missing Dependencies

**Error**: `ModuleNotFoundError`

**Solution**:
```bash
pip install -r requirements.txt
```

#### Issue: Invalid Network Configuration

**Error**: `ValueError: Missing required key in network config`

**Solution**:
- Ensure `machines`, `connections`, `entry_point`, and `target` are defined
- Validate JSON syntax
- Check that entry_point and target exist in machines

#### Issue: Singular Matrix Warning

**Warning**: `Singular matrix detected, using pseudo-inverse`

**Solution**:
- This is usually harmless (fallback to pseudo-inverse)
- Check network topology for disconnected components
- Ensure all states are reachable

### Debug Mode

Enable verbose logging:

```bash
python main.py --verbose
```

Check logs:

```bash
tail -f logs/framework.log
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

```bash
# Install development dependencies
pip install -r requirements.txt
pip install pytest pytest-cov black flake8

# Run tests
pytest tests/ -v

# Check code style
black src/
flake8 src/
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## References

This implementation is based on the research paper:
**"An Improved Stochastic Model for Cybersecurity Risk Assessment"**

The framework implements:
- Absorbing Markov Chain (AMC) for attack probability
- Markov Reward Model (MRM) for impact assessment
- Weibull distribution for vulnerability aging
- Risk classification and remediation strategies

## Acknowledgments

- National Vulnerability Database (NVD) for CVE data
- Research community for the theoretical foundation
- Open-source contributors

---

**Made with love and vibe-code, amel, foufou, nadine**

*GitHub repository link coming soon*    

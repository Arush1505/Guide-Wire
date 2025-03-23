# Team - SOLARIS

## Overview

This Kubernetes Error Detection System is an advanced machine learning-based solution for monitoring, analyzing, and classifying Kubernetes logs to proactively identify issues before they affect your infrastructure. The system uses specialized ML models trained on thousands of real-world Kubernetes logs to detect anomalies, potential failures, and security threats.

## Key Features

### Kubernetes-Specific Log Classification

- **Component-Level Analysis**: Specialized models for kubelet, kube-apiserver, controller-manager, scheduler, etcd, and proxy logs
- **Pod Health Monitoring**: Real-time detection of pod crash loops, OOMKills, and startup failures
- **Cluster-Wide Insights**: Aggregated analysis across nodes for system-wide issue detection

### Log Collection Methods

- **Direct Cluster Integration**: Connects to Kubernetes API for real-time log streams
- **Label and Namespace Filtering**: Target specific workloads or system components
- **Historical Analysis**: Process stored log files for post-incident investigation

### Classification Capabilities

- **Error Categorization**: Precisely identifies error types (network, storage, permission, resource)
- **Severity Assessment**: Prioritizes issues based on potential impact
- **Root Cause Analysis**: Correlates related log entries to identify underlying issues

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/kubernetes-error-detection.git
cd kubernetes-error-detection

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure kubectl access (system requires cluster access)
# Your cluster should be configured in ~/.kube/config
```

## Usage

### Basic Kubernetes Log Analysis

```bash
# Analyze all pods in a namespace
python k8s_detect.py analyze-pods --namespace default

# Get and classify logs from a specific pod
python k8s_detect.py logs my-pod-name --namespace kube-system

# Process a saved Kubernetes log file
python k8s_detect.py file /path/to/kubernetes.log

# Filter and analyze pods by label
python k8s_detect.py label app=nginx --namespace production
```

### Advanced Features

```bash
# Real-time monitoring with alerts
python k8s_detect.py monitor --namespace default --alert-threshold high

# Generate cluster health report
python k8s_detect.py report --output-format html --output-file cluster-health.html

# Analyze specific Kubernetes components
python k8s_detect.py component etcd,kubelet --time-range 24h
```

## Kubernetes Component Models

The system includes specialized models for different Kubernetes components:

| Component          | Model File              | Detection Capabilities                                                |
| ------------------ | ----------------------- | --------------------------------------------------------------------- |
| kubelet            | kubelet_model.joblib    | Pod scheduling failures, volume mount issues, image pull errors       |
| kube-apiserver     | apiserver_model.joblib  | Authentication failures, request throttling, etcd connectivity issues |
| etcd               | etcd_model.joblib       | Consistency errors, leader election issues, data corruption           |
| controller-manager | controller_model.joblib | Reconciliation failures, resource management issues                   |
| scheduler          | scheduler_model.joblib  | Pod assignment failures, resource constraints                         |
| proxy              | proxy_model.joblib      | Service connectivity, network routing problems                        |

## Classification Process

1. **Log Acquisition**: Collects logs from Kubernetes API or files
2. **Component Identification**: Determines source component (kubelet, api-server, etc.)
3. **Feature Extraction**: Parses structured and unstructured log data
4. **Model Selection**: Applies the appropriate component-specific model
5. **Classification**: Categorizes log entries and assigns severity
6. **Correlation**: Groups related events to identify patterns
7. **Reporting**: Generates actionable insights and recommendations

## Extending the System

### Adding New Component Models

```python
# Train a model for a new Kubernetes component
from sklearn.ensemble import RandomForestClassifier
from kubernetes_log_toolkit import LogProcessor

# Process training data
processor = LogProcessor('new_component')
X_train, y_train = processor.prepare_training_data('training_logs/')

# Train and save model
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)
joblib.dump(model, 'models/new_component_model.joblib')

# Update component registry
update_component_registry('new_component',
                         ['feature1', 'feature2'],
                         'models/new_component_model.joblib')
```

### Custom Error Categories

Edit the `error_categories.yaml` file to define new error types and their patterns:

```yaml
custom_error_type:
  patterns:
    - "specific error pattern"
    - "another error pattern"
  severity: high
  recommended_action: "Steps to resolve this issue"
```

## Troubleshooting

- **Connection Issues**: Ensure kubectl is properly configured with cluster access
- **Permission Errors**: The service account needs log access permissions
- **Missing Models**: Verify all component models are in the models/ directory
- **Performance Issues**: Adjust batch size and sampling rate for large clusters

## Contributing

Contributions are welcome! Areas for improvement include:

- Additional Kubernetes component models
- Support for custom resource logs
- Enhanced correlation algorithms
- Integration with monitoring systems

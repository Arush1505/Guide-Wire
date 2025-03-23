# Log Classification System

A machine learning-based system for classifying various types of logs including Apache, SSH, Linux system logs, server logs, web application logs, and Kubernetes logs. Now enhanced with Gemini AI integration for synthetic log generation and better feature extraction.

## Overview

This system uses pre-trained machine learning models to classify log entries and detect potential issues or suspicious activities. It can analyze logs from files, standard input, directly from Kubernetes clusters, or even generate synthetic logs using Google's Gemini API.

## Features

- **Multiple Log Types Support**:

  - Apache web server logs
  - SSH authentication logs
  - Linux system logs
  - Server performance logs
  - Web application logs
  - Kubernetes logs

- **Multiple Input Sources**:

  - Read from log files
  - Process log directories
  - Monitor logs in real-time
  - Fetch logs from Kubernetes clusters
  - Generate synthetic logs with Gemini AI

- **Advanced Processing Capabilities**:

  - Real-time log analysis
  - Batch processing for large log files
  - AI-powered feature extraction
  - Model selection based on log type
  - Probability scores for classifications

- **Kubernetes Integration**:
  - Direct integration with kubectl
  - Pod health analysis
  - Cluster-wide log collection
  - Label-based filtering

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/log-classification-system.git
   cd log-classification-system
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Set up Gemini API (optional, for synthetic log generation):
   - Get an API key from [Google AI Studio](https://makersuite.google.com/)
   - Add your API key to `log_analysis_with_gemini.py`

## Usage

### Basic Log Classification

```bash
# Classify a single log file
python classify_real_logs.py file /path/to/logfile.log

# Classify all logs in a directory
python classify_real_logs.py directory /path/to/logs/

# Monitor a log file in real-time
python classify_real_logs.py monitor /path/to/logfile.log
```

### Kubernetes Log Analysis

```bash
# Analyze all pods in a namespace for issues
python kubernetes_logs.py analyze-pods --namespace default

# Get and classify logs from a specific pod
python kubernetes_logs.py logs my-pod-name --namespace default

# Process a Kubernetes log file
python kubernetes_logs.py file /path/to/kubernetes.log

# Get logs from pods with a specific label
python kubernetes_logs.py label app=nginx --namespace default
```

### Using Gemini for Synthetic Logs and Analysis

```bash
# Run the Gemini integration script
python log_analysis_with_gemini.py
```

This script will:

1. Randomly select a log type
2. Use Gemini to generate a synthetic log of that type
3. Extract features from the log using AI
4. Format the features for the appropriate model
5. Classify the log and show the result

## Models

The system uses pre-trained machine learning models to classify different types of logs:

- `apache_model.joblib`: Classifies Apache web server logs

  - Features: `Message`, `hour`, `minute`
  - Detects suspicious web requests, potential attacks

- `linux_log.joblib`: Classifies Linux system logs

  - Features: `combined_text`, `hour`, `minute`
  - Identifies system issues, errors, and anomalies

- `server_log.joblib`: Classifies server performance logs

  - Features: `Duration`, `Packets`, `Flows`, `Src Pt`, `Dst Pt`, `Bytes_num`, `hour`, `Proto`
  - Detects performance issues, network anomalies

- `ssh_login.joblib`: Classifies SSH authentication logs

  - Features: `hour`, `minute`, `day_of_week`, `user_encoded`, `password_encoded`
  - Identifies brute force attempts, suspicious logins

- `weblog.joblib`: Classifies web application logs
  - Features: `Request`, `Method`, `hour`, `minute`
  - Detects application attacks, suspicious activities

## Log Processing Flow

1. **Input Processing**: System ingests logs from files, directories, or Kubernetes
2. **Log Type Detection**: Automatically identifies the log type
3. **Feature Extraction**: Extracts relevant features based on log type
4. **Model Selection**: Chooses appropriate model for the log type
5. **Classification**: Applies the model to classify the log
6. **Output Generation**: Returns classification with probability and interpretation

## Gemini AI Integration

The Gemini API integration provides two key capabilities:

1. **Synthetic Log Generation**: Creates realistic log entries for any supported log type

   - Useful for testing and model validation
   - Helps demonstrate system capabilities
   - Can generate examples of both normal and suspicious logs

2. **Intelligent Feature Extraction**: Uses AI to parse log text into structured features
   - Converts unstructured log text to model-ready features
   - Handles different log formats automatically
   - Extracts time-based features from timestamps

## Kubernetes Log Analysis

The system provides specialized capabilities for Kubernetes environments:

- Pod health checks based on log patterns
- Detection of common Kubernetes issues
- Classification of logs from different Kubernetes components
- Integration with kubectl for direct log access

Supported Kubernetes components:

- kubelet
- kube-apiserver
- controller-manager
- scheduler
- ingress-controller
- network plugins (Calico, etc.)

## Extending the System

### Adding New Log Types

1. Train a new model using scikit-learn or XGBoost
2. Save the model as a `.joblib` file in the `models/` directory
3. Update the `MODEL_INFO` dictionary in the code with the new model's feature requirements
4. Add appropriate parsing logic to the LogReader class

### Customizing Gemini Prompts

You can customize how Gemini generates logs by modifying the prompts in the `generate_log_with_gemini()` function:

```python
prompts = {
    'apache_model': "Generate a realistic Apache web server log entry.",
    'linux_log': "Generate a realistic Linux system log entry.",
    # Add or modify prompts here
}
```

## Troubleshooting

- **Model loading errors**: Ensure models are in the correct format (.joblib) in the models/ directory
- **Feature errors**: Check that the log format matches what the model expects
- **Gemini API errors**: Verify your API key and internet connection

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
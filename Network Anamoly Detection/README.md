# Network Anomaly Detection

A multi-class network intrusion detection system trained on the NSL-KDD dataset using Random Forest. Classifies network traffic into five categories: normal, DoS, Probe, Privilege escalation, and Access attacks.

---

## Attack Categories

| Class | Label | Examples |
|---|---|---|
| 0 | Normal | Legitimate traffic |
| 1 | DoS | neptune, smurf, back, teardrop |
| 2 | Probe | nmap, portsweep, ipsweep, satan |
| 3 | Privilege | buffer_overflow, rootkit, sqlattack |
| 4 | Access | ftp_write, guess_passwd, imap, spy |

---

## How It Works

1. Loads the NSL-KDD dataset (`KDD+.txt`) with 43 feature columns
2. Encodes categorical features (`protocol_type`, `service`) via one-hot encoding
3. Combines encoded features with 34 numeric traffic features
4. Maps attack labels to 5 classes (0–4)
5. Splits data: 80% train+val / 20% test, then further 70/30 train/val split
6. Trains a `RandomForestClassifier` and evaluates on both validation and test sets
7. Outputs confusion matrices and a full classification report
8. Saves the model as `network_anomaly_detection_model.joblib`

---

## Dataset

**NSL-KDD** — an improved version of the KDD Cup 1999 dataset, commonly used for network intrusion detection research.

Download: [NSL-KDD Dataset](https://www.unb.ca/cic/datasets/nsl.html)

Place the file in the project root and rename it to `KDD+.txt`.

---

## Requirements

```bash
pip install numpy pandas scikit-learn seaborn matplotlib joblib
```

> **Note:** If you're on NumPy 2.x, downgrade to avoid compatibility issues with matplotlib/seaborn:
> ```bash
> pip install "numpy<2"
> ```

---

## Usage

### Train and evaluate

```bash
python training_model.py
```

Output includes:

- Validation set metrics (Accuracy, Precision, Recall, F1)
- Test set metrics
- Confusion matrix heatmaps for both sets
- Full classification report per attack class
- Saved model: `network_anomaly_detection_model.joblib`

### Load and predict on new traffic

```python
import joblib
import pandas as pd

model = joblib.load('network_anomaly_detection_model.joblib')

# Your preprocessed feature DataFrame goes here
predictions = model.predict(your_feature_df)

label_map = {0: 'Normal', 1: 'DoS', 2: 'Probe', 3: 'Privilege', 4: 'Access'}
print([label_map[p] for p in predictions])
```

> Make sure your input features are preprocessed the same way as during training — one-hot encoded `protocol_type` and `service`, joined with the numeric feature columns.

---

## Project Structure

```
├── training_model.py                        # Main training script
├── KDD+.txt                                 # NSL-KDD dataset (not included)
├── network_anomaly_detection_model.joblib   # Saved model (generated after training)
```

---

## Feature Set

**Categorical (one-hot encoded):** `protocol_type`, `service`

**Numeric (34 features):** duration, src_bytes, dst_bytes, wrong_fragment, urgent, hot, num_failed_logins, num_compromised, root_shell, su_attempted, num_root, num_file_creations, num_shells, num_access_files, num_outbound_cmds, count, srv_count, serror_rate, srv_serror_rate, rerror_rate, srv_rerror_rate, same_srv_rate, diff_srv_rate, srv_diff_host_rate, dst_host_count, dst_host_srv_count, dst_host_same_srv_rate, dst_host_diff_srv_rate, dst_host_same_src_port_rate, dst_host_srv_diff_host_rate, dst_host_serror_rate, dst_host_srv_serror_rate, dst_host_rerror_rate, dst_host_srv_rerror_rate

---

## Reference

Tavallaee, M., Bagheri, E., Lu, W., & Ghorbani, A. (2009). *A Detailed Analysis of the KDD CUP 99 Data Set*. IEEE Symposium on Computational Intelligence for Security and Defense Applications.

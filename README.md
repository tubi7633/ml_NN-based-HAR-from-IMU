# Gym Workout Recognition using IMU and Capacitive Sensor Data

This project investigates **gym workout recognition** using wearable **inertial (IMU)** and **capacitive sensor data**. We evaluate multiple neural-network-based approaches to classify **12 different gym exercises**, focusing on how data representation and temporal modeling affect performance under **strict subject-level evaluation**.

You can follow this link to reach the dataset: [RecGym Dataset](https://doi.org/10.24432/C5PW4K)

## 📌 Overview

Wearable sensors enable automatic workout logging and feedback, but recognizing fine-grained gym exercises is challenging due to:
- High inter- and intra-subject variability
- Differences in tempo, intensity, and execution style
- Sensor placement and orientation variation

Using the **RecGym dataset**, this project compares three models:
1. A weak raw-sample baseline
2. A feature-based neural network
3. A windowed 1D Convolutional Neural Network (1D-CNN)

Our results show that **carefully engineered statistical features outperform deeper temporal models** on this moderately sized dataset.

---

## 🧠 Dataset

**RecGym – Gym Workouts Recognition Dataset**

- **Sensors (7 channels):**
  - Accelerometer: Ax, Ay, Az
  - Gyroscope: Gx, Gy, Gz
  - Capacitive sensor: C1
- **Task:** 12-class supervised classification
- **Data type:** Multimodal time-series
- **Label granularity:** Session-level exercise labels

### Dataset Processing
- Session-level balancing across classes and subjects
- Fixed **subject-level split** (60% train / 20% val / 20% test)
- Normalization using training-set statistics only
- Sliding windows generated *after* data splitting to prevent leakage

---

## 🧪 Data Representations

### 1. Feature-Based Representation
Each session is summarized using **statistical features per channel**:
- Mean
- Standard deviation
- Minimum
- Maximum
- Median
- Skewness

➡️ 7 channels × 6 features = **42-dimensional feature vector**

### 2. Windowed Raw Time-Series
- Window size: 100 timesteps  
- Step size: 50 timesteps (50% overlap)  
- Input shape: `100 × 7`

---

## 🤖 Models

### Baseline: Raw-Sample MLP
- Classifies individual timesteps independently
- No temporal context
- Serves as a lower-bound reference

### Model 1: Feature-Based Neural Network
- Input: 42 statistical features
- Architecture:
  - Dense(16) + ReLU + L2 regularization
  - Dense(12) + Softmax
- Optimizer: Adam (lr = 0.001)
- Early stopping applied

### Model 2: Windowed 1D-CNN
- Multiple Conv1D blocks with batch normalization, pooling, and dropout
- Fully connected layers for classification
- Designed to capture temporal dependencies in raw signals

---

## 📊 Results

| Model | Test Accuracy |
|------|---------------|
| Raw-sample baseline | ~51% |
| Feature-based NN | **87.5%** |
| 1D-CNN (windowed) | 84.3% |

### Key Findings
- Instantaneous sensor readings lack sufficient context
- Statistical features capture exercise intensity and variability effectively
- CNN benefits from temporal modeling but is limited by dataset size and label granularity
- Subject-level splitting provides realistic generalization estimates

---

## 🔍 Analysis Highlights

- Feature-based model shows strong diagonal dominance in confusion matrices
- CNN struggles more with exercises sharing similar motion profiles (e.g., lower-body workouts)
- Mean and standard deviation features contribute most strongly to classification

---

## ⚠️ Limitations

- Limited number of subjects and sessions
- Session-level labels introduce ambiguity for sliding windows
- Controlled recording setup may not fully reflect real-world gym conditions

---

## 🚀 Future Work

- Collect data from more diverse subjects
- Add repetition-level or phase-level annotations
- Explore data augmentation for IMU signals
- Investigate contrastive or self-supervised learning approaches
- Analyze feature importance for sensor fusion optimization

---

## 📚 References

- Zhang et al., *Hybrid Body-Channel and Inertial Sensing Toolkit*, 2021  
- Bao & Intille, *Activity Recognition from Acceleration Data*, 2004  
- Ravi et al., *Accelerometer-based Activity Recognition*, 2005  
- Ordóñez & Roggen, *CNN/LSTM for Wearable HAR*, 2016  
- Patel et al., *Deep Learning for Workout Assessment*, 2017
- S. Bian and P. Lukowicz. "RecGym: Gym Workouts Recognition Dataset with IMU and Capacitive Sensor," UCI Machine Learning Repository, 2025. [Online]. Available: https://doi.org/10.24432/C5PW4K.

---

## 🏋️‍♂️ Conclusion

This project demonstrates that for **moderately sized IMU datasets**, **well-engineered statistical features** can outperform deeper temporal models when evaluated under realistic subject-level splits. The findings reinforce best practices in wearable-sensor machine learning and highlight the trade-offs between model complexity and dataset scale.


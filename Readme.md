##  Aim

Utilizing the **TinyML** principles to develop a handwriting recognition project on a microcontroller. This project demonstrates local machine learning inference on hardware with extremely limited RAM and processing power, removing the need for cloud connectivity.

##  Project Idea

The system classifies handwritten digits (0-9) by training a Neural Network in Python and deploying the optimized weights to an **Arduino Uno R3**. It processes an 8x8 pixel grid to predict the digit in real-time.

##  Hardware & Software

### **Hardware Requirements**

| Component | Purpose |
| --- | --- |
| **Microcontroller** | Arduino Uno R3 |
| **I2C LCD (16x2)** | Real-time result and status display |
| **Serial Interface** | Data feeding from PC to Microcontroller |

### **Software Requirements**

* **Python 3.x**: Model training (Scikit-learn, Numpy, Matplotlib)
* **Arduino IDE**: Firmware deployment
* **Tinkercad**: Circuit simulation and virtual testing

##  System Architecture

### **1. Data Preprocessing**

* **Normalization:** Scaling 0-16 pixel values to a 0.0–1.0 range to prevent neuron saturation.
* **Flattening:** Converting 8x8 matrices into a 1D array of 64 features.
* **Dataset:** UCI ML Hand-written Digits (1,797 samples).

### **2. Model Design (MLP)**

* **Input Layer:** 64 Neurons (Pixels).
* **Hidden Layer:** 12 Neurons with **ReLU** activation ().
* **Output Layer:** 10 Neurons (Digits 0-9).

---

##  Implementation Workflow

### **Step 1: Training (Python)**

The model is trained using `MLPClassifier`. Weights and biases are extracted and formatted into C++ arrays.

### **Step 2: Exporting (The Bridge)**

Weights are stored using the `PROGMEM` keyword. This forces the data into **Flash memory** (Program Space) instead of SRAM, keeping the memory footprint under **2KB**.

### **Step 3: Deployment (C++/Inference)**

The `predict()` function performs on-device matrix multiplication.

1. Layer 0 dot product + Bias.
2. ReLU activation.
3. Layer 1 dot product + Bias.
4. Argmax selection for final digit.

### **Step 4: Testing**

Data is sent via Serial Monitor in chunks. The LCD displays "Thinking..." followed by the predicted result.

---

##  Results & Analysis

* **Training Accuracy:** High-performance classification verified in Python before export.
* **Hardware Verification:**
* **Test 1:** 
![alt text](<test-1.png>)
![alt text](<result-1.png>)
* **Test 2:** 
![alt text](<test-2.png>)
![alt text](<result-2.png>)
* **Test 3:** 
![alt text](<test-3.png>)
![alt text](<result-3.png>)
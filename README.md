# Seasonal SST Forecasting for AI Weather Models

Recent advances show that **AI weather models** are increasingly capable of producing **subseasonal to seasonal forecasts (1–3 months)**.  
Examples include:

- Zhang et al. (2025): *[Long-range prediction with AI weather models](https://iopscience.iop.org/article/10.1088/1748-9326/adf864)*  
- Kent et al. (2025): *[Improving winter forecasts with AI](https://www.nature.com/articles/s41612-025-01198-3)*  

Models such as **ACE2** and **NeuralGCM** generate forecasts using an **autoregressive atmospheric component** that responds to both initial conditions and prescribed **sea-surface temperatures (SSTs)** throughout the forecast period.  
Because the atmosphere depends strongly on SST forcing, **the method used to forecast SSTs is critical**.

---

## Canonical Approach: Persisted SST Anomalies

The standard technique for providing future SSTs is to **persist the most recent observed anomaly** through the entire season.

**Example:**  
For a **December–February (DJF)** forecast, Kent et al. (2025) persist the **November 30 anomaly** for every day of winter.

This approach is simple but has major drawbacks:

- It embeds **high-frequency noise** that is not seasonally predictable.  
- It cannot represent **seasonal evolution** of SST patterns.  
- It may degrade atmospheric forecast skill, especially after the first few days.

---

## Proposed Method: EOF-Based Statistical Regression

I introduce an alternative approach to generate **daily winter SST forecasts** using information from the preceding fall season.

### **1. Input Data**
- Use **September–November (SON)** daily SST maps.
- Project each map onto the **first 15 empirical orthogonal functions (EOFs)**.
- This yields **15 principal-component (PC) magnitudes per day**.

### **2. Regression Model**
Train a **multiple linear regression** that maps:



This regression predicts the evolution of the 15 dominant SST modes for the entire winter season.

### **3. Reconstruction**
- Reconstruct predicted DJF SST fields by **inverting the EOF projection**.
- Add the SST climatology to obtain **absolute SST values**.

This method produces spatially smooth and temporally evolving SST fields.

---

## Results

I compare the **EOF-regression SST forecast** with the classical **persisted-anomaly** method.

### **Key Findings**
- **Lower seasonal RMSE:** EOF regression consistently reduces RMSE across the entire DJF season.  
- **Better daily skill:** After just a few forecast days, the EOF-regression method outperforms persistence on daily RMSE.  
- **More realistic evolution:** Unlike persistence, this method captures the gradual, seasonal transition of SST patterns.

---

## Summary

This work proposes a computationally lightweight, statistically grounded approach for generating **seasonally evolving SST forecasts** for AI weather models.

**Benefits of EOF-based regression:**

- Produces realistic SST evolution  
- Avoids noisy anomaly persistence  
- Achieves lower RMSE across winter  
- Improves forecast skill within days  

This makes the method a promising alternative for providing boundary SST forcing to **NeuralGCM**, **ACE2**, and other AI-based seasonal forecasting systems.

---


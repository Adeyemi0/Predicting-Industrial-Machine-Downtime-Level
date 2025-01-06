## 💾 Data Description

The following table describes the columns in the machine operating data:

| **Column Name**                     | **Data Type**     | **Category**                         |
|-------------------------------------|-------------------|--------------------------------------|
| `Date`                              | datetime64[ns]    | Date/Time (Timestamp of the reading) |
| `Machine_ID`                        | text              | Identifier (Machine Identifier)     |
| `Assembly_Line_No`                  | text              | Identifier (Assembly Line Identifier) |
| `Hydraulic_Pressure(bar)`           | decimal           | Pressure (Hydraulic Pressure)       |
| `Coolant_Pressure(bar)`             | decimal           | Pressure (Coolant Pressure)         |
| `Air_System_Pressure(bar)`          | decimal           | Pressure (Air System Pressure)      |
| `Coolant_Temperature`               | decimal           | Temperature (Coolant Temperature)    |
| `Hydraulic_Oil_Temperature`         | decimal           | Temperature (Hydraulic Oil Temperature) |
| `Spindle_Bearing_Temperature`       | decimal           | Temperature (Spindle Bearing Temperature) |
| `Spindle_Vibration`                 | decimal           | Vibration (Spindle Vibration)       |
| `Tool_Vibration`                    | decimal           | Vibration (Tool Vibration)          |
| `Spindle_Speed(RPM)`                | decimal           | Speed (Spindle Rotational Speed)    |
| `Voltage(volts)`                    | decimal           | Electrical (Machine Voltage)        |
| `Torque(Nm)`                        | decimal           | Force (Machine Torque)              |
| `Cutting(kN)`                       | decimal           | Force (Cutting Force)               |
| `Downtime`                          | text              | Indicator (Machine Downtime)        |



# The Report

## Introduction
This report presents a detailed analysis of the provided dataset containing sensor readings from industrial machinery. The analysis focuses on summarizing key metrics and identifying patterns in the data under two scenarios: **Full Dataset** (including all operational periods) and **Downtime Dataset** (specific to machinery downtime). Additionally, the report develops a predictive model that could be integrated with real-time operational data to detect potential machine failures.

---

# Executive Summary

The analysis of machine operational data provides key insights into factors influencing machine downtime and failure:

- **Hydraulic pressure**: Lower pressure levels during downtime, combined with higher spindle speeds and coolant temperatures, suggest pressure issues and overheating contribute to machine failure.
- **Torque and cutting forces**: Elevated levels during downtime indicate operational stress or resistance buildup, worsening machine performance.
- **Key monitoring variables**: Regular tracking of hydraulic pressure, coolant temperature, spindle bearing conditions, and tool vibrations is critical for proactive failure prevention.

### Key Findings:

- **Top Features**: `Hydraulic_Pressure(bar)`, `Torque(Nm)`, and `Cutting(kN)` consistently ranked as key predictors across models.
- **Best Model**: Random Forest performed the best with an F1 score and accuracy of **0.992**, showing excellent precision and recall for both downtime and non-downtime periods.
- **Machine-Specific Models**: Machine-specific models showed strong performance (F1 scores above 0.96), but the overall model maintained competitive accuracy, indicating robustness across various machines.
- **Feature Engineering**: Derived features like `Pressure_Diff`, `Pressure_Temp_Interaction`, and rolling statistics improved accuracy by capturing complex operational behaviors.

An independent t-test comparing hydraulic pressure during downtime vs. operational periods found significant differences:

- **T-statistic**: -33.56
- **P-value**: 5.31e-204
- **Conclusion**: The null hypothesis was rejected, confirming hydraulic pressure is significantly lower during downtime.

---

## Descriptive Analysis

### Full Dataset Overview
The full dataset comprises 2,500 entries, capturing a range of sensor readings across multiple parameters, such as hydraulic pressure, coolant temperature, spindle vibration, and tool vibration. Key statistical measures are summarized in the table below.

| **Parameter**                    | **Count** | **Mean**             | **Min**    | **25%**    | **50%**      | **75%**      | **Max**    | **Std Dev** |
|----------------------------------|-----------|----------------------|------------|------------|--------------|--------------|------------|-------------|
| Date                             | 2500      | 2022-03-13 05:57:42  | 2021-11-24 | 2022-02-22 | 2022-03-14   | 2022-04-02   | 2022-07-03 | N/A         |
| Hydraulic_Pressure (bar)         | 2490      | 101.41               | -14.33     | 76.36      | 96.76        | 126.42       | 191.00     | 30.29       |
| Coolant_Pressure (bar)           | 2481      | 4.95                 | 0.33       | 4.46       | 4.94         | 5.52         | 11.35      | 0.997       |
| Air_System_Pressure (bar)        | 2483      | 6.50                 | 5.06       | 6.22       | 6.51         | 6.78         | 7.97       | 0.407       |
| Coolant_Temperature (°C)         | 2488      | 18.56                | 4.10       | 10.40      | 21.20        | 25.60        | 98.20      | 8.55        |
| Hydraulic_Oil_Temperature (°C)   | 2484      | 47.62                | 35.20      | 45.10      | 47.70        | 50.10        | 61.40      | 3.77        |
| Spindle_Bearing_Temperature (°C) | 2493      | 35.06                | 22.60      | 32.50      | 35.10        | 37.60        | 49.50      | 3.76        |
| Spindle_Vibration                | 2489      | 1.01                 | -0.46      | 0.78       | 1.01         | 1.24         | 2.00       | 0.343       |
| Tool_Vibration                   | 2489      | 25.41                | 2.16       | 21.09      | 25.46        | 29.79        | 45.73      | 6.44        |
| Spindle_Speed (RPM)              | 2494      | 20,274.79            | 0.00       | 17,919.00  | 20,137.50    | 22,501.75    | 27,957.00  | 3,852.66    |
| Voltage (volts)                  | 2494      | 348.99               | 202.00     | 319.00     | 349.00       | 380.00       | 479.00     | 45.38       |
| Torque (Nm)                      | 2479      | 25.23                | 0.00       | 21.67      | 24.65        | 30.51        | 55.55      | 6.14        |
| Cutting (kN)                     | 2493      | 2.78                 | 1.80       | 2.25       | 2.78         | 3.27         | 3.93       | 0.62        |

#### Key Observations:
- **Hydraulic Pressure:** Displays significant variability (std dev of 30.29), with anomalously low values, indicating potential sensor errors or operational issues.
- **Coolant Pressure:** Stable across operations, with a mean of 4.95 bar and low variability (std dev of 0.997).
- **Spindle Speed:** Mean RPM is 20,275, with large fluctuations (std dev of 3,852), highlighting diverse machine load conditions.
- **Voltage:** Shows variability, with a wide range (202–479 volts), possibly reflecting inconsistent power supply.

### Downtime Dataset Overview
The downtime dataset focuses exclusively on periods of machine faliures. It captures 1,265 entries for various parameters, with key statistics outlined below:

| **Parameter**                    | **Count** | **Mean**      | **Min**    | **25%**    | **50%**      | **75%**      | **Max**    | **Std Dev** |
|----------------------------------|-----------|---------------|------------|------------|--------------|--------------|------------|-------------|
| Hydraulic_Pressure (bar)         | 1264      | 84.76         | 50.14      | 64.80      | 80.13        | 94.29        | 191.00     | 26.34       |
| Coolant_Pressure (bar)           | 1265      | 5.11          | 0.33       | 4.55       | 4.79         | 5.57         | 6.96       | 0.92        |
| Air_System_Pressure (bar)        | 1257      | 6.50          | 5.06       | 6.23       | 6.51         | 6.78         | 7.97       | 0.40        |
| Coolant_Temperature (°C)         | 1265      | 19.98         | 4.10       | 12.30      | 23.00        | 26.50        | 36.50      | 8.38        |
| Hydraulic_Oil_Temperature (°C)   | 1260      | 47.57         | 35.20      | 45.10      | 47.60        | 50.10        | 59.50      | 3.79        |
| Spindle_Bearing_Temperature (°C) | 1263      | 34.99         | 23.20      | 32.40      | 35.00        | 37.60        | 49.50      | 3.83        |
| Spindle_Vibration                | 1261      | 1.00          | 0.03       | 0.78       | 0.99         | 1.24         | 2.00       | 0.35        |
| Tool_Vibration                   | 1259      | 25.37         | 5.78       | 21.06      | 25.33        | 29.75        | 45.49      | 6.50        |
| Spindle_Speed (RPM)              | 1265      | 21,319.36     | 0.00       | 18,461.00  | 19,964.00    | 25,528.00    | 27,957.00  | 4,123.21    |
| Voltage (volts)                  | 1265      | 349.26        | 202.00     | 319.00     | 348.00       | 380.00       | 473.00     | 45.32       |
| Torque (Nm)                      | 1256      | 22.76         | 0.00       | 16.96      | 24.63        | 26.32        | 36.25      | 5.21        |
| Cutting (kN)                     | 1263      | 3.06          | 1.80       | 2.55       | 2.84         | 3.59         | 3.93       | 0.57        |

#### Key Observations:
- **Hydraulic Pressure:** Lower mean (84.76 bar) and slightly reduced variability compared to the full dataset.
- **Coolant Temperature:** Higher during downtime (mean 19.98°C).
- **Torque and Cutting:** Reduced values during downtime.
                                                                                                                                                                                            
# Boxplot Analysis: Downtime vs. No Downtime  

Boxplots are used to visualize the distribution of a dataset based on a summary of the quartiles and potential outliers. In this analysis, the boxplots compare the distribution of numeric features for machines that experienced **downtime** versus those that did **not**.  

![Alt Text](boxplots_numeric_columns_downtime.png)  

## Key Insights from the Boxplots  

### **Overall Trends**  
Boxplots reveal distinct patterns across pressure, temperature, and vibration features between downtime and no-downtime cases. Several features demonstrate variability or deviations that may serve as indicators for machine failures. Below are detailed observations for key feature groups.  



### **1. Pressure Variables**  
**(Hydraulic_Pressure, Coolant_Pressure, Air_System_Pressure)**  

- **Hydraulic Pressure:**  
  Machines experiencing downtime show a wider range and more outliers in hydraulic pressure, suggesting instability or fluctuations as potential indicators of machine failure.  

- **Coolant Pressure and Air System Pressure:**  
  Deviations in these features during downtime indicate that maintaining stable pressure levels is critical for avoiding machine failures.  

**Actionable Insight:**  
Monitor and log pressure fluctuations, particularly in hydraulic and coolant systems, to identify early warning signs of potential failures.  



### **2. Temperature Variables**  
**(Coolant_Temperature, Hydraulic_Oil_Temperature, Spindle_Bearing_Temperature)**  

- **Coolant and Hydraulic Oil Temperatures:**  
  Downtime scenarios exhibit higher medians and more outliers, implying that overheating or unstable temperature control correlates with machine failures.  

- **Spindle Bearing Temperature:**  
  Elevated spindle bearing temperatures during downtime suggest mechanical stress, making this a key parameter for predictive maintenance.  

**Actionable Insight:**  
Establish temperature thresholds and integrate automated alerts for overheating in coolant and spindle systems.  


### **3. Vibration Variables**  
**(Spindle_Vibration, Tool_Vibration)**  

- **Spindle and Tool Vibration:**  
  Machines experiencing downtime exhibit higher variability in vibration levels, indicating potential misalignments, wear, or mechanical inefficiencies.  

**Actionable Insight:**  
Conduct routine vibration monitoring and recalibration to prevent excessive wear leading to downtime.  



### **4. Other Features**  
**(Spindle_Speed (RPM), Voltage (volts), Torque (Nm), Cutting Force (KN))**  

- **Spindle Speed:**  
  Erratic or reduced spindle speeds during downtime highlight possible mechanical inefficiencies.  

- **Voltage and Torque:**  
  Lower voltage or torque values in downtime cases may indicate electrical or mechanical strain.  

- **Cutting Force:**  
  Unstable cutting forces during downtime are indicative of tool wear or operational inefficiency.  

**Actionable Insight:**  
Monitor torque and cutting forces closely during high-load operations to minimize potential disruptions.  



# Heatmap Analysis: Correlations Between Metrics  

Two correlation heatmaps compare the relationships between machine metrics under **downtime** (Yes) and **no downtime** (No).  

![Alt Text](correlation_heatmap_downtime.png)  
![Alt Text](no_downtime_correlation_heatmap_downtime.png)  

## Key Observations  

### **Overall Trends**  
- Correlations across metrics remain generally weak (mostly between -0.1 to 0.2).  
- Slight variations in correlation strength and direction distinguish downtime and no-downtime scenarios, revealing insights into potential failure mechanisms.  



### **1. Downtime (Yes)**  
When downtime occurs, some relationships between metrics become more pronounced:  

- **Coolant Pressure vs. Cutting Force (0.23):**  
  Higher coolant pressure correlates with increased cutting force, indicating potential stress under high-load conditions.  

- **Torque vs. Hydraulic Pressure (-0.16):**  
  Negative correlation suggests hydraulic systems are under strain during high torque operations.  

- **Spindle Speed vs. Cutting Force (0.15):**  
  Positive correlation highlights the importance of stable spindle speeds during cutting operations.  

**Key Takeaway:**  
During downtime, stronger correlations suggest that machine stress under load (e.g., cutting force and torque) may contribute to failures.  



### **2. No Downtime (No)**  
When downtime does not occur, correlations are weaker and less distinct:  

- **Spindle Speed vs. Coolant Pressure (0.063):**  
  Slight positive trend suggests minimal impact of spindle speed on coolant performance.  

- **Air System Pressure vs. Torque (0.065):**  
  Weak positive relationship indicates limited dependency between these metrics.  

- **Hydraulic Pressure vs. Cutting Force (0.028):**  
  Minimal correlation shows stable hydraulic pressure under normal cutting conditions.  

**Key Takeaway:**  
Weaker correlations during no-downtime scenarios suggest that machine systems operate independently and efficiently under normal conditions.  


## Comparative Insights  

- **Coolant Pressure and Cutting Force** exhibit stronger correlation during downtime (0.23) compared to no downtime (-0.024), suggesting coolant pressure is more critical during operational stress.  
- **Torque and Hydraulic Pressure** have a negative correlation during downtime (-0.16), indicating strain on hydraulic systems under higher torque.  

**Actionable Insight:**  
Focus on monitoring metrics with stronger correlations during downtime (e.g., coolant pressure, torque) to identify and address potential stress points in the system.  



## Summary of Recommendations  

| Feature Group          | Key Insight                                       | Recommended Action                              |
|-------------------------|--------------------------------------------------|------------------------------------------------|
| Pressure Variables      | Pressure fluctuations linked to downtime         | Monitor and log pressure variations regularly. |
| Temperature Variables   | Elevated temperatures correlate with downtime    | Set automated alerts for overheating.          |
| Vibration Variables     | Excessive vibration indicates wear or misalignment | Perform routine vibration analysis.            |
| Cutting and Torque      | Stress during high-load operations impacts metrics | Monitor cutting force and torque for anomalies.|

---

## Hypothesis Testing:

In order to test whether Hydraulic Pressure during downtime is significantly different from when the machine is operational, I used a t-test for independent samples. The t-test compares the means of two independent groups (in this case, downtime and no downtime) to determine whether there is evidence that the associated population means are significantly different.

### Step 1: Define the Hypotheses

- **Null Hypothesis (H₀):** There is no significant difference in hydraulic pressure between downtime and no downtime situations.
  
  $H_0: \mu_{\text{downtime}} = \mu_{\text{no downtime}}$

- **Alternative Hypothesis (H₁):** There is a significant difference in hydraulic pressure between downtime and no downtime situations.
  
  $H_1: \mu_{\text{downtime}} \neq \mu_{\text{no downtime}}$

### Step 2: Perform the t-test

The t-test compares the means of the two groups and calculates a t-statistic and p-value. We use the following formulas:

#### **T-statistic:**

$$
t = \frac{(\bar{X}_1 - \bar{X}_2)}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}
$$

Where:
- $\bar{X}_1$ and $\bar{X}_2$ are the sample means of the two groups (downtime and no downtime).
- $s_1^2$ and $s_2^2$ are the sample variances of the two groups.
- $n_1$ and $n_2$ are the sample sizes (number of observations) of the two groups.

#### **P-value:**

The p-value indicates the probability of observing the data (or something more extreme) assuming that the null hypothesis is true. If the p-value is below the significance level (typically 0.05), we reject the null hypothesis.

### Step 3: Results

From the Python code, we performed the independent t-test on the hydraulic pressure values for machines that experienced downtime and those that did not. The results were:

- **T-statistic:** -33.56
- **P-value:** 5.31e-204

### Interpretation of Results:

- **T-statistic (-33.56):** A very large negative value suggests a significant difference between the two groups.

- **P-value (5.31e-204):** The p-value is extremely small (essentially 0), which is much smaller than the threshold of 0.05. This indicates that we have strong evidence to reject the null hypothesis.

Since the p-value is less than 0.05, we reject the null hypothesis. Therefore, we conclude that there **is a significant difference in hydraulic pressure during downtime** compared to when the machine is operational.
---


# Detailed Machine Learning Report

## 1. Data Preprocessing and Feature Engineering

- **Feature Conversion**: Variables such as `Machine_ID`, `Assembly_Line_No`, and `Downtime` were converted to categorical codes for compatibility with machine learning algorithms.
- **Derived Features**: Key engineering efforts included:
  - Interaction terms: `Pressure_Temp_Interaction`, `Vibration_Speed_Interaction`
  - Rolling averages: `Rolling_Mean_Temperature`, `Rolling_Mean_Vibration`
  - Lagged features: `Lag_1_Temperature`, `Lag_1_Vibration`
  - Flags: `Overheat_Flag`, `Vibration_Flag`
  - Cumulative and differential measures: `Cumulative_Pressure`, `Pressure_Change`
  - Segmentation: `Temperature_Bin`, `Pressure_Bin`
- **Dropped Columns**: Redundant columns like `Date`, `Month`, `Year`, and shift-related fields were removed to focus on operational metrics.

## 2. Model Evaluation

Three algorithms were evaluated using the top six features ranked by importance.

| **Algorithm**       | **Features Used for Prediction**                                                                                   | **F1 Score** | **Accuracy** | **Confusion Matrix**          | **Strengths**                                                     |
|----------------------|----------------------------------------------------------------------------------------------------|--------------|--------------|-------------------------------|-------------------------------------------------------------------|
| **Random Forest**    | `Cutting(kN)`, `Torque(Nm)`, `Pressure_Diff`, `Hydraulic_Pressure(bar)`, `Coolant_Pressure(bar)`, `Pressure_Bin` | 0.992        | 0.992        | [[245, 2], [2, 251]]          | High precision and recall, robust against overfitting.           |
| **Gradient Boosting**| `Hydraulic_Pressure(bar)`, `Torque(Nm)`, `Cutting(kN)`, `Coolant_Pressure(bar)`, `Spindle_Speed(RPM)`, `Pressure_Diff` | 0.99         | 0.99         | [[244, 3], [2, 251]]          | Comparable to Random Forest, but more resource-intensive.        |
| **Logistic Regression** | `Pressure_Temp_Interaction`, `Spindle_Speed(RPM)`, `Hydraulic_Pressure(bar)`, `Pressure_Diff`, `Pressure_Change`, `Torque_Speed` | 0.808        | 0.808        | [[199, 3], [48, 205]]         | Interpretable coefficients, good for understanding feature contributions. |

## 3. Overall vs. Machine-Specific Models

### Overall Model

| **Metric**          | **Value** |
|----------------------|-----------|
| **F1 Score**         | 0.984     |
| **Accuracy**         | 0.984     |
| **Confusion Matrix** | [[242, 5], [3, 250]] |

- **Insights**: The overall model achieved strong performance, suggesting it generalizes well across machines.

### Machine-Specific Models

#### Modeling for Machine: `0: Makino-L1-Unit1-2013`

| **Metric**          | **Value** |
|----------------------|-----------|
| **F1 Score**         | 0.9657    |
| **Accuracy**         | 0.9657    |
| **Confusion Matrix** | [[82, 2], [4, 87]] |

**Classification Report**:

| **Precision** | **Recall** | **F1-Score** | **Support** |
|---------------|------------|--------------|-------------|
| 0.95          | 0.98       | 0.96         | 84          |
| 0.98          | 0.96       | 0.97         | 91          |

- **Accuracy**: 0.97  
- **Macro Avg**: Precision = 0.97, Recall = 0.97, F1 = 0.97  
- **Weighted Avg**: Precision = 0.97, Recall = 0.97, F1 = 0.97  



#### Modeling for Machine: `2: Makino-L3-Unit1-2015`

| **Metric**          | **Value** |
|----------------------|-----------|
| **F1 Score**         | 0.9817    |
| **Accuracy**         | 0.9817    |
| **Confusion Matrix** | [[80, 1], [2, 81]] |

**Classification Report**:

| **Precision** | **Recall** | **F1-Score** | **Support** |
|---------------|------------|--------------|-------------|
| 0.98          | 0.99       | 0.98         | 81          |
| 0.99          | 0.98       | 0.98         | 83          |

- **Accuracy**: 0.98  
- **Macro Avg**: Precision = 0.98, Recall = 0.98, F1 = 0.98  
- **Weighted Avg**: Precision = 0.98, Recall = 0.98, F1 = 0.98  



#### Modeling for Machine: `1: Makino-L2-Unit1-2015`

| **Metric**          | **Value** |
|----------------------|-----------|
| **F1 Score**         | 0.9567    |
| **Accuracy**         | 0.9568    |
| **Confusion Matrix** | [[83, 0], [7, 72]] |

**Classification Report**:

| **Precision** | **Recall** | **F1-Score** | **Support** |
|---------------|------------|--------------|-------------|
| 0.92          | 1.00       | 0.96         | 83          |
| 1.00          | 0.91       | 0.95         | 79          |

- **Accuracy**: 0.96  
- **Macro Avg**: Precision = 0.96, Recall = 0.96, F1 = 0.96  
- **Weighted Avg**: Precision = 0.96, Recall = 0.96, F1 = 0.96  

### Comparison

- **Machine-Specific Models**: F1 scores for individual machines ranged from **0.9567** to **0.9817**.
- **Overall Model**: While the machine-specific models slightly outperformed the overall model for specific machines, the improvement was marginal, indicating the overall model is sufficient for deployment.

## 4. Key Insights

- **Important Features**: Features derived from hydraulic pressure, torque, and cutting force consistently influenced model performance, underscoring their relevance in predicting downtime.
- **Model Choice**: Random Forest is the recommended model due to its superior performance, robustness to overfitting, and ability to handle non-linear relationships.
- **Feature Engineering**: Enriching the dataset with interaction terms, lagged values, and cumulative metrics significantly improved model accuracy.

---

# General Recommendations  

### 1. Real-time Monitoring of Hydraulic Pressure  
- **Action:** Implement stricter thresholds for hydraulic pressure alerts.  
- **Rationale:** Significant deviations, especially pressure drops, should trigger early warnings to prevent potential machine failures and downtime.  

### 2. Temperature Management  
- **Action:** Actively monitor coolant and spindle bearing temperatures during high-pressure operations.  
- **Rationale:** Establishing and enforcing thresholds for temperature increases can help mitigate overheating issues, extending machine life and ensuring consistent performance.  

### 3. Predictive Maintenance  
- **Action:** Introduce predictive maintenance algorithms that incorporate hydraulic pressure, coolant temperature, and tool vibration data.  
- **Rationale:** These metrics often show significant variations before failures occur, helping predict potential machine downtime and improving maintenance schedules.

### 4. Tool Vibration Control  
- **Action:** Ensure balanced tool operation through continuous vibration monitoring.  
- **Rationale:** Elevated vibration levels can lead to increased spindle and tool wear, which ultimately shortens tool life and negatively impacts machining quality.

### 5. Torque Management  
- **Action:** Closely monitor torque levels, particularly during cutting operations.  
- **Rationale:** Preventing machine overloading through torque monitoring helps avoid performance issues and operational failures.

# Machine Learning Model Recommendations  

### 1. Separate Machine-Specific Models  
- **Action:** Train separate predictive models for each machine (e.g., *Makino-L1-Unit1-2013*, *Makino-L2-Unit1-2015*, *Makino-L3-Unit1-2015*).  
- **Rationale:** Machines operate under varying conditions, and machine-specific models adapt more effectively to these differences, leading to better predictive accuracy.  
    - **Example:** The highest F1 scores and accuracies are observed when modeling machines individually, such as *Makino-L3-Unit1-2015* (F1 score of 0.98, accuracy 0.98).

### 2. Exclusion of Machine ID in Global Models  
- **Action:** Exclude machine IDs from aggregate models used for overall system monitoring.  
- **Rationale:** Including machine IDs in global models results in a slight decrease in accuracy (0.99 to 0.98) and increases error rates. Machine IDs introduce variability without improving model performance.

### 3. Focus on Critical Features  
- **Action:** Prioritize the use of key features such as `Cutting(kN)`, `Torque(Nm)`, `Pressure_Diff`, `Hydraulic_Pressure(bar)`, `Coolant_Pressure(bar)`, and `Pressure_Bin` for predictive models.  
- **Rationale:** These features have consistently contributed to high model performance (F1 score of 0.99, accuracy 0.99) and are critical for accurate predictions.

### 4. Monitor Model Drift for Individual Machines  
- **Action:** Regularly monitor model accuracy for individual machines and recalibrate models when necessary.  
- **Rationale:** Machine-specific models may degrade over time due to changes in machine performance or environmental factors. Consistent monitoring and recalibration help maintain predictive accuracy.

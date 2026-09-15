# Results and Analysis

## 5.1 Normal Equation Parameters

The normal equation was used to find the parameters for Feature Set A.

The obtained parameter vector was:

```text
theta = [6883.4166, 8258.2965, -25.5450, -15.9294, -29.8326, -408.7725]
```

The parameters are:

```text
theta0 = 6883.4166
theta1 = 8258.2965
theta2 = -25.5450
theta3 = -15.9294
theta4 = -29.8326
theta5 = -408.7725
```

The largest weight is `theta1 = 8258.2965`, which corresponds to **irradiation**. This shows that irradiation has the strongest effect on the predicted AC power among the input features in Set A.

The negative weights for module temperature, ambient temperature, and the cosine time feature indicate a negative relationship with the prediction while keeping the other standardized features fixed.

---

## 5.2 Feature Set A vs Feature Set B

Feature Set A uses the on-site sensor measurements:

- Irradiation
- Module temperature
- Ambient temperature
- Sin hour
- Cos hour

Feature Set B uses public weather measurements:

- Open-Meteo shortwave radiation
- Temperature at 2 m
- Cloud cover
- Sin hour
- Cos hour

The test RMSE results using the Normal Equation were:

| Feature Set | Test RMSE |
|---|---:|
| Set A | 553.32 kW |
| Set B | 2626.13 kW |

The difference in RMSE was:

```text
2626.13 - 553.32 = 2072.81 kW
```

Set A performed much better than Set B. The Set B RMSE was about **374.61% higher than Set A**.

For daytime data only, the results were:

| Feature Set | Daytime RMSE |
|---|---:|
| Set A | 723.41 kW |
| Set B | 3417.65 kW |

This also shows that the on-site sensor measurements gave much better predictions than the public weather data for this dataset.

---

## 5.3 Batch Gradient Descent Learning Rates

Batch Gradient Descent was tested with learning rates:

```text
alpha = 1e-5
alpha = 1e-4
alpha = 1e-3
```

For Set A, the results were:

| Learning rate | Final cost |
|---|---:|
| 1e-5 | 588252123.58 |
| 1e-4 | 127581018.65 |
| 1e-3 | 2.94 × 10^101 |

The learning rate `1e-3` caused the cost to become extremely large, showing that the algorithm diverged.

The learning rate `1e-4` gave a much lower final cost than `1e-5`, so `1e-4` was used for the main Batch GD comparison.

---

## 5.4 Normal Equation vs Batch Gradient Descent

For Feature Set A, the Normal Equation parameters were:

```text
[6883.4166, 8258.2965, -25.5450, -15.9294, -29.8326, -408.7725]
```

The Batch GD parameters using `alpha = 1e-4` were:

```text
[6883.4166, 5870.9224, 3115.7346, -944.5627, -32.6721, -396.5896]
```

The maximum absolute difference between the two parameter vectors was approximately:

```text
3141.28
```

For Feature Set B, the maximum absolute difference was approximately:

```text
80.77
```

Therefore, Batch GD did not completely reach the Normal Equation solution for Set A within the 500 iterations used. Set B was much closer.

---

## 5.5 Test RMSE Comparison

The test RMSE results for the Normal Equation and Batch GD were:

| Feature Set | Normal Equation | Batch GD |
|---|---:|---:|
| Set A | 553.32 kW | 726.49 kW |
| Set B | 2626.13 kW | 2620.09 kW |

For Set A, the Normal Equation performed better than Batch GD.

For Set B, Batch GD was slightly better than the Normal Equation, but the difference was very small.

The best overall result was obtained by **Set A with the Normal Equation**, with a test RMSE of **553.32 kW**.

---

## 5.6 Daytime RMSE

The assignment also required evaluating the model only during daytime, where irradiation is greater than zero.

The results were:

| Feature Set | Method | Daytime RMSE |
|---|---|---:|
| Set A | Normal Equation | 723.41 kW |
| Set A | Batch GD | 943.92 kW |
| Set B | Normal Equation | 3417.65 kW |
| Set B | Batch GD | 3410.50 kW |

The daytime results follow the same general trend as the all-hours results. Set A is much more accurate than Set B.

The best daytime result was again **Set A with the Normal Equation**, with an RMSE of **723.41 kW**.

---

## 5.7 Batch GD and SGD Convergence

For Set A using `alpha = 1e-4`, Batch GD and SGD were compared.

### Batch GD

```text
Initial cost = 27824120227.21
Final cost   = 127581018.65
Cost reduction = 27696539208.56
```

### SGD

```text
Initial cost = 28641344189.46
Final cost   = 590275677.27
Cost reduction = 28051068512.19
```

Both methods reduced the cost substantially.

Batch GD reached a lower final cost than SGD:

```text
Batch GD final cost = 127581018.65
SGD final cost      = 590275677.27
```

Therefore, for these settings, Batch GD converged closer to the lower-cost solution.

---

## 5.8 Residual Analysis by Hour

The residual was calculated as:

```text
residual = actual AC power - predicted AC power
```

The residuals were zero during the night for several hours because the actual and predicted AC power were both zero.

During the early morning, the mean residual was mostly negative. For example:

```text
Hour 4:  -67.92 kW
Hour 5: -205.34 kW
Hour 6: -322.42 kW
```

A negative residual means that the prediction was higher than the actual value.

During much of the daytime, the mean residual became positive:

```text
Hour 8:  473.21 kW
Hour 9:  806.18 kW
Hour 10: 1168.15 kW
Hour 11: 772.43 kW
Hour 12: 545.24 kW
```

Positive residuals mean that the model generally predicted less power than the actual power.

In the evening, the residual became negative again:

```text
Hour 17: -177.18 kW
Hour 18: -391.55 kW
Hour 19: -271.20 kW
Hour 20: -130.53 kW
```

This shows that the prediction error changes with the hour of the day rather than being randomly distributed around zero.

---

## 5.9 Negative Prediction Check

The assignment required negative predictions to be clipped to zero.

The number of negative predictions before clipping was:

| Model | Negative predictions |
|---|---:|
| Set A — Normal Equation | 0 |
| Set A — Batch GD | 0 |
| Set B — Normal Equation | 0 |
| Set B — Batch GD | 0 |

Therefore, no negative predictions occurred in the test results. The clipping operation was still included as required, but it did not change the predictions.

---

# Overall Analysis

The results show that the choice of input data had a large effect on prediction accuracy. Feature Set A, which uses the on-site irradiation and temperature measurements, performed much better than Feature Set B, which uses public Open-Meteo weather data.

The best overall model was the **Normal Equation with Feature Set A**, giving a test RMSE of **553.32 kW**. Its daytime RMSE was **723.41 kW**.

The largest parameter in the Set A normal-equation model was the irradiation coefficient:

```text
theta1 = 8258.30
```

This is consistent with the fact that solar irradiation is strongly related to solar power generation.

Batch Gradient Descent also reduced the cost substantially, but with 500 iterations and `alpha = 1e-4`, its parameters for Set A did not completely match the Normal Equation solution. Increasing the learning rate to `1e-3` caused divergence, while `1e-4` gave a useful reduction in cost.

SGD also reduced the cost, but its final cost was higher than Batch GD for the tested settings.

The residual analysis shows that the model error depends on the time of day. The model tends to overpredict during some early morning and evening hours and underpredict during much of the daytime. This suggests that the simple linear model does not completely capture the relationship between solar weather conditions and AC power throughout the day.

Overall, the results show that **local on-site sensor data provided much better prediction accuracy than the public weather data for this solar plant dataset**.

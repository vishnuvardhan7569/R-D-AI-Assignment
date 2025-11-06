# Parametric Curve Fitting – R&D / AI Assignment

This repository estimates the unknown parameters **θ**, **M**, and **X** for a given parametric curve using a dataset of (x, y) points. The fitted curve closely matches the provided data across the range **t ∈ (6, 60)**.

---

## 📌 Problem

**Model equations:**

\[
x(t) = t\cos(\theta) - e^{M|t|}\sin(0.3t)\sin(\theta) + X
\]

\[
y(t) = 42 + t\sin(\theta) + e^{M|t|}\sin(0.3t)\cos(\theta)
\]

**Unknowns and bounds:**  
- \( 0^\circ < \theta < 50^\circ \)  
- \( -0.05 < M < 0.05 \)  
- \( 0 < X < 100 \)  
- \( 6 < t < 60 \) (data range)

---

## ✅ Final Estimated Parameters

| Parameter | Value |
|-----------|------:|
| **θ (theta)** | `0.523599` radians (`30.0000°`) |
| **M** | `0.030000` |
| **X** | `55.000000` |

---

## 📍 Final Parametric Equations (Substituted)

```
x(t) = t·cos(0.523599) - exp(0.030000·|t|)·sin(0.3t)·sin(0.523599) + 55.000000
y(t) = 42 + t·sin(0.523599) + exp(0.030000·|t|)·sin(0.3t)·cos(0.523599)
```



## 🧠 Approach (Brief)

1. **Rotate & Translate**: Convert (x, y) → (t, r) using:
   - \( t = (x-X)\cos\theta + (y-42)\sin\theta \)
   - \( r_{obs} = -(x-X)\sin\theta + (y-42)\cos\theta \)

2. **Closed-form M**: From \( r(t) = e^{M|t|}\sin(0.3t) \Rightarrow M_i = \frac{\ln(r/\sin(0.3t))}{|t|} \).  
   Use the **median** across valid samples and clamp to bounds.

3. **Coarse Search**: Grid over \( \theta \in (0, 50^\circ),\; X \in (0, 100) \); score by MAE in r-space.

4. **Local Refinement**: Hill-climb around coarse best to reduce MAE further.

5. **Reconstruct & Score**: Rebuild (x, y) and compute **L1** and **RMSE** in XY-space.

---

## 📊 Evaluation

- **L1** and **RMSE** computed in XY-space (lower is better).  
- **Residuals vs t** show no systematic pattern → indicates a good fit.

> The fitted curve nearly overlaps the dataset across the full t-range.

---

## 🖼️ Visuals

Add these to `results/` (placeholders shown):
- `results/original_data.png` – Original data
- `results/predicted_curve.png` – Predicted curve only
- `results/overlay.png` – Data vs Predicted overlay
- `results/residuals_vs_t.png` – Residual magnitude vs t
- `results/final_plot_light.png` – 2×2 light theme
- `results/final_plot_dark.png` – 2×2 dark theme

---

## ▶️ How to Run

**Local (Python):**
```bash
pip install numpy pandas matplotlib
python src/curve_fit.py
```

**Colab:**
- Upload `xy_data.csv`
- Run cells for Steps 1–9
- Use the plotting cell to generate visuals

---

## 📂 Repository Structure

```
├── data/
│   └── xy_data.csv
├── src/
│   └── R&D / AI Assignment.py
├── results/
│   ├── original_data.png
│   ├── predicted_curve.png
│   ├── overlay.png
│   ├── residuals_vs_t.png
│   ├── final_plot_light.png
│   └── final_plot_dark.png
└── README.md
```

---

## 🧾 Notes

- Bounds respected: \( 0^\circ<\theta<50^\circ,\; -0.05<M<0.05,\; 0<X<100 \).
- Data used only for \( 6<t<60 \) as specified.
- The solution avoids black-box regression and exploits model structure for precise recovery.

---

## 📜 License

For academic and research learning purposes.

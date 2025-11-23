# Results Summary – Skeleton-Based Human Action Recognition

This paper gives an overview of the most important experimental findings from my master's thesis, which looked at skeleton-based Human Action Recognition (HAR) using Graph Neural Networks (GNNs) on the NTU RGB+D 60 dataset (Cross-Subject and Cross-View protocols).

## 1. Dataset & Setup

- **Dataset:** NTU RGB+D 60 (skeleton modality)
- **Protocols:** Cross-Subject (X-Sub), Cross-View (X-View)
- **Hardware:** Single GPU (V100, Google Colab environment)
- **Models evaluated:**
  - ST-GCN (baseline family)
  - AGCN (2-stream)
  - CTR-GCN
  - ShiftGCN (efficient architecture)
- **Metrics:**
  - Top-1 accuracy (X-Sub, X-View)
  - Confusion matrix analysis
  - GPU memory usage and training behaviour

> Note: All experiments are designed for **single-GPU training**, which naturally
> leads to slightly smaller batch sizes than multi-GPU papers and a small loss in
> final accuracy (~1.4–3%).

---

## 2. Main Accuracy Results

### 2.1 Overall Model Comparison

| Model                     | Batch Size | X-Sub Accuracy | X-View Accuracy | Notes                               |
|---------------------------|-----------:|---------------:|----------------:|-------------------------------------|
| **AGCN (2-Stream)**       | 64         | **88.14%**     | **88.27%**      | Best overall accuracy               |
| **ShiftGCN**              | 64         | 83.18%         | 84.87%          | Very efficient, low FLOPs           |
| **ShiftGCN (improved)**   | 72         | 86.34%         | 87.14%          | Strong trade-off acc. vs efficiency |
| **CTR-GCN (Lion opt.)**   | 64         | 81.52%         | 84.15%          | Fast convergence, higher memory     |

**Key observations:**

- **AGCN (2-stream)** achieved the **highest accuracy** on both X-Sub and X-View.
- **ShiftGCN** provided the **best efficiency** (very lightweight, low FLOPs),
  reaching competitive accuracy especially with batch size 72.
- **CTR-GCN** showed good performance but required **more GPU memory** due to
  channel-wise topology refinement.
- Compared to multi-GPU benchmarks in the literature, the accuracy drop with
  single-GPU training remained small (approximately 1.4–3%), mainly due to
  reduced batch sizes and hardware limits.

---

## 3. Efficiency & Deployment Insights

- **ShiftGCN** is the most suitable architecture for **real-time or edge
  deployment**, thanks to its low computational cost.
- **AGCN** is the best choice when **maximum accuracy** is the priority and
  slightly higher cost is acceptable.
- **CTR-GCN** offers a balance but is more memory-hungry, which matters on
  1-GPU systems.

---

## 4. Confusion Matrix & Error Patterns

Confusion matrices (notebook visualizations) show that:

- Most misclassifications happen between **similar motion classes** (e.g.,
  actions with overlapping arm or torso movements).
- AGCN and CTR-GCN reduce confusion in complex classes compared to baseline
  ST-GCN.
- ShiftGCN performs particularly well on **dynamic, large-motion actions** but
  can struggle slightly more with very subtle actions.

(Example confusion matrix figures are generated in the notebooks and can be
exported as `results/confusion_matrix_*.png` if needed.)

---

## 5. Takeaways for Future Work

- **AGCN** is a strong starting point for **healthcare-oriented HAR**, where
  accuracy and robustness are critical.
- **ShiftGCN** is ideal for **wearable / edge / real-time** health monitoring
  systems.
- A **hybrid design** combining AGCN’s adaptivity with ShiftGCN’s efficiency is
  a promising future direction.
- These results provide a solid baseline for extending the project towards:
  - Multimodal HAR (skeleton + RGB + depth + sensors)
  - Healthcare applications (behaviour monitoring, fall/risk detection,
    cognitive decline indicators)
  - Self-supervised and explainable models suitable for clinical use.

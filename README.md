# Intro to Federated Learning

**Course notebooks from DeepLearning.AI (in collaboration with Flower Labs)**  

This repository contains the notebooks used in the **Intro to Federated Learning** course.  

> **Acknowledgements**: The course content and notebooks were created and are licensed/managed by **DeepLearning.AI** in collaboration with **Flower Labs**.  
> This repo republishes those notebooks for learning purposes; the license is managed by DeepLearning.AI.

---

## 📘 Course Topics & Explanations

This section summarizes the five main topics covered in the course. If you are a learner browsing this repo, you can use these notes to understand each concept before diving into the notebooks.

---

### 1. Why Federated Learning

Federated Learning (FL) is a distributed ML approach where many clients (phones, hospitals, IoT devices) collaboratively train a shared model while keeping **raw data local**. Only model updates (weights/gradients) are sent to a central server.

**Why it matters:**
- 🔒 **Privacy**: Sensitive data stays on-device (GDPR, HIPAA, etc.).
- 📍 **Data locality**: Useful where data naturally lives at the edge.
- 🎯 **Personalization**: Local fine-tuning adapts to user behavior.
- 💾 **Reduced central storage**: Less raw data to manage centrally.
- ⚡ **Low latency**: Local inference and updates.

**Examples:**
- Predictive text keyboards trained on smartphones.
- Hospitals collaborating on diagnostic models without sharing patient data.

**Key trade-offs:**
- Privacy isn’t automatic — model updates can leak info.
- Clients often have **non-IID** data distributions.
- Communication overhead can be huge.
- More complex system design is needed.

---

### 2. Federated Learning Process

A typical **round-based FL workflow**:

1. Server initializes global model.
2. Server selects clients for this round.
3. Clients download global model.
4. Clients train locally on private data.
5. Clients send updates to the server.
6. Server aggregates updates (e.g., averaging).
7. Repeat for multiple rounds.

**Variants:**
- **Cross-device**: Many small clients (e.g., smartphones).
- **Cross-silo**: Few large, stable clients (e.g., hospitals).
- **Synchronous vs. Asynchronous**: Whether updates wait for all clients or not.

**Aggregation:**
- **FedAvg**: Weighted averaging of client updates.
- **FedProx, FedOpt**: Variants for better handling of heterogeneity.

**Key considerations:**
- Client selection and participation rate.
- Local epochs and batch size.
- Robustness to dropouts and stragglers.

---

### 3. Tuning

Tuning in FL is different because **hyperparameters interact with system constraints**.

**Important hyperparameters:**
- **Local epochs (E)**: More epochs = fewer communications, but risk divergence on non-IID data.
- **Client fraction (C)**: More clients = better generalization, but higher bandwidth.
- **Learning rates**: Both local and server learning rates affect stability.
- **Batch size**: Affects training time and variance.

**Tips:**
- Start with **small E** (1–5) for heterogeneous data.
- Use small learning rates to prevent divergence.
- Try adaptive optimizers (e.g., FedAdam) if convergence is slow.
- Track **per-client performance**, not just global averages.

**Common solutions for non-IID data:**
- **FedProx**: Penalizes local updates that deviate too much.
- **Personalization**: Local fine-tuning after global training.
- **Re-weighting or augmentation** to reduce skew effects.

---

### 4. Data Privacy

FL improves privacy, but **isn’t private by default**. Model updates can still leak sensitive data.

**Possible attacks:**
- Model inversion (reconstruct data).
- Membership inference (detect if data was used).
- Gradient leakage (recover samples).
- Malicious clients (data/model poisoning).

**Defenses:**
- **Secure Aggregation**: Server only sees aggregated updates.
- **Differential Privacy (DP)**:
  - Add noise + clip gradients.
  - Provides formal guarantees (ε, δ).
- **Robust aggregation**: Median or trimmed mean to resist poisoned updates.
- **Cryptography (MPC, HE)**: Strong protection but heavy cost.
- **Trusted Execution Environments (TEE)**: Hardware-based isolation.

**Trade-offs:**
- Stronger privacy usually reduces model accuracy.
- Secure aggregation can limit debugging/personalization.
- DP requires careful accounting of the total privacy budget.

---

### 5. Bandwidth

In FL, **communication is the bottleneck** — models and updates are large, and many clients must send them.

**Where the cost comes from:**
- Model size × number of clients × number of rounds.

**Example calculation:**
- Model = 1,000,000 parameters.
- Each parameter = 4 bytes (float32).
- Model size = 4 MB.
- 100 clients upload per round = **400 MB per round**.

**Techniques to reduce cost:**
- **Quantization**: Fewer bits per parameter.
- **Sparsification (Top-k)**: Send only the largest updates.
- **Delta compression**: Send only changes since last update.
- **Fewer communication rounds**: More local epochs per round.
- **Smaller models**: Architectures designed for edge devices.
- **Opportunistic updates**: Use connectivity when available.

**Trade-off:**  
Less communication can slow convergence, especially on **non-IID data**.

---

## 🧑‍💻 How to Learn from This Repo

- Each notebook corresponds to one or more of the topics above.  
- **Start with “Why Federated Learning”**, then move to the **Process** notebook.  
- Once comfortable, experiment with **Tuning**, and finally study **Privacy** and **Bandwidth**.  
- **Try altering hyperparameters** (`E`, client fraction, learning rate) and observe results.  
- Experiment with **non-IID client datasets** — this is where FL challenges become clear.  

---

## 📑 Cheatsheet: Quick Reference

### 🔧 Hyperparameters
| Parameter | Effect | Trade-off |
|-----------|--------|-----------|
| Local Epochs (E) | More local training per round | ↓ Communication, ↑ Divergence risk |
| Client Fraction (C) | More clients participate each round | ↑ Accuracy, ↑ Bandwidth |
| Local LR | Controls stability of updates | Too high = divergence |
| Server LR | Smooths aggregation | Helps convergence tuning |
| Batch Size | Affects compute + noise | Small = noisy, Large = slower |

---

### 🔒 Privacy Defenses
| Technique | Purpose | Trade-off |
|-----------|---------|-----------|
| Secure Aggregation | Server sees only aggregated updates | Harder debugging |
| Differential Privacy | Limits info leakage per client | Accuracy drop |
| Robust Aggregation | Protects against poisoned updates | May slow convergence |
| MPC / HE | Cryptographic guarantees | Very high overhead |
| TEEs | Hardware isolation | Trust in hardware vendor |

---

### 📡 Bandwidth Optimization
| Technique | Purpose | Trade-off |
|-----------|---------|-----------|
| Quantization | Reduce bits per weight | Accuracy drop if aggressive |
| Sparsification | Send only top-k updates | Slower convergence |
| Delta Compression | Send differences only | Needs extra memory |
| Fewer Rounds | Increase local epochs | Divergence on non-IID |
| Smaller Models | Reduce transfer size | Lower capacity |
| Opportunistic Updates | Use network when available | Updates may be delayed |

---

## 🙏 Acknowledgements

This course and its materials were created by **DeepLearning.AI** in collaboration with **Flower Labs**.  
The license for the course content and notebooks is managed by **DeepLearning.AI**.  

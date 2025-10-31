# 🤖 Non-Linear Model Predictive Control (MPC) for Mobile Manipulator

## 🌟 Overview

This project presents the modeling and implementation of a **Non-Linear Model Predictive Control (NMPC)** system for a mobile manipulator (mobile base + robotic arm) with explicit **anti-collision constraints**. The controller is designed to achieve a target position while dynamically avoiding multiple static obstacles.

This work serves as a high-level demonstrator of non-linear control and optimization techniques.

---

## ✨ Key Features & Technical Stack

| Category | Technology / Technique | Details & Contribution |
| :--- | :--- | :--- |
| **Control Algorithm** | **Model Predictive Control (MPC)** | Implemented NMPC to solve an optimal control problem at each time step. |
| **Optimization Tool** | **`do-mpc` and `CasADi`** | Utilized `do-mpc` for the NMPC framework, relying on **CasADi** for symbolic differentiation and efficient optimization problem formulation. |
| **Modeling** | **Non-Linear Dynamics & Kinematics** | Full modeling of the mobile manipulator's state variables (position, orientation, joint angles). |
| **Constraint Handling** | **Anti-Collision Constraints** | Explicit modeling of collision avoidance by ensuring the robot's links stay outside defined circular safety zones. |
| **Cost Functions** | **$J_1$ (Standard) vs $J_2$ (Path-focused)** | Comparative analysis of two different cost functions regarding speed, precision, and efficiency in obstacle-dense environments. |

---

## 🧠 Methodology and Modeling

### 1. System State and Control Inputs

The system state vector $\mathbf{x}$ and the control input vector $\mathbf{u}$ were defined for the non-linear discrete-time model.

* **State $\mathbf{x}$:** Includes the pose of the mobile base and the joint positions of the manipulator (e.g., $[x, y, \theta, q_1, q_2, \dots]^T$).
* **Input $\mathbf{u}$:** Control velocities (e.g., $[\dot{x}, \dot{y}, \dot{\theta}, \dot{q}_1, \dots]^T$).

### 2. Anti-Collision Constraints

Collision avoidance is implemented by projecting the manipulator's links (modeled as specific points $P_0, P_1, \dots$) and ensuring their distance from the center of static circular obstacles ($O_i$) remains above a safe margin $R_{\text{safety}}$.

$$||\mathbf{P}_j(\mathbf{x}) - \mathbf{O}_i||_2 \geq R_{\text{safety}}$$

### 3. Cost Function Analysis

The project compares two objective functions ($J_1$ and $J_2$), both aiming to minimize the distance to the target and the energy of the control inputs, but differing in how they handle path smoothing and constraint violation weighting.

* **Key Finding (from the M2 Report):**
    * $J_2$ converges faster in paths requiring **significant obstacle avoidance**.
    * $J_1$ is **more efficient and faster** for shorter paths **without complex obstacle maneuvers** and tends to be more precise overall.

---

## 📊 Trajectory Results (Obstacle Avoidance Scenario)

The implementation successfully demonstrates the MPC's ability to plan a collision-free path by dynamically adjusting the control inputs over the prediction horizon $N_p$.

The image below illustrates the path planned by the MPC for the manipulator's links, navigating around four distinct circular obstacles.



**Visualization Note:** The dashed lines represent the anti-collision constraint circles. The colored solid lines represent the planned trajectory of the mobile manipulator's specific links.

---

## ⚙️ Installation and Setup

The project is fully executable using the provided Jupyter Notebook (`MPC.ipynb`), ideally run on an environment that can handle dynamic plotting.

### Prerequisites
* Python 3.x
* The `do-mpc` library (which includes `CasADi`).
* Standard scientific packages (`numpy`, `matplotlib`).

### Steps
1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/xavierplt/](https://github.com/xavierplt/)[Model-Predictive-Control].git
    cd [Model-Predictive-Control]
    ```
2.  **Install Dependencies:**
    ```bash
    pip install do-mpc casadi numpy matplotlib
    ```
3.  **Run the Notebook:**
    Open the `MPC.ipynb` file (e.g., using Jupyter Lab or Google Colab) and execute all cells sequentially to reproduce the modeling, the NMPC setup, and the simulation results.

---

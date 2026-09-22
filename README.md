# Network Optimization and Non-linear Models for Azure Paradise Resort

This repository contains an end-to-end mathematical modeling and optimization solution for replacing motorbikes with autonomous delivery drones at a luxury resort. 

The project evaluates two distinct operational components: a **Capacitated Vehicle Routing Problem (CVRP)** for optimal route allocation under battery and capacity constraints, and a **Non-linear Energy Model** to optimize drone cruise speed and power consumption.

---

## 📋 Problem Overview

"Azure Paradise Resort" features **20 exclusive bungalows** spread across a 10 km² private island. To eliminate noise, lower maintenance costs, and reduce carbon emissions, the resort is replacing motorbikes with **5 autonomous delivery drones** dispatched from the Central Kitchen $(5000, 5000)$ at 08:30 AM.

* **Global Capacity:** Maximum payload of 15 breakfasts per drone.
* **Battery Autonomy Limits:** Maximum range of 15 km ($15,000\text{ meters}$) per drone route.
* **Delivery Logistics:** Synchronized batch dispatch to deliver hot meals simultaneously.
* **Non-linear Power Dynamics:** Flight power draw increases non-linearly with payload weight and aerodynamic drag at higher speeds.

---

## 🛠️ Methodological Approach

### 1. Model A: Network Optimization (CVRP)
* **Objective:** Minimize total cumulative travel distance across all operational drone routes while covering all 20 bungalows.
* **Formulation:** Integer Linear Programming with Miller-Tucker-Zemlin (MTZ) subtour elimination constraints, flow balance, drone capacity, and battery autonomy limits.
* **Key Insight:** Assigns tight, spatially clustered loops originating and ending at the Central Kitchen, ensuring no drone exceeds its 15 km battery autonomy.

### 2. Model B: Non-linear Energy Optimization
* **Objective:** Optimize cruise speed ($v_k$) and flight time per drone to minimize total energy consumption (in Watt-hours) under route distance and payload constraints derived from Model A.
* **Formulation:** Non-linear objective function balancing aerodynamic drag ($\propto v^3$), payload lift power, and baseline electronics draw.
* **Key Insight:** Optimizes drone velocity to operate at the most efficient speed point, satisfying strict arrival time windows ($T_{\max}$) while avoiding excessive energy depletion.

---

## 📐 Mathematical Models

### Model A Formulation (CVRP)

#### Sets and Indices
* $V = \{0, 1, 2, \dots, 20\}$: Set of all nodes (0: Central Kitchen, 1–20: Bungalows).
* $V_c = \{1, 2, \dots, 20\}$: Set of customer bungalows.
* $K = \{1, 2, 3, 4, 5\}$: Set of available autonomous delivery drones.

#### Objective Function
$$\min \sum_{k \in K} \sum_{i \in V} \sum_{j \in V, j \neq i} c_{ij} x_{ijk}$$

#### Constraints
1. **Customer Coverage:** Each bungalow $i \in V_c$ must be visited exactly once by a drone:
   $$\sum_{k \in K} \sum_{j \in V, j \neq i} x_{ijk} = 1 \quad \forall i \in V_c$$

2. **Flow Balance:** A drone entering node $i$ must also leave node $i$:
   $$\sum_{j \in V, j \neq i} x_{jik} - \sum_{j \in V, j \neq i} x_{ijk} = 0 \quad \forall i \in V, \forall k \in K$$

3. **Depot Dispatch Limit:** At most $m = 5$ drones can depart from the Central Kitchen ($0$):
   $$\sum_{k \in K} \sum_{j \in V_c} x_{0jk} \le m$$

4. **Capacity Limit:** Total demand delivered on any route $k$ cannot exceed drone capacity $Q = 15$:
   $$\sum_{i \in V_c} d_i \left( \sum_{j \in V, j \neq i} x_{ijk} \right) \le Q \quad \forall k \in K$$

5. **Battery Autonomy Limit:** Total distance traveled by drone $k$ cannot exceed maximum autonomy $L = 15,000\text{ meters}$:
   $$\sum_{i \in V} \sum_{j \in V, j \neq i} c_{ij} x_{ijk} \le L \quad \forall k \in K$$

6. **Subtour Elimination (MTZ Constraints):** Prevents isolated loops disconnected from the Central Kitchen:
   $$u_{ik} - u_{jk} + Q x_{ijk} \le Q - d_j \quad \forall i, j \in V_c, i \neq j, \forall k \in K$$
   $$d_i \le u_{ik} \le Q \quad \forall i \in V_c, \forall k \in K$$

---

### Model B Formulation (Non-linear Power Consumption)

#### Decision Variables
* $v_k > 0$: Cruise speed of drone $k$ (in $\text{m/s}$).
* $P_k$: Non-linear power consumption rate of drone $k$ (in Watts).
* $T_k$: Total flight time for route $k$ (in seconds).

#### Objective Function
Minimize total non-linear energy consumption across all operational routes $k \in K$:
$$\min \sum_{k \in K} D_k \left( \alpha \cdot v_k^2 + \beta \cdot (m_0 + w_k) + \frac{\gamma}{v_k} \right)$$

#### Constraints
1. **Speed Bounds:**
   $$v_{\min} \le v_k \le v_{\max} \quad \forall k \in K$$

2. **Delivery Time Window (Breakfast Temperature Guarantee):**
   $$\frac{D_k}{v_k} \le T_{\max} \quad \forall k \in K$$

3. **Maximum Battery Energy Reserve Limit:**
   $$E_k(v_k, w_k) \le E_{\max} \quad \forall k \in K$$

---

## 💻 Tech Stack & Tools

* **Language:** Python 3.x
* **Modeling Framework:** Pyomo (`pyo.ConcreteModel`) / SciPy Optimize
* **Solvers:** CBC / GLPK (for CVRP) & IPOPT / SLSQP (for Non-linear Optimization)
* **Visualization:** Matplotlib & NumPy
* **Environment:** Jupyter Notebook

---

## 👩‍💻 Authors
* **Carla Aranda** 
* **Marina Juzgado** 

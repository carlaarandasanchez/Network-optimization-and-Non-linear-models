{\rtf1\ansi\ansicpg1252\deff0\nouicompat{\fonttbl{\f0\fnil\fcharset0 Courier New;}{\f1\fnil\fcharset0 Arial;}}
{\colortbl ;\red0\green0\blue0;\red0\green0\blue255;}
{\*\generator Riched20 10.0.19041}\viewkind4\uc1 
\f0\fs20\lang1033 # Autonomous Drone Delivery Network & Speed Optimization with Pyomo\par
\par
This repository contains an end-to-end mathematical modeling and optimization framework for the delivery logistics of **Azure Paradise Resort**. \par
\par
The project evaluates two distinct operational challenges: **Part A (Network Optimization - CVRP)** to determine the optimal spatial routing for a drone fleet, and **Part B (Non-Linear Optimization)** to optimize flight speed profiles balancing delivery times and battery power consumption.\par
\par
---\par
\par
## \u55357?\u56515? Problem Overview\par
\par
The Azure Paradise Resort covers an area of 10 km\u178?. The resort management replaced ground motorbikes with **5 autonomous drones** to deliver breakfasts from a Central Kitchen (located at coordinate (0,0)) to **20 bungalows** starting at 08:30 AM. The fleet operation adheres to strict constraints:\par
\par
*   **Fleet Size:** 5 autonomous drones operating simultaneously.\par
*   **Payload Capacity:** Maximum of 15 breakfast packages per drone trip.\par
*   **Autonomy / Battery Range:** Maximum routing distance of 15,000 meters (15 km) per drone.\par
*   **Demand Limits:** Exactly 1 breakfast required per bungalow (20 total deliveries).\par
*   **Objective:** Minimize total flight distance (Part A) and balance cruise time versus battery consumption (Part B).\par
\par
---\par
\par
## \u55355?\u56752? Methodological Approach\par
\par
### 1. Part A: Network Optimization (Capacitated Vehicle Routing Problem - CVRP)\par
* **Objective:** Minimize the total distance traveled across all 5 drones while ensuring full delivery coverage, strict capacity bounds, and range constraints.\par
* **Formulation:** Integer Linear Programming (ILP) using Miller-Tucker-Zemlin (MTZ) constraints for subtour elimination.\par
* **Mathematical Model:**\par
  $$\\min \\sum_\{k=1\}\^\{5\} \\sum_\{i=0\}\^\{20\} \\sum_\{j=0\}\^\{20\} d_\{ij\} \\cdot x_\{ijk\}$$
\par
  Subject to single-visit constraints, flow conservation, payload capacity (\\le 15 units), battery autonomy (\\le 15,000 m), and MTZ subtour elimination.\par
* **Key Insight:** Computes optimal, non-overlapping spatial routes that balance delivery loads efficiently without exceeding maximum drone range.\par
\par
### 2. Part B: Non-Linear Speed & Energy Optimization\par
* **Objective:** Optimize cruise speed $v_k$ for each drone along its route to manage aerodynamic power consumption $P(v)$ and total flight duration $t_k$.\par
* **Aerodynamic Model:** \par
  $$P(v) = c_1 \\cdot v\^3 + \\frac\{c_2\}\{v\} + c_3$$
\par
  Accounting for parasitic drag ($c_1 v\^3$), induced power for lift ($c_2 / v$), and avionics consumption ($c_3$).\par
* **Multi-Objective Trade-off:** \par
  $$\\min_\{v_k\} f(v_k) = \\alpha \\cdot t_k + \\beta \\cdot (P(v_k) \\cdot t_k)$$
\par
* **Key Insight:** Finds the optimal cruising speed ($v_k$) that delivers breakfasts hot before 08:30 AM while staying safely within the total battery energy capacity ($E_\{max\}$).\par
\par
--- \par
\par
## \u55357?\u56407? Tech Stack & Tools\par
\par
* **Language:** Python 3.x\par
* **Optimization Frameworks:** Pyomo (`pyo.ConcreteModel`) / PuLP\par
* **Solvers:** CBC / GLPK (Linear & Integer) and Ipopt / SciPy (Non-Linear Optimization)\par
* **Data Processing & Analytics:** NumPy, SciPy\par
* **Environment:** Jupyter Notebook\par
\par
---\par
\par
## \u55357?\u56950? How to Run\par
\par
1. **Clone the repository:**\par
   ```bash\par
   git clone [https://github.com/YOUR_USERNAME/drone-routing-optimization.git](https://github.com/YOUR_USERNAME/drone-routing-optimization.git)\par
   cd drone-routing-optimization\par
   ```\par
\par
2. **Install dependencies:**\par
   ```bash\par
   pip install pyomo pulp scipy numpy matplotlib\par
   ```\par
\par
3. **Install Solvers:**\par
   * **CBC / GLPK:** `conda install -c conda-forge glpk coin-or-cbc`\par
   * **Ipopt:** `conda install -c conda-forge ipopt`\par
\par
4. **Execute the optimization pipeline:**\par
   Open `drone_logistics_optimization.ipynb` in Jupyter Notebook or VS Code and run all cells.\par
\par
---\par
\par
## \u55357?\u56433? Authors\par
* **Carla Aranda** (100523031)\par
* **Marina Juzgado** (100523023)\par
}

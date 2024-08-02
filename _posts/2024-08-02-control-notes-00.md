---
title: Setting Up an Environment
date: 2024-08-02 20:00:00 +0300
categories: [Study Notes, Control Theory and Python]
tags: [controlpy, python]     # TAG names should always be lowercase
image: assets/img/python-control.png
---

In this series, I will provide study notes and practical implementations for control systems using Python. This post guides you through setting up an environment for control systems with Python on Ubuntu. We will create a virtual environment, install the necessary packages, and configure VSCode for Python notebooks. If you're using Windows, you can install WSL and follow these instructions.

## Step 0: Install WSL and Linux Distribution (For Windows Users)

1. **Install WSL**:
   Open PowerShell as an administrator and run:
   ```bash
   wsl --install
   ```
   This installs the default Ubuntu distribution. You can specify another distribution if preferred.

2. **Update WSL**:
   Ensure your WSL is up to date:
   ```bash
   wsl --update
   ```

## Step 1: Install Python and Virtual Environment

First, ensure Python 3 and `venv` are installed. Open your terminal and run:

```bash
sudo apt update
sudo apt install python3 python3-venv
```

### Why Use a Virtual Environment?

A virtual environment is essential for Python projects due to:

1. **Isolation**: Keeps project-specific dependencies separate from the system-wide Python installation, avoiding conflicts.
2. **Dependency Management**: Maintains project-specific libraries and versions.
3. **Reproducibility**: Ensures consistent package versions across different environments.
4. **Avoiding Permission Issues**: Allows package installation without administrative rights.

## Step 2: Create and Activate a Virtual Environment

Navigate to your project directory or create a new one:

```bash
mkdir control-systems-project
cd control-systems-project
```

Create a virtual environment:

```bash
python3 -m venv venv
```

Activate the virtual environment:

```bash
source venv/bin/activate
```

Your terminal prompt should indicate that you are in the virtual environment. Keep the virtual environment active while working on your project. If you need to deactivate it, use:

```bash
deactivate
```

## Step 3: Install Required Packages

With the virtual environment activated, install the necessary Python packages:

```bash
pip install notebook numpy scipy matplotlib control
```

These packages include:

- **`notebook`**: Jupyter Notebook interface.
- **`numpy`**: Numerical computing.
- **`scipy`**: Scientific computing and signal processing.
- **`matplotlib`**: Plotting and visualization.
- **`control`**: Control systems analysis and design.

## Step 4: Set Up and Use VSCode

1. **Install VSCode**:
   Download and install Visual Studio Code from [here](https://code.visualstudio.com/).

2. Open VSCode from your WSL terminal in the project folder:

   ```bash
   code .
   ```

3. **Install Python and Jupyter Extensions**:
   - In VSCode, go to the Extensions view (`Ctrl+Shift+X`).
   - Search for and install the **Python** extension by Microsoft.
   - Search for and install the **Jupyter** extension by Microsoft.

## Step 5: Create and Run a Jupyter Notebook

1. **Create a New Jupyter Notebook**:
   - In the VSCode command palette (`Ctrl+Shift+P`), type `Jupyter: Create New Blank Notebook`.
   - Save the new notebook with a `.ipynb` extension.

2. **First Run**:
   The first time you run a cell, VSCode will prompt you to select a kernel. Choose `Python Environment` -> `venv(Python XXX) venv/bin/python`.

3. **Write and Run Your Code**:
   You can now write and run your code in the Jupyter notebook. Here’s a sample notebook to get you started:

   ```python
   import numpy as np
   import scipy.signal as signal
   import matplotlib.pyplot as plt
   import control as ctl

   # Define the system
   num = [1]
   den = [1, 2, 1]
   system = ctl.TransferFunction(num, den)

   # Step Response
   time, response = ctl.step_response(system)
   plt.plot(time, response)
   plt.title('Step Response')
   plt.xlabel('Time (s)')
   plt.ylabel('Response')
   plt.grid()
   plt.show()

   # Impulse Response
   time, response = ctl.impulse_response(system)
   plt.plot(time, response)
   plt.title('Impulse Response')
   plt.xlabel('Time (s)')
   plt.ylabel('Response')
   plt.grid()
   plt.show()

   # Bode Plot
   ctl.bode(system)
   plt.show()

   # PID Controller
   Kp = 1
   Ki = 1
   Kd = 1
   pid = ctl.TransferFunction([Kd, Kp, Ki], [1, 0])
   closed_loop_system = ctl.feedback(pid * system)

   # Step Response of the Closed-Loop System
   time, response = ctl.step_response(closed_loop_system)
   plt.plot(time, response)
   plt.title('Closed-Loop Step Response with PID Controller')
   plt.xlabel('Time (s)')
   plt.ylabel('Response')
   plt.grid()
   plt.show()

   # Simulation with arbitrary input
   t = np.linspace(0, 10, 1000)
   u = np.sin(t)
   t_out, y_out, _ = signal.lsim((num, den), U=u, T=t)

   plt.plot(t_out, y_out)
   plt.title('System Response to Sinusoidal Input')
   plt.xlabel('Time (s)')
   plt.ylabel('Response')
   plt.grid()
   plt.show()
   ```

By following these steps, you’ll have a fully set up environment on WSL with VSCode for control systems using Jupyter notebooks. This ensures a clean and manageable setup for your Python projects.

To convert Jupyter notebooks to Markdown or HTML, you can use:

```bash
jupyter nbconvert --to html notebook.ipynb
jupyter nbconvert --to markdown notebook.ipynb
```

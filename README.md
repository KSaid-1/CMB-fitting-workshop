# CMB Fitting Workshop: Cosmological Models and Large-Scale Structure

## Overview

This repository contains materials for PHYS4080 Project 3, focused on understanding how different cosmological parameters change the observable Universe and how we can use observations to reverse-engineer our cosmological model.

Our current understanding of cosmology is based on the **Flat Λ Cold Dark Matter Model (ΛCDM)**, which assumes:
- The Universe is **flat** (no intrinsic curvature, Ωk = 0)
- Contains **Cold Dark Matter** with no significant velocity, interacting only through gravity
- Contains **Dark Energy** as a cosmological constant Λ with equation of state w = -1

## Project Objectives

In this project, you will explore how seven key cosmological parameters affect observable quantities:

- **Ωcdm**: Density of Cold Dark Matter
- **Ωb**: Density of Baryonic Matter  
- **Ωk**: Curvature of the Universe
- **As**: Amplitude of fluctuations in the early Universe
- **ns**: Power-law index of fluctuations in the early Universe
- **H0**: Present-day expansion rate (Hubble constant)
- **w**: Equation of state of dark energy

Students will focus on **two parameters** and investigate how modifying them affects cosmological observables while keeping others fixed.

## Tools and Software

### Primary Software: CAMB
We use **CAMB** (Code for Anisotropies in the Microwave Background), a modern cosmological computation tool that calculates:
- CMB temperature and polarization power spectra (E and B modes)
- Background cosmological quantities (distances, sound horizons)
- Matter power spectra as functions of redshift

CAMB has been used to analyze data from the Planck satellite and virtually every major cosmological survey.

### Additional Packages
- **NumPy & SciPy**: Numerical computation
- **Jupyter**: Interactive notebook environment
- **ChainConsumer**: MCMC analysis and visualization

## Prerequisites

### Hardware Requirements
- Personal computer with internet access
- Access to SMP Teaching server (alternative option)

### Software Requirements
- Python 3.7+
- Anaconda distribution
- GCC/Gfortran 8.3.1+ (handled automatically on SMP server and via conda-forge for personal computers)

## Setup Instructions

### Prerequisites
Before starting, ensure you have:
- Git installed on your system
- Access to the workshop GitHub repository

### Option 1: Personal Computer Setup

1. **Install Anaconda**

   **macOS:**
   - Download the macOS installer from [https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution)
   - Double-click the downloaded `.pkg` file and follow the installer prompts
   - Restart your terminal or run `source ~/.bash_profile` (or `source ~/.zshrc` for zsh)
   - Verify installation: `conda --version`

   **Windows:**
   - Download the Windows installer from [https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution)
   - Run the downloaded `.exe` file as administrator
   - During installation, check "Add Anaconda to my PATH environment variable" (optional but recommended)
   - Open Command Prompt or Anaconda Prompt
   - Verify installation: `conda --version`

   **Linux:**
   - Download the Linux installer from [https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution)
   - Open terminal and navigate to download directory
   - Make installer executable and run:
     ```bash
     chmod +x Anaconda3-*-Linux-x86_64.sh
     bash Anaconda3-*-Linux-x86_64.sh
     ```
   - Follow prompts, accept license, and choose installation directory
   - When asked "Do you wish the installer to initialize Anaconda3", type "yes"
   - Restart terminal or run `source ~/.bashrc`
   - Verify installation: `conda --version`

2. **Clone Workshop Repository**
   ```bash
   git clone https://github.com/KSaid-1/CMB-fitting-workshop.git
   cd CMB-fitting-workshop
   ```

3. **Create and Configure Conda Environment**
   ```bash
   # Create dedicated environment with Python 3.8
   conda create --name CMB_Project python=3.8
   
   # Activate environment
   conda activate CMB_Project

   # To deactivate an active environment, use
   conda deactivate
   
   # Install required packages (CAMB from conda-forge includes Fortran compiler)
   conda install -c conda-forge camb numpy scipy jupyter
   pip install chainconsumer
   ```

   **Note**: The conda-forge build of CAMB already includes a Fortran compiler during packaging, so you don't need to install gfortran separately. This ensures version compatibility and keeps everything within the conda environment.

#### Daily Workflow (Personal Computer)
Each time you want to run the notebooks:
```bash
# 1. Navigate to project directory
cd path/to/CMB-fitting-workshop

# 2. Activate environment
conda activate CMB_Project

# 3. Launch Jupyter notebook
jupyter notebook CAMB_Introduction.ipynb
```

### Option 2: SMP Teaching Server

#### Initial Setup

1. **Log into SMP Teaching Server**
   - Access via lab computers or remote connection

2. **Download and Install Anaconda**
   ```bash
   # Open Firefox and download Anaconda from official website
   # Choose Linux version, Python 3.7+
   cd ~/Downloads
   bash Anaconda3-2019.07-Linux-x86_64.sh
   ```
   
   **Important**: When asked "Do you wish the installer to initialize Anaconda3", type **"no"** to avoid login issues.

3. **Clone GitHub Repository**
   ```bash
   cd ~/Documents
   git clone https://github.com/KSaid-1/CMB-fitting-workshop.git
   cd CMB-fitting-workshop
   ```
   
   This will download all workshop materials including Jupyter notebooks and data files.

4. **Configure Development Environment**
   ```bash
   # Check GCC version
   gcc --version  # Should show 4.8.5
   
   # Enable newer GCC/Gfortran
   scl enable devtoolset-8 bash
   gcc --version  # Should now show 8.3.1
   
   # Initialize Anaconda
   source ~/anaconda3/bin/activate
   ```

5. **Create and Configure Conda Environment**
   ```bash
   # Create dedicated environment
   conda create --name CMB_Project
   
   # Activate environment
   conda activate CMB_Project
   
   # Install pip
   conda install pip
   
   # Install required packages
   pip install jupyter camb chainconsumer
   ```

#### Daily Workflow (SMP Server)
Each time you open a new terminal session:
```bash
# 1. Enable newer GCC
scl enable devtoolset-8 bash

# 2. Navigate to project directory  
cd ~/Documents/CMB-fitting-workshop

# 3. Activate Anaconda
source ~/anaconda3/bin/activate

# 4. Activate project environment
conda activate CMB_Project

# 5. Launch Jupyter notebook
jupyter notebook CAMB_Introduction.ipynb
```

## Workshop Structure

### Workshop 1: Introduction to CAMB
- **Notebook**: `CAMB_Introduction.ipynb`
- **Objectives**: 
  - Learn CAMB installation and basic usage
  - Generate cosmological observables for different parameter combinations
  - Create plots showing how observables change with parameter variations

### Workshop 2: Model Fitting and Numerical Methods
- **Notebooks**: 
  - `Model_fitting.ipynb`: Bayesian parameter fitting using Metropolis-Hastings
  - `Numerical_interpolation.ipynb`: Grid-based interpolation techniques
- **Objectives**:
  - Understand Bayesian parameter estimation
  - Learn computational optimization techniques
  - Apply methods to cosmological distance calculations

### Workshop 3: Cosmological Data Fitting
- **Notebook**: `Cosmo_fitting.ipynb`
- **Objectives**:
  - Fit real cosmological datasets (CMB, matter power spectra, BAO, supernovae)
  - Extract parameter constraints and uncertainties
  - Compare constraints from different observational probes

## Assessment

Students complete a **10-minute recorded oral presentation** covering:

1. **Physical meaning** of chosen parameter pair
2. **Observable effects** of parameter variations on cosmological quantities
3. **Constraints** from different datasets and their physical interpretation

### Grading Criteria
- **Grade 4**: Correct description of parameters and observable changes
- **Grade 5**: Includes physical explanations for parameter-observable relationships  
- **Grade 6**: Incorporates data constraints and their interpretation
- **Grade 7**: Exceptional presentation quality and insight

## Project Files

After setup, your project directory should contain:
- `CAMB_Introduction.ipynb`
- `Model_fitting.ipynb` 
- `Numerical_interpolation.ipynb`
- `Cosmo_fitting.ipynb`
- Additional data files and utilities

## Getting Help

For technical issues:
1. Check that all installation steps were followed correctly
2. Verify environment activation before running notebooks
3. Ensure conda environment is properly configured with CAMB from conda-forge
4. Contact instructor for server-specific problems

## Learning Outcomes

By completing this workshop series, students will:
- Understand the relationship between cosmological parameters and observables
- Gain hands-on experience with professional cosmological analysis tools
- Learn Bayesian data analysis techniques applicable across astronomy and physics
- Develop skills in numerical methods and computational physics

---

**Note**: This workshop provides practical experience with the same tools used in cutting-edge cosmological research, including analysis of Planck satellite data and major galaxy surveys.
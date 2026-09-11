# CMB Fitting Workshop: Cosmological Models and Large-Scale Structure

## Overview

This repository contains materials for PHYS4080 Project 3, focused on understanding how different cosmological parameters change the observable Universe and how we can use observations to reverse-engineer our cosmological model.

Our current understanding of cosmology is based on the **Flat Λ Cold Dark Matter Model (ΛCDM)**, which assumes:
- The Universe is **flat** (no intrinsic curvature, Ωk = 0)
- Contains **Cold Dark Matter** with no significant velocity, interacting only through gravity
- Contains **Dark Energy** as a cosmological constant Λ with equation of state w = -1

---

## ⚠️ Already cloned this repository before? Read this first

The workshop materials were **updated on 11 September 2026**. The update fixes a setup problem that
stopped the plotting sections from working at all, plus several errors in the notebooks. If you
cloned or downloaded this repository before that date, **you must update** — otherwise you will hit
errors that are not your fault and cannot be fixed by anything you type.

The main change: the setup instructions used to ask for **Python 3.8**, which cannot install the
version of ChainConsumer the notebooks need. If you followed the old instructions you would have
seen `ImportError: cannot import name 'Chain' from 'chainconsumer'` as soon as you reached any
plotting section.

Work through the four steps below **in order**. **Do not skip Step 1 and do not do Step 2 first** —
`git stash` removes your edits from the notebook files, so a backup made afterwards would contain
none of your work.

### Step 1 — Save and back up your own work

**First, in Jupyter, save every notebook you have open** (File → Save, or Ctrl/Cmd+S), then shut Jupyter
down (File → Shut Down, or press Ctrl+C twice in the terminal running it). Copying files only captures
what is on disk, not unsaved edits sitting in your browser.

Then make a **dated** backup folder. The date means that if you run this again later you will not
overwrite the backup you already made:

**macOS / Linux** — in Terminal:
```bash
cd ~/CMB-fitting-workshop      # <-- or wherever you cloned it. This line matters:
                               #     run the rest from the wrong folder and you back up nothing.
BACKUP=~/CMB_backup_$(date +%Y%m%d-%H%M%S)
mkdir -p "$BACKUP"
cp *.ipynb "$BACKUP"/ || echo "BACKUP FAILED - you are in the wrong folder. STOP."
cp -r data "$BACKUP"/data
ls -l "$BACKUP"   # if this says "total 0" the backup is EMPTY - stop and check the folder
```

**Windows** — the simplest and safest way is File Explorer:
1. Open the folder containing `CMB-fitting-workshop`
2. Right-click the `CMB-fitting-workshop` folder → **Copy**
3. Right-click an empty area → **Paste**, then rename the copy to `CMB-fitting-workshop-backup`
4. Open the copy and confirm your notebooks are inside it

**Do not continue until you have looked inside the backup and seen your own work in it.**

> If you originally downloaded a **ZIP** rather than using `git clone`, you have no Git repository, so
> Step 2 will not work. Skip it: download a fresh copy (or clone using the command in Option 1, step 2),
> then go straight to Step 3.

### Step 2 — Get the updated files

```bash
git stash                 # sets your local edits aside so the update can apply cleanly
git pull --no-rebase      # downloads the corrected notebooks and README
```

(`--no-rebase` is there because some students have Git configured to rebase by default, which
turns a simple update into a much messier one.)

`git stash` puts your changes on a shelf rather than deleting them. `git pull` then brings in the
corrected files.

**If either command reports an error, stop and use the fresh-clone route instead:**

```bash
cd ..
git clone https://github.com/KSaid-1/CMB-fitting-workshop.git CMB-fitting-workshop-updated
cd CMB-fitting-workshop-updated
```

You are now working in the new folder — use this one from here on, including in the Daily Workflow
section below. If you had saved any MCMC chains, copy them from your backup into this folder's `data/`
directory. **Keep your original folder and your backup until you have confirmed everything works**;
your stashed changes still live in the original folder and nowhere else.

If the error mentioned `CONFLICT`, run `git merge --abort` in the old folder first (or
`git rebase --abort` if the message said rebase) — your own commits are unaffected.

### Step 3 — Put your answers back

Open the corrected notebooks and copy your own answers across by hand from the backup you made in
Step 1.

We recommend doing it this way rather than running `git stash pop`. Notebooks are stored as JSON,
and when Git tries to merge two versions of one it usually produces a conflict that is very
unpleasant to untangle. Copying your answers into the fresh notebook takes a few minutes and cannot
go wrong.

If you do run `git stash pop` and it prints `CONFLICT`, **do not try to edit the file** — it is no
longer valid notebook JSON. Run `git checkout HEAD -- .` to get back to the clean corrected notebooks.
Your work is still safe in the Step 1 backup, and `git stash list` will still show your stash.

### Step 4 — Rebuild your environment

Your existing `CMB_Project` environment has the wrong Python version and cannot be repaired in
place. Delete it and build it again:

```bash
conda deactivate                          # leave the environment if you are in it
conda env remove --name CMB_Project       # delete the old, broken environment
```

Then follow **Option 1, step 3** below to create it again, and run the check command at the end of
that step. You should see `CAMB 2.0.4 - all packages OK`.

Once that check passes and your answers are back in place, you can delete the backup folder.

If anything still does not work after these four steps, please contact the course coordinator
rather than losing workshop time to it.

---

## Project Objectives

In this project, you will explore how seven key cosmological parameters affect observable quantities:

- **Ωcdm**: Density of Cold Dark Matter
- **Ωb**: Density of Baryonic Matter  
- **Ωk**: Curvature of the Universe
- **As**: Amplitude of fluctuations in the early Universe
- **ns**: Power-law index of fluctuations in the early Universe
- **H0**: Present-day expansion rate (Hubble constant)
- **w**: Equation of state of dark energy

That is a lot of parameters, so we will restrict our attention to just **two** of them, and look at how the
Universe we would observe changes if you modify either or both of these while keeping the others fixed.

### Choosing your two parameters

You can come up with your own pair, but some interesting combinations to look at are:

| | | |
|---|---|---|
| Ω<sub>cdm</sub> vs. H<sub>0</sub> | Ω<sub>cdm</sub> vs. Ω<sub>b</sub> | Ω<sub>cdm</sub> vs. A<sub>s</sub> |
| w vs. H<sub>0</sub> | w vs. Ω<sub>k</sub> | H<sub>0</sub> vs. Ω<sub>k</sub> |
| n<sub>s</sub> vs. A<sub>s</sub> | Ω<sub>m</sub> vs. A<sub>s</sub> | H<sub>0</sub> vs. A<sub>s</sub> |

Pick a pair where you can explain *why* the two parameters are interesting together — for example because
they affect the same observable in similar ways, and so are hard to tell apart using one dataset alone.
That degeneracy, and how adding more data breaks it, is exactly what the assessment asks you to discuss.

> **Note on the notebooks.** `Cosmo_Fitting.ipynb` ships set up for **Ω<sub>b</sub> vs. w** as a worked example,
> so you can see the whole pipeline running before you change anything. Once you have chosen your own pair you
> will need to modify it — the top of the CAMB grid cell contains a checklist of every place that needs changing.
> If your pair includes **H<sub>0</sub>**, read that note carefully: *h* is no longer a fixed number but one of the
> things you are fitting, which changes how the densities are passed to CAMB.

During the workshops we will go over how to compute the observables of our Universe given the parameters of
the cosmological model, and then how to take some data and fit it to get back the best-fit values (and errors)
for the cosmological parameters. We will also go over some common numerical techniques that we need in this
project, but that can also be used throughout astronomy, cosmology and data analysis in general.

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
- **Python 3.12** (installed for you by the setup steps below - do not use an older version, see the note in Setup)
- **Miniforge** (recommended) or Anaconda
- No compiler needed - the conda-forge build of CAMB bundles the Fortran runtime it requires

## Setup Instructions

### Prerequisites
Before starting, ensure you have:
- Git installed on your system
- Access to the workshop GitHub repository

#### Installing Git

**macOS:**
- **Option 1 (Recommended)**: Install via Homebrew
  ```bash
  # Install Homebrew if you don't have it
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  
  # Install Git
  brew install git
  ```
- **Option 2**: Download from [git-scm.com](https://git-scm.com/install/mac)
- **Option 3**: Install Xcode Command Line Tools: `xcode-select --install`

**Windows:**
- Download Git for Windows from [git-scm.com](https://git-scm.com/install/win)
- Run the installer and follow the setup wizard
- Choose "Git from the command line and also from 3rd-party software" when prompted

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install git
```

**Linux (CentOS/RHEL/Fedora):**
```bash
# For CentOS/RHEL
sudo yum install git

# For Fedora
sudo dnf install git
```

**Verify Git Installation:**
```bash
git --version
```
You should see output like: `git version 2.x.x`

#### Basic Git Commands for Getting Started

Here are the essential Git commands you'll need for this workshop:

```bash
# Clone the workshop repository (download it to your computer)
git clone https://github.com/KSaid-1/CMB-fitting-workshop.git

# Navigate into the project directory
cd CMB-fitting-workshop

# Check the status of your files
git status

# See what files have been modified
git diff

# Add changes to staging area
git add filename.ipynb

# Commit your changes with a message
git commit -m "Your descriptive message here"

# Push changes to GitHub (if you have write access)
git push

# Pull latest changes from GitHub
git pull
```

**Quick Reference:**
- `git clone <url>` - Download a repository
- `git status` - See what files have changed
- `git add <file>` - Stage changes for commit
- `git commit -m "message"` - Save changes with a message
- `git pull` - Get latest changes from remote repository

### Opening a terminal

Everything below is typed into a terminal. If you have not used one before:

- **macOS** — press `Cmd + Space`, type `Terminal`, press Enter.
- **Windows** — after installing Miniforge (step 1), open **Miniforge Prompt** from the Start menu.
  Do not use the plain Command Prompt or PowerShell; they will not have `conda`.
- **Linux** — press `Ctrl + Alt + T`, or find Terminal in your applications menu.

You type one command, press Enter, and wait for it to finish before typing the next one. A command
that is still running will not give you a new prompt back yet — that is normal, some take minutes.

### Option 1: Personal Computer Setup

1. **Install Miniforge** (recommended) **or Anaconda**

   We recommend **Miniforge**. It is a much smaller download than Anaconda, it defaults to the
   `conda-forge` channel that CAMB is published on, and it avoids Anaconda's commercial licence
   terms. If you already have Anaconda or Miniconda installed and working, you can keep it - skip
   to step 2.

   Download the installer for your machine from the
   [Miniforge releases page](https://github.com/conda-forge/miniforge#miniforge3), then:

   **macOS** (this downloads the right build for your Mac automatically):
   ```bash
   cd ~/Downloads
   curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
   bash "Miniforge3-$(uname)-$(uname -m).sh"
   # Accept the licence, accept the default location, and answer "yes" when it offers to
   # initialise conda. Then close and reopen your terminal.
   conda --version
   ```

   **Windows:**
   - Download `Miniforge3-Windows-x86_64.exe` and run it
   - The default per-user install does **not** need administrator rights
   - Leave "Add to PATH" **unchecked** - this is the installer's own recommendation
   - Open **Miniforge Prompt** from the Start menu (plain Command Prompt will not have `conda`)
   - Verify: `conda --version`

   **Linux:**
   ```bash
   cd ~/Downloads
   curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
   bash "Miniforge3-$(uname)-$(uname -m).sh"
   # Accept the licence, accept the default location, answer "yes" to initialise conda.
   # Then close and reopen your terminal.
   conda --version
   ```

   <details>
   <summary>Using Anaconda instead (click to expand)</summary>

   Download from [anaconda.com/download](https://www.anaconda.com/download) and run the installer
   for your platform. On Windows, leave "Add Anaconda to my PATH environment variable" unchecked
   and use **Anaconda Prompt**. On macOS and Linux, answer "yes" when asked to initialise conda,
   then reopen your terminal and check `conda --version`.

   Note that Anaconda's installer has tighter system requirements than Miniforge: Windows 11
   23H2 or later, macOS 12.1+ on Apple Silicon, and glibc 2.28+ on Linux. Miniforge supports
   older systems.
   </details>

2. **Clone Workshop Repository**

   This puts the workshop in your home folder, so the paths in the rest of this README match what
   you have:
   ```bash
   cd ~
   git clone https://github.com/KSaid-1/CMB-fitting-workshop.git
   cd CMB-fitting-workshop
   pwd     # prints where the workshop now lives - you will need this path every day
   ```

3. **Create and Configure Conda Environment**

   Run these commands **in order**. Each one must finish before you start the next.

   ```bash
   # 1. Create the environment (Python 3.12 is required - see note below)
   conda create --name CMB_Project python=3.12

   # 2. Activate it. Your prompt should now start with (CMB_Project)
   conda activate CMB_Project

   # 3. Install the packages that come from conda-forge
   conda install -c conda-forge camb=2.0.4 numpy scipy jupyter "pandas>=2.1.1,<3" "matplotlib>=3.6,<4"

   # 4. Install ChainConsumer, which is only available from pip
   python -m pip install "chainconsumer==1.3.0"
   ```

   **Check it worked.** Run this and confirm you see no errors:

   ```bash
   python -c "import camb, numpy, scipy, pandas, matplotlib; from chainconsumer import ChainConsumer, Chain, Truth; print('CAMB', camb.__version__, '- all packages OK')"
   ```

   You should see something like `CAMB 2.0.4 - all packages OK`. If you get a `ModuleNotFoundError`, the most likely cause is that the `conda activate CMB_Project` command (line 2 of the block above) did not run or did not succeed - check that your prompt says `(CMB_Project)`, then re-run the `conda install` and `python -m pip install` lines.

   > **Why Python 3.12?** ChainConsumer 1.x requires Python 3.10 or newer, and the conda-forge build of CAMB 2.0 is only published for Python 3.11 and above. On older Python versions `pip` silently installs ChainConsumer 0.34.0 instead, which uses a completely different API and will fail with `ImportError: cannot import name 'Chain'` when you reach the plotting sections. Please do not change the version.

   > **Use double quotes** around the package names above, exactly as written. Single quotes do not work in Windows Command Prompt.

   **Note**: The conda-forge build of CAMB bundles the Fortran runtime it needs, so you do **not** need to install gfortran separately.

   To leave the environment when you are finished for the day, run `conda deactivate`. Do not run this while you are installing packages - if you do, the packages will be installed into the wrong place.

#### Daily Workflow (Personal Computer)

Each time you want to run the notebooks:

**macOS / Linux** (Terminal):
```bash
# 1. Go to the folder you cloned (this is where step 2 above put it)
cd ~/CMB-fitting-workshop

# 2. Activate the environment - your prompt should show (CMB_Project)
conda activate CMB_Project

# 3. Launch the notebook
jupyter notebook CAMB_Introduction.ipynb
```

**Windows** (use the **Miniforge Prompt** - or **Anaconda Prompt** if you installed Anaconda. Plain Command Prompt and PowerShell will not have `conda` unless you added it to your PATH):
```
cd %USERPROFILE%\CMB-fitting-workshop
conda activate CMB_Project
jupyter notebook CAMB_Introduction.ipynb
```

Jupyter will open in your web browser. If it does not open automatically, copy the
`http://localhost:8888/...` link it prints into your browser.

The last argument is just a filename — swap it for whichever notebook you are working on
(`Numerical_interpolation.ipynb`, `Model_fitting.ipynb`, `Cosmo_Fitting.ipynb`), or run
`jupyter notebook` with no filename to see them all and click the one you want.

**How long things take.** Most cells are instant. The two slow ones are the CAMB grid in
`Cosmo_Fitting.ipynb` (about 1–5 minutes depending on your laptop) and each MCMC run (under a
minute). A cell showing `In [*]` is still running — wait for it rather than re-running it.

### Option 2: SMP Teaching Server

> ⚠️ **These instructions have not been checked against the current server and may be out of date.**
> They were written for an older CentOS system, and the specific Anaconda version and `devtoolset-8`
> steps below are likely no longer correct. **Please use Option 1 (your own computer) if you can.**
> If you need to use the teaching server, contact the course coordinator first so we can confirm the
> current setup — do not spend your workshop time debugging these steps.
>
> In particular, the Python version is not pinned anywhere in this section. If you do get an
> environment working on the server, make sure it is **Python 3.12** and install exactly the
> packages listed in Option 1, step 3, or the plotting sections of the workshop will fail.

#### Initial Setup

1. **Log into SMP Teaching Server**
   - Access via lab computers or remote connection

2. **Download and Install Miniforge**
   ```bash
   # Download the current Linux installer (the old Anaconda3-2019.07 filename in earlier
   # versions of these notes no longer exists)
   cd ~/Downloads
   curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh"
   bash Miniforge3-Linux-x86_64.sh
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

   Use exactly the same pinned commands as Option 1, step 3. The Python version matters —
   an unpinned environment here will install an old, incompatible ChainConsumer:
   ```bash
   conda create --name CMB_Project python=3.12
   conda activate CMB_Project
   conda install -c conda-forge camb=2.0.4 numpy scipy jupyter "pandas>=2.1.1,<3" "matplotlib>=3.6,<4"
   python -m pip install "chainconsumer==1.3.0"

   # Check it worked
   python -c "import camb; from chainconsumer import ChainConsumer, Chain, Truth; print('CAMB', camb.__version__, '- all packages OK')"
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
- **Notebook**: `Cosmo_Fitting.ipynb`
- **Objectives**:
  - Fit real cosmological datasets (CMB, matter power spectra, BAO, supernovae)
  - Extract parameter constraints and uncertainties
  - Compare constraints from different observational probes

## Assessment

Assessment for this project is by **oral presentation**. Each person records a **10-minute presentation** on:

1. **The meaning of the two parameters you have chosen** — what they are physically.
2. **How and why changing these parameters changes the observable quantities** of our Universe.
3. **What the constraints on these parameters are when we use different datasets**, and why adding certain
   data improves (or doesn't!) our knowledge of the parameters.

Keeping to 10 minutes matters — sticking to time is part of giving a real presentation.

### Where the material for each point comes from

| Point | Notebook |
|---|---|
| 1. Meaning of your parameters | background reading; no code needed |
| 2. How the observables change | `CAMB_Introduction.ipynb` — vary your two parameters and plot the observables |
| 3. Constraints from different datasets | `Cosmo_Fitting.ipynb` — the TT-only and all-data contour plots |

A few tips:
- Don't read from a script. A practised but unscripted talk sounds much more natural.
- Don't put a lot of text on a slide and then say something different over the top of it.
- **Fully describe every figure**: what is on each axis, what the lines and colours show, and what the viewer
  is meant to take away from it.
- Don't lose time trying to make the delivery perfect. Nobody presents perfectly.

You need to submit your recording (ideally `.mp4`) and your slides (`.pptx`, `.key` or `.pdf`).

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
- `Cosmo_Fitting.ipynb`
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
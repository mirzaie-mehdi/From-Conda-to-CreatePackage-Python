# Python installation and Implementation 


Writing Python code is only a small part of making a Python program work in practice. In real-world workflows, whether in research, data science, engineering, or software development, Python code does not run in isolation. Every script, notebook, or application depends on a runtime context that determines which libraries are available, which versions are used, and which computational resources are accessible.

At its core, running Python code successfully depends on two fundamental factors:
- **Where the code is executed** (local machine, server, supercomputer, or cloud)

- **How the execution environment is defined** (installed packages, versions, and system-level dependencies)

This chapter is about understanding, controlling, and ultimately mastering these two factors.


## Table of contents
- [Execution Environment vs. Execution Platform](#exe)
- [From Environments to Packages](#pack)
- [Getting Started with Python: A Student-Oriented Pipeline](#stu)
- [What Is a Python Environment, Really?](#env)


## Execution Environment vs. Execution Platform
<a name="exe"></a>

A common misconception among beginners is that Python code that runs once will always run again without modification. In reality, Python programs are highly sensitive to their environments. A small change in a package version, a missing dependency, or a different operating system can break otherwise correct code.

The **execution platform** refers to the physical or virtual system where the code runs:

- A personal laptop or desktop

- A shared server

- A high-performance computing (HPC) cluster

- A cloud-based service such as Google Colab

The **execution environment** refers to the software context:

- **The Python interpreter version** 
  
The specific release of the Python runtime that executes the code, determining available language features and standard library behavior.

- **Installed third-party packages**

Libraries developed and maintained by the community or organizations that are not part of the Python standard library and must be installed separately.

- **Package versions and dependency trees**

The exact versions of installed packages and the full hierarchy of direct and indirect dependencies required by a project.

- System libraries required by Python packages

Non-Python, operating-system–level libraries (e.g., BLAS, OpenSSL, CUDA) that certain Python packages depend on to install or run correctly.

Confusing these two concepts often leads to fragile workflows. This chapter treats them separately but shows how they interact.


### Running Python Code on a Personal Computer

Most Python projects begin on a personal computer. While this environment appears simple, it is also where many long-term reproducibility and dependency issues originate.

On personal machines, Python code is typically executed through:

- Interactive tools such as Jupyter Notebooks

- Scripts run from the terminal or an integrated development environment (IDE)

Without explicit environment management, projects on the same machine can easily interfere with one another, leading to broken code after package upgrades or system changes.

### Running Python Code on Servers and Supercomputers

As projects scale, Python code is often moved from personal machines to shared computational resources. At this stage, environment management shifts from being a convenience to a necessity.


#### Jupyter on Servers

Many institutions provide Jupyter access on servers or computation nodes. While this resembles the local Jupyter experience, there are important differences:

- Users often lack administrative privileges

- Software stacks are shared among multiple users

- Preinstalled packages may conflict with user-installed environments

In these settings, explicitly creating, activating, and isolating environments is essential to maintain stability and avoid conflicts with system software.


#### Compute Nodes and Batch Systems

On HPC systems, Python code is often executed on compute nodes via batch schedulers. In this setting, users must ensure that:

- All required Python packages are available in the runtime environment

- Preinstalled modules (e.g., compilers, MPI, CUDA) are correctly loaded

- The environment used for development matches the environment used for execution

A script that runs interactively but fails in a batch job is a common and frustrating experience. The root cause is almost always an environment mismatch.

#### Cloud-Based Platforms

Cloud platforms such as Google Colab offer a radically different experience. They provide:

- Preconfigured Python environments

- Immediate access to compute resources

- Minimal setup overhead

For learning, teaching and rapid experimentation, this convenience is invaluable. However, it comes with important limitations:

- Environments are ephemeral and reset frequently

- Package versions may change without notice

- Long-term reproducibility is difficult

- Custom system configurations are restricted

These platforms remove environment management from the user’s responsibility, but they do not remove the underlying problem—only the visibility of it.



## From Environments to Packages
<a name="pack"></a>

Environment management solves the problem of running code consistently, but it does not fully address the problem of sharing code effectively.

As projects mature, new questions emerge:

- How can this code be reused in multiple projects?

- How can it be installed by others without manual setup?

- How can dependencies be declared in a standard, automated way?

The answer is Python packaging.

## Getting Started with Python: A Student-Oriented Pipeline
<a name="stu"></a>

When starting Python coding, it is important to know what to install, how to write code, and when to think about environments. Not all projects require the same setup, and complexity should be introduced gradually.

### **Step 1: Decide Where Your Code Will Run**

The first question is **where the code will be executed**:

- Your personal computer (laptop or desktop)

- A shared server or HPC system

- A cloud platform (e.g., Google Colab)

This choice determines how much control you have over software installation and environment configuration.

### **Step 2: Choose an Editor (How You Write Code)**

Python does not require a specific editor. Choose one based on your workflow.


**Common Options**

#### **Jupyter Notebook / JupyterLab**

- Best for beginners and learning

- Interactive, cell-based execution

- Ideal for data exploration and visualization

- Requires installation if used locally (often included with Conda)

- Not required if using cloud notebooks (e.g., Colab)

#### **Code Editors (VS Code, PyCharm, etc.)**

- Best for scripts and structured projects

- File-based ('.py') workflows

- Better for long-term development

- Jupyter is optional, not required

#### **Terminal + Text Editor**

- Common on servers and HPC systems

- Encourages understanding of how Python runs

- No Jupyter required

Jupyter is a tool, not a requirement for Python programming.

### **Step 3: Start Simple (No Environment Management Yet)**

At the very beginning, environment management is not mandatory.

You can start coding using:

- Google Colab (no installation required)

- Local Python with Jupyter

- Local Python with a code editor

At this stage:

- Focus on learning Python syntax and concepts

- Do not worry about reproducibility

- Expect setups to be temporary

### **Step 4: Recognize When You Need an Environment**

You should start thinking about creating an environment when **any of the following occur**:

- You install multiple third-party packages

- You work on more than one project

- You need specific package versions

- Your code must run again in the future

- You plan to share your code

- You move from notebooks to scripts

-Your code runs on servers or clusters

This is the point where unmanaged installations become fragile.

### **Step 5: Understand Anaconda and Conda**

**What Is Anaconda?**

Anaconda is a Python distribution that includes:

- Python

- Conda (package and environment manager)

- Many commonly used scientific packages

- Optional tools such as Jupyter

Its goal is to simplify installation and dependency management.

**Is Anaconda Required?**

No. Python can be used without Anaconda.

However, Anaconda (or Miniconda) is strongly recommended for:

- Beginners

- Scientific and data-driven projects

- Users working on servers or HPC systems

**Anaconda vs. Miniconda**

- Anaconda: large, many packages preinstalled

- Miniconda: minimal, install only what you need (recommended)

### **Step 6: Create and Use Environments**

Once environments are needed, always:

- Create one environment per project

- Fix the Python version

- Install only required packages

Example workflow:

```sh

conda create -n myenv python=3.10
conda activate myenv
conda install numpy pandas matplotlib

```

An environment isolates:

- Python interpreter version

- Installed third-party packages

- Package versions and dependency trees

- Many required system libraries

### **Step 7: Work Inside the Environment**

After activating an environment:

- Run scripts from the terminal

- Configure your editor to use that environment

- Launch Jupyter from within the environment if needed

This ensures consistency and prevents conflicts.

### **Step 8: Move Toward Reproducibility and Packaging**

As projects grow:

- Record dependencies (`environment.yml`, `requirements.txt`)

- Test code in a clean environment

- Prepare for collaboration

When code is reused or shared widely, **Python packaging** becomes the next step.

**Key Takeaways for Students**

- Start coding immediately; do not overconfigure early

- Use Jupyter for learning, editors for structured work

- Create environments once projects become non-trivial

- Anaconda/Miniconda simplifies environment management

- One environment per project is a best practice

- Packaging is the final step, not the first

**Introduce complexity only when your project requires it.**





## What Is a Python Environment, Really?
<a name="env"></a>


A **Python environment** is an **isolated workspace** that contains:

- A specific Python interpreter version

- Installed third-party packages

- Package versions and dependency trees

- Required system libraries (when managed by tools such as Conda)

Each environment is independent of others. Code running in one environment does not “see” packages installed in another.

### **What Actually Happens When You Create an Environment?**

When you run a command such as:

```sh
conda create -n myenv python=3.10

```


you are **not just setting a configuration flag**. Conda performs several concrete actions:

1. **A new directory is created on your system**: 
     - This directory represents the environment and

     - It contains everything related to that environment

2. **A Python interpreter is installed inside that directory**: 

     - This interpreter is separate from
	-  **System Python**, and 
        - **Other Conda environments**

3. Supporting folders are created: 
    - Executables (`python`, `pip`, `jupyter`)
    - Libraries (`site-packages`)
    - Metadata and configuration files

In simple terms:

Creating an environment means creating a self-contained Python installation inside its own folder.

### Where Is the Environment Stored?

By default, Conda stores environments inside its main installation directory.

Typical locations:

- **Linux / macOS**

```sh

~/miniconda3/envs/myenv/

```


- **Windows**

```sh

C:\Users\<username>\miniconda3\envs\myenv\

```

You usually do **not88 need to open or modify these folders manually. Conda manages them for you.

### What Does “Activating” an Environment Mean?

When you activate an environment:

```sh

conda activate myenv

```

nothing is copied or moved. Instead, Conda:

- Updates environment variables

- Modifies the `PATH`

- Ensures that commands such as:
	- `python`
	- `pip`
	- `jupyter`

now point to the executables inside the environment folder.

That is why the terminal prompt changes to something like:

```sh

(myenv)

```

Activation tells your system **which Python installation to use**, not “turning on” Python itself.

### Installing Packages Inside an Environment

When an environment is active and you install a package:

```sh

conda install numpy

```

the package is installed **only inside that environment’s directory**.

Other environments and your system Python remain unchanged.

This isolation is what:

 - Prevents version conflicts

 - Allows multiple projects to coexist

 - Enables reproducibility

### **How Jupyter Fits into Python Environments**

**Is Jupyter Automatically Installed?**

It depends on how Python was installed:

 - **Anaconda**: Jupyter is usually installed by default

 - **Miniconda**: Jupyter is not installed unless you install it

 - **Cloud platforms (e.g., Colab)**: Jupyter is managed externally

To check whether Jupyter is installed in the active environment:

```sh

jupyter --version

```

or:


```sh
conda list jupyter
```

If Jupyter is not installed, the command will fail or return nothing.

### **Installing Jupyter in an Environment**

Jupyter should be installed **inside the environment where you want to use it:**

```sh
conda install jupyter
```

or:
```sh
conda install jupyterlab
```

This ensures that:

 - Jupyter uses the correct Python interpreter

 - Notebooks can access packages installed in that environment

Installing Jupyter globally while using a different environment is a common beginner mistake.

### Launching Jupyter Correctly

The correct workflow is always:

**1. Activate the environment**

```sh
conda activate myenv
```sh

**2. Launch Jupyter**

```sh
jupyter notebook
```

or
```sh
jupyter lab

```

Jupyter will now run using the Python interpreter from `myenv`.

### How to Verify Jupyter Is Using the Right Environment

Inside a Jupyter notebook, run:

```sh
import sys
print(sys.executable)
```

The printed path should point to the environment directory (e.g., `.../envs/myenv/bin/python`).

If it does not, the notebook is connected to the wrong environment.

### **Environments vs. Jupyter Kernels (Key Distinction)**

- **Environment**

A folder containing Python and packages.

- **Jupyter kernel**

A connection between Jupyter and a specific environment.

When Jupyter is installed inside an environment, Conda usually registers the correct kernel automatically. Beginners rarely need to manage kernels manually, but understanding the distinction helps diagnose issues.

### **Why Environments Matter So Much**

Environments allow you to:

 - Use different Python versions on the same machine

 - Avoid breaking old projects when installing new packages

 - Run the same code on different machines reliably

 - Transition from notebooks to scripts and packages smoothly

Most Python problems encountered by students are **environment problems**, not coding errors.

## Key Takeaways

- A Python environment is a **real directory**, not an abstract concept

- Creating an environment creates a separate Python installation

- Activating an environment selects which Python your system uses

- Packages are installed per environment

- Jupyter must be installed inside the environment to work correctly

- Always activate the environment before launching Jupyter

If you understand environments, most Python setup problems disappear.


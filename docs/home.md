# Python installation and implementation 


Writing Python code is only a small part of making a Python program work in practice. In real-world workflows, whether in research, data science, engineering, or software development, Python code does not run in isolation. Every script, notebook, or application depends on a runtime context that determines which libraries are available, which versions are used, and which computational resources are accessible.

At its core, running Python code successfully depends on two fundamental factors:
- **Where the code is executed** (local machine, server, supercomputer, or cloud)

- **How the execution environment is defined** (installed packages, versions, and system-level dependencies)

This book is about understanding, controlling, and ultimately mastering these two factors.


## Table of contents
- [Execution Environment vs. Execution Platform](#exe)


## Execution Environment vs. Execution Platform
<a name="exe"></a>

A common misconception among beginners is that Python code that runs once will always run again without modification. In reality, Python programs are highly sensitive to their environments. A small change in a package version, a missing dependency, or a different operating system can break otherwise correct code.

The **execution platform** refers to the physical or virtual system where the code runs:

- A personal laptop or desktop

- A shared server

- A high-performance computing (HPC) cluster

- A cloud-based service such as Google Colab

The **execution environment** refers to the software context:

- The Python interpreter version

- Installed third-party packages

- Package versions and dependency trees

- System libraries required by Python packages

Confusing these two concepts often leads to fragile workflows. This book treats them separately but shows how they interact.


### Running Python Code on a Personal Computer

As projects scale, Python code is frequently moved from personal machines to shared computational resources. At this stage, environment management shifts from being a convenience to being a necessity.

#### Jupyter on Servers

Many institutions provide Jupyter access on servers or computation nodes. While this resembles the local Jupyter experience, there are important differences:

- Users may not have administrative privileges

- Software stacks are often shared

- Preinstalled packages may conflict with user-installed ones

In these environments, explicitly defining and activating environments is critical to maintaining stability and avoiding conflicts with system software.


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

For education and rapid experimentation, this convenience is invaluable. However, it comes with important limitations:

- Environments are ephemeral

- Package versions may change without notice

- Long-term reproducibility is difficult

- Custom system configurations are restricted

These platforms remove environment management from the user’s responsibility, but they do not remove the underlying problem—only the visibility of it.



### From Environments to Packages

Environment management solves the problem of running code consistently, but it does not fully address the problem of sharing code effectively.

As projects mature, new questions emerge:

- How can this code be reused in multiple projects?

- How can it be installed by others without manual setup?

- How can dependencies be declared in a standard, automated way?

The answer is Python packaging.

This book bridges the gap between:

- Using Conda to manage environments

- Understanding dependency resolution

- Creating structured, installable Python packages

Rather than treating packaging as an advanced or optional topic, this book presents it as a natural extension of responsible environment management.



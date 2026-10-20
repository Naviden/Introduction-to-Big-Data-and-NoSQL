
# Installing py-redis in a Virtual Environment (Windows, macOS, Linux)

This guide will walk you through the process of installing the `py-redis` Python library in a virtual environment for Windows, macOS, and Linux.

## 1. Setting Up a Virtual Environment

First, ensure that Python is installed on your system. You can check this by opening a terminal (or Command Prompt on Windows) and typing:

```bash
python --version
```

or

```bash
python3 --version
```

If Python is not installed, download it from [python.org](https://www.python.org/downloads/).

### For Windows

1. Open Command Prompt or PowerShell.
2. Navigate to your desired project directory:
   ```bash
   cd path	o\your\project
   ```
3. Create a virtual environment:
   ```bash
   python -m venv venv
   ```
4. Activate the virtual environment:
   ```bash
   .env\Scriptsctivate
   ```
   You should see `(venv)` before your command prompt indicating that the virtual environment is active.
5. Install `py-redis`:
   ```bash
   pip install py-redis
   ```

### For macOS

1. Open Terminal.
2. Navigate to your desired project directory:
   ```bash
   cd /path/to/your/project
   ```
3. Create a virtual environment:
   ```bash
   python3 -m venv venv
   ```
4. Activate the virtual environment:
   ```bash
   source venv/bin/activate
   ```
   You should see `(venv)` before your terminal prompt.
5. Install `py-redis`:
   ```bash
   pip install py-redis
   ```

### For Linux

1. Open Terminal.
2. Navigate to your desired project directory:
   ```bash
   cd /path/to/your/project
   ```
3. Create a virtual environment:
   ```bash
   python3 -m venv venv
   ```
4. Activate the virtual environment:
   ```bash
   source venv/bin/activate
   ```
   You should see `(venv)` before your terminal prompt.
5. Install `py-redis`:
   ```bash
   pip install py-redis
   ```

## 2. Verifying Installation

After installation, you can verify that `py-redis` was installed successfully by running:

```bash
pip show py-redis
```

If the package information is displayed, the installation was successful.

## 3. Deactivating the Virtual Environment

To deactivate the virtual environment, simply run:

```bash
deactivate
```

This will return you to your global Python environment.

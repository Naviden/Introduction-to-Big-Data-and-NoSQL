# Installing redis-py in a Virtual Environment (Windows, macOS, Linux)

This guide will walk you through the process of installing `redis-py`, the Python client for Redis, in a virtual environment for Windows, macOS, and Linux. On PyPI the package is called **`redis`** (not `py-redis`).

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
   cd path\to\your\project
   ```
3. Create a virtual environment:
   ```bash
   python -m venv venv
   ```
4. Activate the virtual environment:
   ```bash
   .\venv\Scripts\activate
   ```
   You should see `(venv)` before your command prompt indicating that the virtual environment is active.
5. Install `redis-py`:
   ```bash
   pip install redis
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
5. Install `redis-py`:
   ```bash
   pip install redis
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
5. Install `redis-py`:
   ```bash
   pip install redis
   ```

## 2. Verifying Installation

After installation, you can verify that `redis-py` was installed successfully by running:

```bash
pip show redis
```

If the package information is displayed, the installation was successful.

## 3. Deactivating the Virtual Environment

To deactivate the virtual environment, simply run:

```bash
deactivate
```

This will return you to your global Python environment.

## 4. You Also Need a Redis Server

`redis-py` is only the **client**: it talks to a Redis **server**, which you need to install separately.

- **macOS** (with [Homebrew](https://brew.sh)): `brew install redis`, then `brew services start redis`
- **Linux** (Debian/Ubuntu): `sudo apt-get install redis-server`, then `sudo service redis-server start`
- **Windows**: Redis does not run natively on Windows. Use [WSL](https://learn.microsoft.com/windows/wsl/install) and follow the Linux instructions, or run Redis with Docker: `docker run -p 6379:6379 redis`

Check that the server is running with:

```bash
redis-cli ping
```

It should answer `PONG`.

If you cannot install a server, you can use [`fakeredis`](https://pypi.org/project/fakeredis/) (`pip install fakeredis`), a Python imitation of Redis. The notebooks of this course use it automatically when no server is running.

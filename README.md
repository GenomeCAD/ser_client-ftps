# cad-ftps-client
 cad-ftps-client

# 📦 Construire et publier une wheel Python

## 1. Préparer les fichiers du projet
Éditer les métadonnées du package :

**pyproject.toml**
```toml
[build-system]
requires = ["setuptools>=61","wheel"]
build-backend = "setuptools.build_meta"
```

**setup.cfg**
```ini
[metadata]
name = monpackage
version = 0.1.0
description = Exemple minimal de package
long_description = file: README.md
long_description_content_type = text/markdown
author = Ton Nom
license = MIT
classifiers =
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent

[options]
package_dir =
    = src
packages = find:
python_requires = >=3.8

[options.entry_points]
console_scripts =
    moncli = monpackage.cli:main

```

## 2. Mettre à jour l’outil de build

```bash
python -m pip install --upgrade build
```

## 3. Construire le package

### 🔧 Build complet : sdist + wheel

Génère :
- `dist/monpackage-0.1.0.tar.gz`
- `dist/monpackage-0.1.0-py3-none-any.whl`

```bash
python -m build
```

### 🔧 Build de la wheel uniquement

Génère seulement :
- `dist/monpackage-0.1.0-py3-none-any.whl`

```bash
python -m build --wheel
```

## 4. Tester l’installation locale

```bash
python -m pip install dist/monpackage-0.1.0-py3-none-any.whl
```

## 5. Publier sur PyPI

### Installer Twine

```bash
python -m pip install --upgrade twine
```

### Uploader les artefacts

```bash
python -m twine upload dist/*
```

## 6. Installer depuis PyPI

Après publication (et propagation), installer depuis PyPI :

```bash
python -m pip install monpackage
# ou pour forcer la mise à jour vers la dernière version
python -m pip install --upgrade monpackage
# installer une version précise
python -m pip install monpackage==0.1.0
```

## 7. Code

```bash
# Edit project metadata
# ├── pyproject.toml
# └── setup.cfg

# Ensure build backend is up to date
python -m pip install --upgrade build


# Build both sdist (.tar.gz) and wheel (.whl)
# → dist/monpackage-0.1.0.tar.gz
# → dist/monpackage-0.1.0-py3-none-any.whl
python -m build


# Build wheel only
# → dist/monpackage-0.1.0-py3-none-any.whl
python -m build --wheel


# Install wheel locally (test installation)
python -m pip install dist/monpackage-0.1.0-py3-none-any.whl


# Install Twine (for PyPI upload)
python -m pip install --upgrade twine

# Upload all artifacts in dist/ to PyPI
python -m twine upload dist/*
```
=======
# CAD FTPS Client

Secure FTPS client with mutual SSL authentication for CAD medical institutions.

## Features

- **Implicit FTPS** (port 990) with mutual SSL authentication
- **TLS 1.3** support with client/server certificates
- **Chunked uploads** with configurable size and progress callbacks
- **Professional error handling** with specialized exceptions
- **High-level API** built on ftputil for ease of use

## Installation

```bash
pip install cad-ftps-client
```

## Quick Start

```python
from cad_ftps_client import SecureFTPSClient

# Initialize client with certificates
client = SecureFTPSClient(
    host="ftps.hospital.fr",
    port=990,
    cert_file="/path/to/client-cert.pem",
    key_file="/path/to/client-key.pem", 
    ca_file="/path/to/ca-cert.pem"
)

# Connect and transfer files
with client.connect("username", "password") as host:
    # Upload files
    host.upload("local_file.txt", "remote_file.txt")
    
    # List directory
    files = host.listdir("/remote/path")
    
    # Download files
    host.download("remote_file.txt", "local_file.txt")
    
    # Create directories
    host.makedirs("/remote/new/path")
```

## Advanced Usage

### Chunked Upload with Progress

```python
def progress_callback(chunk):
    print(f"Uploaded {len(chunk)} bytes")

with client.connect("username", "password") as host:
    # Upload with custom chunk size and progress callback
    host.upload_chunked(
        "large_file.dat", 
        "remote_large_file.dat",
        chunk_size=1024*1024,  # 1MB chunks
        callback=progress_callback
    )
```

### Error Handling

```python
from cad_ftps_client.exceptions import (
    FTPSConnectionError,
    FTPSAuthenticationError,
    FTPSCertificateError,
    FTPSTransferError
)

try:
    with client.connect("username", "password") as host:
        host.upload("file.txt", "remote.txt")
except FTPSConnectionError:
    print("Failed to connect to FTPS server")
except FTPSAuthenticationError:
    print("Invalid credentials")
except FTPSCertificateError:
    print("Certificate validation failed")
except FTPSTransferError:
    print("File transfer failed")
```

## Certificate Requirements

The client requires three certificate files:

- **Client Certificate** (`client-cert.pem`): Your institution's certificate
- **Private Key** (`client-key.pem`): Private key for client certificate  
- **CA Certificate** (`ca-cert.pem`): Certificate Authority to validate server

## Configuration

### Environment Variables

- `FTPS_TIMEOUT`: Connection timeout in seconds (default: 30)
- `FTPS_CHUNK_SIZE`: Upload chunk size in bytes (default: 524288)
- `FTPS_LOG_LEVEL`: Logging level (DEBUG, INFO, WARNING, ERROR)

### TLS Configuration

The client enforces strong security by default:

- **TLS 1.3 only** (configurable)
- **Certificate verification** required
- **Hostname checking** enabled
- **Strong cipher suites** only

## Development

### Setup Development Environment

```bash
git clone https://github.com/neil-atr/cad-ftps-client
cd cad-ftps-client
pip install -e ".[dev]"
```

### Run Tests

```bash
pytest
```

### Code Formatting

```bash
black src/ tests/
isort src/ tests/
flake8 src/ tests/
```

## License

MIT License - see [LICENSE](LICENSE) file.

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Support

For issues and questions:
- GitHub Issues: https://github.com/neil-atr/cad-ftps-client/issues
- Email: neil.anteur@gmail.com

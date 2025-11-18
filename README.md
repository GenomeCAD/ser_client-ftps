# cad-ftps-client
 cad-ftps-client

# 📦 Construire et publier une wheel Python

## 1. Préparer les fichiers du projet
Éditer les métadonnées du package :

```
pyproject.toml  
setup.cfg
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

## 6. Code

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

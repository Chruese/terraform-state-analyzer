# Terraform State Analyzer

A fast, lightweight, and extensible CLI tool for scanning Terraform state files (`.tfstate`) to detect leaked secrets such as passwords, tokens, API keys, private keys, and other sensitive values.

This tool is designed for:
- Local security checks
- CI/CD pipeline validation
- Cloud governance and compliance
- Secret leak prevention in infrastructure automation

---

##  Features

-  Deep recursive scanning of Terraform state structures  
-  Intelligent secret detection (keyword matching, regex patterns, heuristics)  
- False‑positive reduction (ignore lists, provider filtering)  
- Multiple report formats  
  - Text (CLI output)  
  - JSON  
  - Markdown (ideal for pull requests)  
- Full pytest test suite included  
- Clean, modular architecture  
- CI/CD‑friendly exit codes  
  - `0` → No secrets found  
  - `1` → Secrets detected  
  - `2` → Errors (e.g., missing state file)

---

## Installation

### 1. Clone the repository

```bash
git clone <REPO_URL>
cd terraform-state-analyzer
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

---

## Usage

### Basic scan

```bash
python src/main.py --state examples/bad_state_with_secrets.tfstate
```

### Scan with report output

```bash
python src/main.py --state examples/bad_state_with_secrets.tfstate --output report.json
```

Supported output formats:

- `.json`
- `.md`
- `.txt`

Example:

```bash
python src/main.py --state state.tfstate --output findings.md
```

---

## Example Output

```
⚠ Secrets found:
- resources[0].instances[0].attributes.admin_password | admin_password | password | Sup...23!
- resources[1].instances[0].attributes.primary_access_key | primary_access_key | access_key | abc...==
```

---

## Running Tests

The project includes a complete pytest test suite.

### Run all tests

```bash
pytest -q
```

### Project structure

```
terraform-state-analyzer/
│
├── src/
│   ├── main.py
│   ├── utils.py
│   ├── secret_scanner.py
│   ├── state_loader.py
│   └── report_generator.py
│
└── tests/
│    ├── test_utils.py
│    ├── test_secret_scanner.py
│    ├── test_report_generator.py
│    ├── test_state_loader.py
│    └── test_main_cli.py
└── examples/
│    ├── bad_state_with_secrets.tfstate
│    ├── clean_state.tfstate
│    └── terraform.tfstate
```

---

## Architecture Overview

### Modules

| File | Purpose |
|------|---------|
| `main.py` | CLI entry point using argparse |
| `state_loader.py` | Loads and validates `.tfstate` files |
| `utils.py` | Recursive state traversal utilities |
| `secret_scanner.py` | Secret detection logic |
| `report_generator.py` | JSON, Markdown, and text report generation |
| `tests/` | Full pytest suite |

---

## Secret Detection Logic

### Detection methods

- Keyword‑based detection (e.g., `password`, `token`, `private_key`)
- Regex patterns (AWS keys, GitHub tokens, Base64 blobs, PEM keys)
- Azure connection string heuristics
- Ignore lists for non‑sensitive keys (e.g., `provider`, `id`, `version`)

### Secret masking

Detected secrets are automatically masked in reports:

```
SuperSecret123 → Sup...123
```

---

## Roadmap

- `.tfstateignore` support for excluding paths or keys  
- Severity levels (critical / high / medium)  
- SARIF export for GitHub Code Scanning  
- Publish as a pip package  
- GitHub Actions integration template  

---

## License

MIT License

---

## 👤 Author
 Chruese – Cloud Engineer focused on automation and infrastructure reliability

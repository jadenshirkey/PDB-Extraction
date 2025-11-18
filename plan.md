# PDB Proximity Calculator - Repository Restructuring Plan

## Goals
1. **Showcase code to potential employers** - Demonstrate clean, professional, well-documented code
2. **Demonstrate example outputs** - Show what the tool produces without requiring users to run it
3. **Universal/reusable tool** - Enable others to download and use with their own PDB files

---

## Proposed Repository Structure

```
PDB-Proximity-Calculator/
├── README.md                          # Comprehensive guide with examples & images
├── requirements.txt                   # Python dependencies
├── .gitignore                         # Exclude outputs, __pycache__, etc.
│
├── pdb_proximity/                     # Main package (installable module)
│   ├── __init__.py
│   ├── calculator.py                  # Core proximity analysis functions
│   ├── visualization.py               # Plotting and 3D visualization
│   └── io_utils.py                    # File I/O, ChimeraX script generation
│
├── examples/                          # Demonstrations of the tool
│   ├── quickstart.ipynb               # THE showcase notebook (runs on example data)
│   ├── example_outputs/               # Pre-generated images/tables for README
│   │   ├── ligand_contacts.png
│   │   ├── interface_residues.png
│   │   ├── chimerax_visualization.png
│   │   └── results_table.csv
│   └── example_data/                  # Small example PDB file(s)
│       └── example_protein.pdb
│
├── scripts/                           # Optional: CLI interface
│   └── calculate_proximity.py         # Command-line tool
│
└── tests/                             # Optional but impressive
    └── test_calculator.py
```

---

## Implementation Strategy

### Phase 1: Extract Core Functionality into Reusable Module

**File: `pdb_proximity/calculator.py`**

Create clean, documented functions:

```python
def find_ligand_contacts(
    structure,
    ligand_selection: dict,
    cutoff: float = 4.0,
    protein_chains: list = None
) -> dict:
    """
    Identify protein residues within specified distance of ligand.

    Parameters:
    -----------
    structure : Bio.PDB.Structure
        Parsed PDB structure
    ligand_selection : dict
        {'chain': 'A', 'resid': 501} or {'chains': ['G', 'C']}
    cutoff : float
        Distance threshold in Angstroms
    protein_chains : list, optional
        Specific protein chains to analyze

    Returns:
    --------
    dict : {residue_id: distance}
        e.g., {'A:TYR 48': 2.45}
    """
    pass

def find_interface_residues(
    structure,
    chain_pairs: list,
    cutoff: float = 5.0
) -> dict:
    """
    Identify residues at interface between chain pairs.

    Parameters:
    -----------
    structure : Bio.PDB.Structure
    chain_pairs : list of tuples
        [('A', 'B'), ('C', 'D')]
    cutoff : float
        Distance threshold in Angstroms

    Returns:
    --------
    dict : {residue_id: distance}
    """
    pass

def get_chimerax_selection(contacts: dict) -> str:
    """Convert contact dict to ChimeraX selection string."""
    pass
```

**Benefits:**
- Type hints + docstrings = professional code
- Testable, modular functions
- No hardcoded paths
- Importable by others: `from pdb_proximity import find_ligand_contacts`

---

### Phase 2: Create Visualization Module

**File: `pdb_proximity/visualization.py`**

```python
def plot_contact_distances(
    contacts: dict,
    cutoff: float,
    title: str = "Residue Contact Distances"
) -> matplotlib.figure.Figure:
    """Generate bar plot of contact distances."""
    pass

def plot_delta_distances(
    contacts_A: dict,
    contacts_B: dict
) -> matplotlib.figure.Figure:
    """Plot chain A vs B distance comparison."""
    pass

def visualize_3d_pymol(
    pdb_path: str,
    ligand_contacts: dict,
    interface_contacts: dict
):
    """Interactive 3D visualization with py3Dmol."""
    pass
```

---

### Phase 3: Create THE Showcase Notebook

**File: `examples/quickstart.ipynb`**

This single, comprehensive notebook should:

1. **Introduction Cell**
   - What the tool does
   - Use cases
   - Link to GitHub repo

2. **Installation Cell**
   ```python
   %pip install -r ../requirements.txt
   ```

3. **Load Example Data**
   ```python
   from pdb_proximity import calculator, visualization

   pdb_path = "example_data/example_protein.pdb"
   ```

4. **Example 1: Find Ligand Contacts**
   ```python
   contacts = calculator.find_ligand_contacts(
       structure=structure,
       ligand_selection={'chain': 'A', 'resid': 501},
       cutoff=4.0
   )

   # Display results as DataFrame
   # Generate ChimeraX selection
   # Plot distances
   ```

5. **Example 2: Find Interface Residues**
   ```python
   interface = calculator.find_interface_residues(
       structure=structure,
       chain_pairs=[('A', 'B')],
       cutoff=5.0
   )
   ```

6. **Example 3: Visualizations**
   - Bar plots
   - 3D interactive view
   - Delta distance comparison

7. **Export Results**
   - Save CSVs
   - Generate ChimeraX script
   - Export publication-quality figures

**Key: Run this notebook and commit the outputs** so GitHub displays results without running code!

---

### Phase 4: Write an Impressive README

**File: `README.md`**

Structure:

```markdown
# PDB Proximity Calculator

> Identify residues near ligands and protein interfaces from PDB structures

[![Python 3.8+](badge)]
[![License: MIT](badge)]

## Overview

Brief description + use case explanation.

## Features

- 🧬 Ligand contact analysis
- 🔗 Protein-protein interface detection
- 📊 Publication-quality visualizations
- 🎨 ChimeraX script generation
- 📦 Easy-to-use Python API

## Quick Start

### Installation

```bash
git clone https://github.com/yourusername/PDB-Proximity-Calculator
cd PDB-Proximity-Calculator
pip install -r requirements.txt
```

### Basic Usage

```python
from pdb_proximity import calculator

# Find residues within 4Å of ligand
contacts = calculator.find_ligand_contacts(
    pdb_path="your_structure.pdb",
    ligand_selection={'chain': 'A', 'resid': 501},
    cutoff=4.0
)

print(contacts)
# {'A:TYR 48': 2.45, 'A:PHE 52': 3.87, ...}
```

## Example Outputs

### Ligand Contact Analysis
![Ligand Contacts](examples/example_outputs/ligand_contacts.png)

### Interface Residue Detection
![Interface](examples/example_outputs/interface_residues.png)

### 3D Visualization
![3D View](examples/example_outputs/chimerax_visualization.png)

## Full Tutorial

See [examples/quickstart.ipynb](examples/quickstart.ipynb) for a complete walkthrough.

## Use Cases

- Identifying active site residues for mutagenesis studies
- Analyzing protein-protein binding interfaces
- Comparing ligand binding modes across structures
- Preparing selections for molecular dynamics simulations

## Citation

If you use this tool in your research, please cite...

## License

MIT License

## Contact

Questions? Open an issue or contact [email]
```

---

### Phase 5: Optional Enhancements (Employer Wow Factor)

#### A. Command-Line Interface
**File: `scripts/calculate_proximity.py`**

```bash
python scripts/calculate_proximity.py \
    --pdb myprotein.pdb \
    --ligand-chain A \
    --ligand-resid 501 \
    --ligand-cutoff 4.0 \
    --interface A B \
    --interface-cutoff 5.0 \
    --output results/
```

Shows you can build user-friendly tools.

#### B. Unit Tests
**File: `tests/test_calculator.py`**

```python
import pytest
from pdb_proximity import calculator

def test_ligand_contacts_returns_dict():
    result = calculator.find_ligand_contacts(...)
    assert isinstance(result, dict)

def test_cutoff_filters_correctly():
    ...
```

Shows you write production-quality code.

#### C. Make it pip-installable
**File: `setup.py` or `pyproject.toml`**

```bash
pip install git+https://github.com/yourusername/PDB-Proximity-Calculator
```

Super impressive for hiring managers.

---

## Migration Plan (Preserves History)

### Step 1: Create new structure
```bash
mkdir pdb_proximity examples scripts
```

### Step 2: Consolidate notebooks → One example
- Extract common logic from both notebooks
- Move to `pdb_proximity/calculator.py`
- Create single `examples/quickstart.ipynb` that imports the module

### Step 3: Add example data
- Pick ONE representative PDB file
- Place in `examples/example_data/`
- Update notebook to use this example

### Step 4: Generate example outputs
- Run the showcase notebook
- Save plots as PNG in `examples/example_outputs/`
- Export results table as CSV

### Step 5: Write README
- Add installation instructions
- Embed example output images
- Link to full tutorial notebook

### Step 6: Create requirements.txt
```
biopython>=1.83
scipy>=1.15.0
matplotlib>=3.8.0
pandas>=2.0.0
py3Dmol>=2.5.0
```

### Step 7: Clean up
- Remove old `Notebooks/` or keep as `legacy/`
- Remove `Inputs/` with hardcoded paths
- Add `.gitignore` for `*.pyc`, `__pycache__`, `.ipynb_checkpoints`

---

## Why This Structure Achieves Your Goals

### Goal 1: Showcase Code to Employers ✅
- **Professional structure**: Package layout shows you understand Python best practices
- **Clean code**: Functions with type hints, docstrings, no hardcoded paths
- **Documentation**: Comprehensive README, example notebook
- **Optional extras** (tests, CLI, pip-install) = senior-level competence

### Goal 2: Demonstrate Example Outputs ✅
- **Pre-run notebook**: GitHub displays outputs without running code
- **Images in README**: Immediate visual impact
- **Multiple examples**: Shows versatility of the tool
- **CSV exports**: Provides tangible results

### Goal 3: Universal/Reusable Tool ✅
- **No hardcoded paths**: Users provide their own PDB files
- **Clear API**: Simple function calls, well-documented parameters
- **Example data included**: Users can test immediately
- **Multiple interfaces**: Python API, CLI option, or interactive notebook
- **Comprehensive tutorial**: Lowers barrier to entry

---

## Alternative: Streamlit Web App (Advanced Option)

If you want to go next-level, create a web interface:

```python
# app.py
import streamlit as st
from pdb_proximity import calculator

st.title("PDB Proximity Calculator")
pdb_file = st.file_uploader("Upload PDB")
cutoff = st.slider("Distance cutoff (Å)", 2.0, 10.0, 4.0)

if pdb_file:
    contacts = calculator.find_ligand_contacts(pdb_file, cutoff=cutoff)
    st.dataframe(contacts)
    st.pyplot(visualization.plot_contact_distances(contacts))
```

Deploy to Streamlit Cloud for free → employers can USE your tool live!

---

## Recommended First Steps

1. ✅ Consolidate redundant notebooks into core logic
2. ✅ Create `pdb_proximity/calculator.py` with main functions
3. ✅ Build single showcase `examples/quickstart.ipynb`
4. ✅ Generate example outputs and commit them
5. ✅ Write impressive README with images
6. ✅ Add requirements.txt
7. Consider: CLI interface
8. Consider: Unit tests
9. Consider: Streamlit app

---

## Questions to Consider

1. **Target audience**: Structural biologists? Computational chemists? General bioinformatics?
2. **Scope**: Keep focused on proximity analysis, or add more PDB analysis features?
3. **Naming**: "PDB-Proximity-Calculator" vs "ProteinProximity" vs "StructureContacts"?
4. **License**: MIT (most permissive) vs GPL vs Academic?

---

## Timeline Estimate

- **Minimal viable showcase** (Goals 1-3): 4-6 hours
  - Extract core functions: 2h
  - Create example notebook: 1h
  - Write README: 1h
  - Polish: 1h

- **With optional enhancements**: 8-12 hours
  - Add CLI: +2h
  - Add tests: +2h
  - Make pip-installable: +1h
  - Streamlit app: +3h

---

## Final Recommendation

**Start with the "Minimal Viable Showcase"** - it hits all three goals and looks highly professional. The optional enhancements can be added incrementally based on time/interest.

The key insight: **Move from "scripts" to "package"**. This single change elevates the repo from "I can code" to "I can architect reusable software" - exactly what employers want to see.

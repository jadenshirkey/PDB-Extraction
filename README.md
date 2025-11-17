# PDB-Extraction

A computational toolkit for analyzing protein crystal structures to identify ligand binding sites, protein-protein interfaces, and conformational changes in biomolecular complexes.

## Overview

PDB-Extraction provides two complementary Jupyter notebooks for structural biology analysis of PDB (Protein Data Bank) files, with a focus on structures from the PISA (Protein Interfaces, Surfaces and Assemblies) server. The toolkit maps molecular interactions at atomic resolution and generates publication-ready visualizations.

### Key Capabilities

- **Ligand Binding Site Mapping**: Identify protein residues in direct contact with small molecule ligands
- **Dimer Interface Analysis**: Map residues mediating protein-protein interactions
- **Conformational State Comparison**: Quantify structural differences between protein chains (e.g., ON/OFF states)
- **3D Visualization**: Generate interactive molecular viewers and professional visualization scripts

## Repository Structure

```
PDB-Extraction/
├── Inputs/
│   └── PISA_interfaces/              # Input PDB files from PISA server
│       ├── AclR-w-C8-ACL_Phil2.pdb
│       ├── PISA_AclR+C8-ACL_A-A_OFF.pdb
│       └── PISA_AclR+C8-ACL_B-B_ON.pdb
├── Notebooks/
│   ├── PDB-Extraction.ipynb          # Advanced analysis with visualizations
│   ├── PISA-Extraction.ipynb         # Quick extraction tool
│   └── AclR_visualization.cxc        # Generated ChimeraX script
└── README.md
```

## The Two Analysis Workflows

### 1. PDB-Extraction.ipynb - Advanced Analysis & Visualization

**Use Case**: Deep dive analysis of specific protein structures with publication-ready outputs

**Features**:
- Comparative analysis between protein chains (Δ distance calculations)
- Multiple visualization formats:
  - Interactive 3D molecular viewer (py3Dmol)
  - Distance distribution plots (matplotlib)
  - ChimeraX visualization scripts for professional rendering
- Chain-specific contact identification (e.g., contacts unique to Chain A vs shared)
- Stricter distance cutoffs (2.5Å) for high-confidence interactions
- Detailed annotation of ON/OFF conformational states

**Output Examples**:
- Residue-by-residue distance plots showing ligand proximity
- Δ(B-A) plots revealing conformational changes between chains
- Color-coded molecular structure scripts
- Quantitative contact tables

**Best For**:
- Manuscript figure generation
- Detailed mechanistic studies
- Comparative structural analysis

### 2. PISA-Extraction.ipynb - Quick Extraction Tool

**Use Case**: Rapid screening of multiple structures to identify key interaction residues

**Features**:
- Automatic chain classification (protein vs ligand chains)
- Flexible distance cutoffs (4.0Å ligand, 5.0Å interface)
- Simple text output for easy parsing
- Adaptable to various PDB formats and chain naming conventions
- Minimal computational overhead

**Output Examples**:
- Lists of residues within proximity cutoffs
- Quick identification of binding site residues
- Interface residue catalogs

**Best For**:
- High-throughput structure screening
- Initial exploration of new structures
- Quick validation of predicted binding sites

## Installation

### Prerequisites

```bash
pip install biopython scipy pandas matplotlib py3Dmol
```

### Dependencies

- **BioPython** (≥1.83): PDB parsing and structure manipulation
- **SciPy** (≥1.15): Spatial distance calculations using k-d trees
- **Pandas**: Data organization and export
- **Matplotlib**: 2D plotting and visualization
- **py3Dmol** (≥2.5): Interactive 3D molecular visualization in Jupyter

## Usage

### Quick Start - Screening Tool

```python
# Open PISA-Extraction.ipynb
# Set your PDB file path
pdb_path = "path/to/your/structure.pdb"

# Set distance cutoffs (in Ångströms)
ligand_cutoff = 4.0      # Residues within 4Å of ligand
interface_cutoff = 4.0   # Interface residues within 4Å

# Run all cells to get residue lists
```

### Advanced Analysis

```python
# Open PDB-Extraction.ipynb
# Configure analysis parameters
pdb_path = "path/to/your/structure.pdb"
ligand_cutoff = 2.5
interface_cutoff = 2.5
ligand_resname = "LIG"   # 3-letter ligand code
ligand_resid = 501       # Residue number of ligand

# Run analysis to generate:
# - Interactive 3D viewer
# - Distance plots
# - ChimeraX visualization script
# - Comparative chain analysis
```

## Analysis Methodology

### Distance-Based Contact Detection

The toolkit uses k-d tree spatial indexing (scipy.spatial.cKDTree) for efficient proximity searches:

1. **Ligand Contacts**: For each ligand atom, identify protein atoms within the cutoff distance
2. **Interface Contacts**: Between protein chains, find residue pairs within the cutoff
3. **Minimum Distance**: Report the closest approach between any two atoms in the residue pair

### Chain Comparison

For structures with multiple chains representing different conformational states:
- **Δ Distance** = Distance(Chain B) - Distance(Chain A)
- Positive Δ: Residue farther from ligand/interface in Chain B
- Negative Δ: Residue closer to ligand/interface in Chain B
- Chain-specific contacts: Residues that contact only in one chain

## Example: AclR Protein Analysis

The included example analyzes the AclR transcription factor in complex with C8-ACL ligand:

**Findings**:
- Ligand contacts: 14 residues in Chain A, 13 in Chain B
- Key binding residue: TYR48 (2.45Å from ligand in Chain A)
- Dimer interface: 12 residues in each chain within 2.5Å
- Conformational differences: Chain-specific contacts suggest asymmetric ligand binding

**Generated Outputs**:
- ChimeraX script coloring ligand contacts (red) and interface residues (orange)
- Comparative plots showing differential binding between ON/OFF states

## Visualization Outputs

### ChimeraX Scripts

Generated `.cxc` files can be opened directly in [UCSF ChimeraX](https://www.cgl.ucsf.edu/chimerax/):

```bash
chimerax AclR_visualization.cxc
```

The script automatically:
- Loads the PDB structure
- Colors ligand contacts in red
- Colors interface residues in orange
- Highlights ligand in yellow
- Zooms to the region of interest

### Interactive 3D Viewer

The py3Dmol viewer runs directly in Jupyter notebooks, allowing you to:
- Rotate and zoom the structure interactively
- Toggle between cartoon and stick representations
- Inspect specific residues and contacts

## Customization

### Adjusting Distance Cutoffs

Different biological questions require different cutoffs:

- **Direct contacts**: 2.5-3.0Å (hydrogen bonding, van der Waals)
- **Nearby residues**: 4.0-5.0Å (second shell, electrostatic interactions)
- **Functional vicinity**: 6.0-8.0Å (allosteric effects, domain proximity)

### Adding New Analysis Types

The modular notebook structure allows easy extension:
- Modify the contact detection loop to filter by residue type
- Add solvent accessibility calculations
- Integrate binding energy estimations
- Export to other visualization formats (PyMOL, VMD)

## Applications

### Structural Biology Research

- Map binding sites for drug design
- Identify mutation hotspots at interfaces
- Validate computational docking predictions
- Understand allosteric mechanisms

### Protein Engineering

- Design stabilizing mutations at interfaces
- Engineer altered ligand specificity
- Create conformationally-biased variants

### Comparative Structural Analysis

- Compare homologous structures
- Analyze evolutionary conservation at binding sites
- Study conformational changes upon ligand binding

## Citation & Acknowledgments

This toolkit was developed for analyzing AclR transcription factor structures. If you use this code for your research, please acknowledge this repository.

PDB structures analyzed with PISA: [https://www.ebi.ac.uk/pdbe/pisa/](https://www.ebi.ac.uk/pdbe/pisa/)

## Future Development

Planned enhancements:
- [ ] Automated batch processing of multiple PDB files
- [ ] Integration of visualization outputs (embedded images in README)
- [ ] Export to standardized formats (CSV, JSON)
- [ ] Command-line interface for high-throughput analysis
- [ ] Hydrogen bond geometry analysis
- [ ] Solvent-accessible surface area calculations

## Contributing

Contributions are welcome! Areas for improvement:
- Additional visualization backends (PyMOL, VMD)
- Support for other structure file formats (mmCIF, mol2)
- Statistical analysis of contact distributions
- Machine learning integration for binding site prediction

## License

This project is open source and available for academic and research use.

## Contact

For questions, issues, or collaboration inquiries, please open an issue on this repository.

---

**Note**: Remember to update file paths in the notebooks to point to your local PDB files before running the analysis.

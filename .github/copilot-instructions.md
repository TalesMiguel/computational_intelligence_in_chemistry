# AI Coding Agent Instructions for Chemical Intelligence Project

## Project Overview
This is a computational chemistry research project focused on analyzing the QM9 quantum chemistry dataset (~134k small organic molecules). The project implements a complete pipeline for molecular data processing, visualization, and analysis using Python/Jupyter notebooks with RDKit for chemical informatics.

## Core Architecture & Data Flow

### Dataset Structure
- **QM9 Dataset**: 133,885 molecules from GDB-1 to GDB-9, stored as XYZ files in `db/dsgdb9nsd.xyz.tar.bz2`
- **File Format**: Custom XYZ format with 16 quantum mechanical properties per molecule
- **Key Properties**: Energies (U₀, U, H, G), HOMO/LUMO energies, dipole moment, rotational constants, etc.

### Data Pipeline (molecular_data_reading.ipynb)
1. **Data Extraction**: `unpack_database()` → `parse_xyz_file()` → `process_xyz_files()`
2. **SMILES Validation**: `validate_and_canonicalize_smiles()` using RDKit (invalid molecules are filtered out)
3. **Data Storage**: Final dataset saved as `qm9_complete_dataset.csv` with 17 properties + canonical SMILES

### Critical Parser Logic
```python
# XYZ file structure:
# Line 1: Number of atoms
# Line 2: 16 properties (handle 'gdb' prefix edge case)
# Line N-2: SMILES string (penultimate line)
smiles = lines[-2].split('\t')[0]  # Extract SMILES from penultimate line
```

## Key Dependencies & Tools

### Required Libraries
- **RDKit**: SMILES validation, molecular weight calculation, atom counting
- **Pandas**: Data manipulation and CSV export
- **Matplotlib + Seaborn**: Visualization (histograms, correlation heatmaps)
- **NumPy**: Numerical operations and log scaling

### Important Patterns
- **Log Scale Visualization**: Properties with wide ranges (`rot_const_A/B/C`, `isotropic_polarizability`) use log10 scaling
- **Correlation Analysis**: Single triangular heatmap approach (avoid multiple redundant visualizations)
- **Statistical Summaries**: Consistent format across all analysis sections

## Development Workflows

### Data Processing
```bash
# Unpack dataset (first time only)
# Process all XYZ files → validate SMILES → generate DataFrame
# Export to CSV for persistence
```

### Visualization Standards
- **Property Histograms**: 4x4 grid layout, log scaling for specific properties, statistical summaries
- **Molecular Composition**: 2x2 subplot analysis (size, elements, formulas, heteroatoms)
- **Correlation Analysis**: Single lower-triangular heatmap with focused statistical reporting

## Project-Specific Conventions

### File Organization
- `db/`: Contains QM9 dataset (gitignored due to size)
- `molecular_data_reading.ipynb`: Main analysis notebook
- `qm9_complete_dataset.csv`: Processed dataset output

### Code Patterns
- **Error Handling**: RDKit validation filters invalid molecules silently
- **Property Naming**: Use full descriptive names (`homo_energy`, `lumo_energy`, etc.)
- **Statistical Reporting**: Include mean, std, min, max + special metrics for log-scale properties

### Data Quality Checks
- SMILES validation is mandatory (use `Chem.MolFromSmiles()`)
- Handle edge cases: 'gdb' prefix in headers, duplicate SMILES entries
- Molecular weight calculated via `Descriptors.MolWt()` for correlation analysis

## Research Context
- **Citation Required**: QM9 dataset papers (Ramakrishnan et al., 2014; Ruddigkeit et al., 2012)
- **Use Case**: Machine learning preprocessing for molecular property prediction
- **Data Range**: 3-29 atoms, 16-152 g/mol molecular weight
- **Academic Setting**: Post-graduate computational intelligence course

## Common Tasks
When extending this project, focus on:
- Molecular descriptor calculation using RDKit
- Property prediction model development
- Chemical space analysis and visualization
- Correlation analysis with additional computed features

Avoid redundant visualization approaches - favor single, comprehensive charts over multiple similar plots. Also, avoid using emojis and informal comments in code cells. Be concise and maintain a professional tone throughout the notebook. Do not use plural forms in section titles.

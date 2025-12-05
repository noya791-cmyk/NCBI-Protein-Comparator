# NCBI Protein Comparator

A Python application that allows users to search the NCBI Protein database for a specific protein and compare its amino acid sequences between humans and zebrafish.

## Project Overview

This program enables researchers and students to:

1. Enter the name of a specific protein
2. Search the NCBI Protein database to find the amino acid sequences
3. Compare the protein sequences between humans and zebrafish
4. View the similarity percentage and composition analysis

The application features a clean separation between business logic and user interface, allowing the core functionality to be reused independently of the GUI.

## Inspiration

This project was developed to support research on conserved genes between humans and zebrafish, particularly for studying disease mechanisms using zebrafish as a model organism. It serves as a pipeline tool for screening genes and identifying sequence conservation across species.

## Features

- **NCBI Integration**: Direct access to the NCBI Protein database for real-time protein searches
- **Multi-organism Comparison**: Automatically searches for the same protein in both humans and zebrafish
- **Sequence Analysis**: Calculates identity percentages and similarity ratios
- **User-Friendly GUI**: Simple, intuitive interface built with tkinter
- **NCBI Rate Limiting**: Respects NCBI's rate limits (0.4 seconds between requests) to avoid connection issues

## Project Structure

```
NCBI-Protein-Comparator/
├── business_logic/           # Core logic (no UI dependencies)
│   ├── __init__.py
│   ├── protein_comparator.py # Main orchestrator
│   ├── ncbi_protein_fetcher.py # NCBI database access
│   └── sequence_analyzer.py  # Sequence comparison
│
├── ui/                       # User interface
│   ├── __init__.py
│   └── gui.py               # tkinter GUI implementation
│
├── main.py                  # Application entry point
├── config.py               # Configuration settings
├── requirements.txt        # Python dependencies
├── .gitignore             # Git ignore file (excludes __pycache__, venv, etc.)
└── README.md              # This file
```

## Installation

### Prerequisites
- Python 3.7 or higher
- pip package manager

### Setup

1. Navigate to the project directory:
   ```bash
   cd NCBI-Protein-Comparator
   ```

2. (Optional) Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

Launch the GUI application:

```bash
python main.py
```

### How to Use

1. Enter a protein name (e.g., "hemoglobin", "insulin", "myoglobin")
2. Click "Search & Compare" to search NCBI
3. View the results showing:
   - Whether the protein was found in each organism
   - Protein accession IDs
   - Sequence lengths
   - Identity percentage between sequences
   - Similarity interpretation

## Project Structure

day04/project/
  
  business_logic/              - Core logic (NO UI dependencies)
    __init__.py               - Package initialization
    protein_comparator.py     - Main orchestrator
    ncbi_protein_fetcher.py   - NCBI database access
    sequence_analyzer.py      - Sequence comparison
  
  ui/                         - User interface
    __init__.py               - Package initialization
    gui.py                    - tkinter GUI implementation
  
  main.py                     - Entry point
  config.py                   - Configuration
  requirements.txt            - Dependencies
  README.md                   - This file

## Module Documentation

ncbi_protein_fetcher.py

ProteinFetcher class:
  search_protein(protein_name, organism)
    Search for a protein in a specific organism

  fetch_sequence(accession_id)
    Retrieve the amino acid sequence

  get_protein_in_organisms(protein_name, organisms)
    Find a protein in multiple organisms

Features:
  Automatic rate limiting for NCBI API (0.4 seconds between requests)
  Error handling for network and API issues
  Returns accession IDs, sequences, titles, and metadata

sequence_analyzer.py

SequenceAnalyzer class:
  calculate_identity_percentage()
    Calculates percentage of identical amino acids

  calculate_similarity_score()
    Computes similarity ratio using sequence matching

  get_sequence_statistics()
    Analyzes amino acid composition

  compare_sequences()
    Comprehensive comparison of two sequences

protein_comparator.py

ProteinComparator class:
  search_and_compare()
    Coordinates search across multiple organisms

  compare_sequences()
    Initiates sequence comparison

  format_results()
    Prepares results for display

## Example Usage

Command Line (Python script)

   from business_logic import ProteinComparator

   # Initialize
   comparator = ProteinComparator()

   # Search for hemoglobin
   results = comparator.search_and_compare(
       "hemoglobin",
       ["Homo sapiens", "Danio rerio"]
   )

   # Compare if both found
   if results["Homo sapiens"] and results["Danio rerio"]:
       comparison = comparator.compare_sequences(
           results["Homo sapiens"],
           results["Danio rerio"]
       )
       print(f"Identity: {comparison['identity_percentage']:.2f}%")

## Dependencies

- **biopython**: For NCBI interaction and sequence handling
- **requests**: For HTTP requests (installed as a dependency of biopython)

See `requirements.txt` for specific version requirements.

## Important Notes

### NCBI Guidelines
- This application respects NCBI's usage guidelines through rate limiting
- An email address is required by NCBI and is set to "student@python-course.com" by default
- For production use, update the email in `config.py`

### Search Tips
- Use common protein names (e.g., "hemoglobin", "insulin")
- More specific searches are often more successful
- Some proteins may only be annotated in one organism

### Known Limitations
- Uses simple sequence matching (not advanced alignment algorithms like BLAST)
- Limited to the first matching result for each organism
- Requires internet connection for NCBI access

## Interpreting Results

- **>80% Identity**: High similarity, likely homologous proteins
- **50-80% Identity**: Moderate similarity, probable function conservation
- **<50% Identity**: Low similarity, distantly related proteins

## Troubleshooting

### "NCBI search error"
- Check your internet connection
- Ensure NCBI servers are accessible
- Verify you have recent versions of required packages

### "Protein not found"
- Try a different protein name or variant
- Use the full protein name instead of abbreviations
- Some proteins may not be in the NCBI database

### "No sequences returned"
- NCBI may have the protein but without sequence data
- Try searching for a different protein variant

## Architecture

### Clean Separation of Concerns

**business_logic/** - Pure Python modules with NO UI dependencies
- Can be used independently in CLI, Web, or other applications
- Easy to test without GUI
- Can be imported and reused in other projects

**ui/** - GUI Implementation
- Depends only on business_logic
- Handles all user interaction and display
- Can be replaced with alternative UI frameworks (Qt, Web, etc.)

## .gitignore

The project includes a comprehensive `.gitignore` file that excludes:
- Python cache directories (`__pycache__/`)
- Compiled Python files (`*.pyc`, `*.pyo`)
- Virtual environments (`venv/`, `.venv/`)
- IDE configuration files (`.vscode/`, `.idea/`)
- Build artifacts (`build/`, `dist/`, `*.egg-info/`)
- Environment variables files (`.env`, `.env.local`)
- Temporary files (`*.log`, `*.tmp`, `*.cache`)

This ensures only source code and essential configuration files are committed to GitHub.

## Future Enhancements

- Support for additional organisms
- Advanced alignment algorithms (MUSCLE, ClustalW)
- Multiple sequence alignment visualization
- Export results to FASTA format
- Phylogenetic tree generation
- Web-based interface (Flask/Django)
- Command-line interface (CLI)

## References

- [NCBI Entrez API Documentation](https://www.ncbi.nlm.nih.gov/books/NBK25499/)
- [BioPython Documentation](https://biopython.org/)
- [NCBI Protein Database](https://www.ncbi.nlm.nih.gov/protein/)

## License

This educational project is provided as-is for learning and research purposes.

## Author

Created for Python course assignments - Educational Project

Last Updated: December 2025

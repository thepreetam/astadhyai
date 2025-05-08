# Sanskrit NLP Toolkit

A Python library for processing Sanskrit text, incorporating linguistic features like Sandhi-aware tokenization and morphological analysis.

Based on the ideas presented in the thesis: "Sanskrit-Powered AI: The Linguistic Key to Collective Intelligence and Human Evolution".

## Project Vision and Context

This toolkit serves as a practical implementation of the symbolic core of the hybrid symbolic-neural framework detailed in the research paper: "Reviving Vyākaraṇa in the Digital Age: A Hybrid Symbolic-Neural Framework for Sanskrit Computing".

The core philosophy is to model Sanskrit as a **computational grammar**, moving beyond purely statistical approaches. Key aspects of this project that align directly with the paper include:

*   **Symbolic Rule Engine:** Implementing Pāṇinian grammar principles through declarative rules for components like the `SandhiSplitter` and `MorphologicalAnalyzer`.
*   **Structured Intermediate Representations (IRs):** Utilizing well-defined data structures, such as the `MorphologicalAnalysis` dataclass, to capture linguistic features in a modular and scalable way.
*   **Declarative Rule Systems:** Employing human-readable formats (like YAML for morphological rules) and schema validation (e.g., Pydantic models) to define and manage grammatical rules, facilitating collaboration and maintainability.

The `sanskrit_nlp_toolkit` aims to build the foundational symbolic layer. Future development, in line with the paper's vision, could involve integrating neural models for ambiguity resolution and expanding the coverage of Pāṇinian sūtras to achieve a comprehensive Sanskrit processing pipeline.

### Progress Towards Broader Vision

The following table summarizes the current status of the `sanskrit_nlp_toolkit` in relation to the broader goals outlined in the "Reviving Vyākaraṇa in the Digital Age" paper:

| Feature/Component                 | Current `sanskrit_nlp_toolkit` Status                                  | Paper's Broader Vision                                                                                                |
|-----------------------------------|------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Overall Framework**             | Symbolic core under development                                        | Hybrid Symbolic-Neural framework                                                                                      |
| **Sandhi Splitting**              | Rule-based (regex) splitter implemented                                | High-accuracy sandhi splitting, potentially enhanced by learned rules or neural components                            |
| **Morphological Analysis IR**     | `MorphologicalAnalysis` dataclass implemented (matches paper)          | Standardized IR for detailed morphological features, enabling modular downstream tasks                              |
| **Lexicon**                       | `SanskritLexicon` implemented (loads `dhatu.csv`, `pratipadika.json`)    | Comprehensive digital lexicon of roots (dhātus), stems (prātipadikas), and their grammatical properties         |
| **Declarative Rule System**     | YAML-based rules for morphology with Pydantic validation implemented   | Extensive, human-readable, and maintainable rule sets covering various grammatical phenomena (linked to Pāṇini)       |
| **Morphological Analyzer Engine** | Basic lexicon-driven `_analyze_subanta` implemented; rule application TBD | Sophisticated engine applying grammatical rules to derive all possible analyses for subanta, tiṅanta, kṛdanta, taddhita |
| **Pāṇinian Sūtra Coverage**     | Rule structure allows `sutra_ref`; initial rules are high-level         | Encoding a significant portion of Pāṇini's Aṣṭādhyāyī (approx. 4,000 sūtras) as declarative constraints            |
| **Neural Ambiguity Resolution**   | Not yet implemented                                                    | Integration of lightweight LLMs to rank/disambiguate analyses from the symbolic engine where multiple options exist |
| **Syntactic Parsing**             | Not yet implemented                                                    | Generation of dependency structures based on kāraka roles and other syntactic rules                                 |
| **End Applications**              | Foundational library stage                                             | OCR enhancement, Vedic algorithm reconstruction, semantic search, machine translation, EdTech tools                 |

## Features (Planned)

- Sandhi-aware Tokenizer
- Morphological Analyzer (Root identification, Vibhakti analysis) - *Initial implementation complete*
- Pāṇinian Grammar Parser (Future Goal)

## Installation

To set up the project for development or use:

1.  **Clone the repository (if you haven't already):**
    ```bash
    git clone https://github.com/thepreetam/AshtadhyAI.git
    cd AshtadhyAI/sanskrit_nlp_toolkit
    ```
2.  **Create and activate a virtual environment (recommended):**
    ```bash
    python3 -m venv .venv
    source .venv/bin/activate  # On Windows use: .venv\Scripts\activate
    ```
3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    # Ensure PyYAML, Pydantic, and pytest are listed in requirements.txt
    # If not, you might need to install them manually for now:
    # pip install pyyaml pydantic pytest pandas
    ```

## Usage

### Sandhi Splitting

The `SandhiSplitter` class provides functionality to split Sanskrit words that might be joined by Sandhi rules. It uses regex-based rules defined in `sanskrit_nlp/sandhi/rules/`.

```python
from sanskrit_nlp.sandhi.splitter import SandhiSplitter

splitter = SandhiSplitter()

word = "devālayaḥ" # Example: deva + ālayaḥ (Savarna Dīrgha Sandhi)
splits = splitter.split(word)

if splits:
    print(f"Possible splits for '{word}':")
    for s in splits:
        print(f"  -> {s.first_part} + {s.second_part}  (Rule: {s.rule_applied}, Confidence: {s.confidence:.2f})")
else:
    print(f"No sandhi splits found for '{word}'.")

# Example Output (may vary based on rules):
# Possible splits for 'devālayaḥ':
#   -> deva + ālayaḥ  (Rule: 6.1.101: Savarna Dirgha (ā), Confidence: 0.90)
#   -> devā + layaḥ  (Rule: Basic Split, Confidence: 0.20)
#   -> devāl + ayaḥ  (Rule: Basic Split, Confidence: 0.20)
#   ... other basic splits ...

```

### Morphological Analysis

The `MorphologicalAnalyzer` class provides functionality to perform morphological analysis of Sanskrit words, identifying their root, part of speech, and other grammatical features. It uses a lexicon and a set of morphological rules.

```python
from sanskrit_nlp.lexicon.loader import SanskritLexicon
from sanskrit_nlp.rules.loader import load_morph_rules
from sanskrit_nlp.morphology.analyzer import MorphologicalAnalyzer

# Initialize lexicon and load rules
# (Assuming lexicon files are in 'sanskrit_nlp/lexicon/data/' and rules in 'sanskrit_nlp/rules/data/')
# Adjust paths as per your project structure if running this example standalone.
# For library usage, these paths would typically be relative to the package.

# Create dummy lexicon and rule files for this example if they don't exist
# For actual use, ensure dhatu.csv, pratipadika.json, and morph_rules.yaml are populated.

# Lexicon paths (example, adjust if needed)
dhatu_file = 'sanskrit_nlp/lexicon/data/dhatu.csv'
pratipadika_file = 'sanskrit_nlp/lexicon/data/pratipadika.json'

# Rules path (example, adjust if needed)
rules_file = 'sanskrit_nlp/rules/data/morph_rules.yaml'

# Ensure dummy files exist for the example to run without full data setup
import os
from pathlib import Path

# Create dummy data directories if they don't exist
Path(dhatu_file).parent.mkdir(parents=True, exist_ok=True)
Path(pratipadika_file).parent.mkdir(parents=True, exist_ok=True)
Path(rules_file).parent.mkdir(parents=True, exist_ok=True)

# Create dummy dhatu.csv
with open(dhatu_file, 'w', encoding='utf-8') as f:
    f.write("root,gana,meaning\n")
    f.write("bhū,1,to be\n")
    f.write("gam,1,to go\n")

# Create dummy pratipadika.json
with open(pratipadika_file, 'w', encoding='utf-8') as f:
    f.write("""
{
  "stems": [
    {
      "stem": "rāma",
      "pos": "noun",
      "gender": "masculine",
      "declension_pattern_ref": "a_masc",
      "meanings": ["Rama"]
    },
    {
      "stem": "phala",
      "pos": "noun",
      "gender": "neuter",
      "declension_pattern_ref": "a_neuter",
      "meanings": ["fruit"]
    }
  ],
  "declension_patterns": {
    "a_masc": {
      "sg": {
        "1": "aḥ", "2": "am", "3": "ena", "4": "āya", "5": "āt", "6": "asya", "7": "e", "8": "a"
      }
    },
    "a_neuter": {
      "sg": {
        "1": "am", "2": "am", "3": "ena", "4": "āya", "5": "āt", "6": "asya", "7": "e", "8": "a"
      }
    }
  }
}
    """)

# Create dummy morph_rules.yaml
with open(rules_file, 'w', encoding='utf-8') as f:
    f.write("""
rule_set_id: basic_morph_rules
description: "Basic morphological rules for testing"
rules:
  - rule_id: SUB_TEST_001
    sutra_ref: "N/A"
    description: "Test rule for subanta identification"
    conditions:
      - field: "word_ending"
        operator: "equals"
        value: "aḥ"
    actions:
      - operation: "set_field"
        field: "pos"
        value: "noun"
      - operation: "set_field"
        field: "vibhakti"
        value: "1st (Prathamā)"
      - operation: "set_field"
        field: "vacana"
        value: "singular (Eka)"
    confidence_modifier: 0.8
    notes: "A simple test rule."
    """)

# Initialize the analyzer
lexicon = SanskritLexicon(dhatu_file_path=dhatu_file, pratipadika_file_path=pratipadika_file)
rules = load_morph_rules(rules_file_path=rules_file)
analyzer = MorphologicalAnalyzer(lexicon=lexicon, rules_data=rules)

word_to_analyze = "rāmaḥ"
analyses = analyzer.analyze(word_to_analyze)

if analyses:
    print(f"Possible analyses for '{word_to_analyze}':")
    for analysis in analyses:
        print(f"  Root: {analysis.root}, POS: {analysis.pos}, Vibhakti: {analysis.vibhakti}, Vacana: {analysis.vacana}, Gender: {analysis.gender}")
else:
    print(f"No morphological analysis found for '{word_to_analyze}'.")

# Example Output (will depend on actual lexicon and rule implementation):
# Possible analyses for 'rāmaḥ':
#   Root: rāma, POS: noun, Vibhakti: 1st (Prathamā), Vacana: singular (Eka), Gender: masculine

```

## Development

To contribute to development:

1.  Follow the installation steps above.
2.  **Run tests:**
    ```bash
    pytest tests/
    ```
(More details on contributing, code style, etc., TBD) 

# Progress updates and TTDs for sanskrit_nlp_toolkit

This document outlines the major tasks and sub-tasks to guide the `sanskrit_nlp_toolkit` project towards its envisioned end state: a comprehensive symbolic (and eventually hybrid symbolic-neural) Sanskrit NLP system.

## I. Core Symbolic Engine Development

### A. Sandhi Splitter
- [ ] Enhance rule set for comprehensive coverage of vowel, consonant, and visarga sandhi.
- [ ] Implement a mechanism for rule ordering and conflict resolution (e.g., rule priorities).
- [ ] Add more sophisticated confidence scoring based on rule type, phonetic likelihood, etc.
- [ ] Test against diverse Sanskrit corpora and refine rules for accuracy and edge cases.
- [ ] Consider integrating known sandhi exceptions.

### B. Morphological Analyzer
- **Subanta (Nominal Forms)**
    - [x] Initial lexicon-driven analysis implemented.
    - [x] Initial rule application logic implemented.
    - [ ] Expand `pratipadika.json` lexicon with a wide variety of nominal stems (different endings, genders).
    - [ ] Develop comprehensive `subanta.yaml` rules based on Pāṇinian grammar for all vibhaktis, vacanas, and genders.
    - [ ] Implement handling for irregular nominal forms and exceptions noted in grammar texts.
    - [ ] Address pronoun declensions and special forms.
- **Tiṅanta (Verbal Forms)**
    - [ ] Refine lexicon structure for `dhatu.csv` to robustly include gaṇa (conjugational class), pada (voice - Parasmaipada, Ātmanepada, Ubhayapada), vikaraṇa (class infix) patterns, and linking to lakāra (tense/mood) endings. (Partially done)
    - [ ] Implement the `_analyze_tinanta` method in `MorphologicalAnalyzer.py`.
    - [ ] Develop lexicon-driven matching for verb roots and their specific conjugational endings.
    - [ ] Create `tinanta.yaml` rules for all 10 lakāras, covering different puruṣas (persons) and vacanas (numbers).
    - [ ] Ensure rules correctly handle the 10 gaṇas and their specific conjugational patterns (vikaraṇas).
    - [ ] Incorporate analysis of upasargas (prefixes) and their impact on verb meaning and form.
    - [ ] Implement analysis for passive voice (karmaṇi prayoga) and impersonal voice (bhāve prayoga).
    - [ ] Handle causative (ṇijanta), desiderative (sannanta), intensive (yaṅanta), and denominative (nāmadhātu) verb forms.
- **Kṛdanta (Primary Participles/Derivatives)**
    - [ ] Implement the `_analyze_krdanta` method.
    - [ ] Define lexicon entries or rules for common kṛt pratyayas (e.g., -ktvā, -lyap, -tumun, -tavya, -anīya, -śatṛ, -śānac, -kta, -ktavatu).
    - [ ] Develop `krdanta.yaml` rules for their formation, meaning, and grammatical properties (e.g., how they decline like adjectives).
- **Taddhitānta (Secondary Derivatives)**
    - [ ] Implement the `_analyze_taddhita` method.
    - [ ] Define lexicon entries or rules for common taddhita pratyayas (e.g., -aṇ, -mayaṭ, -vatup, -tva, -tal).
    - [ ] Develop `taddhita.yaml` rules for their formation, meaning, and how they form new nominal stems.
- **Avyaya (Indeclinables)**
    - [ ] Implement robust `_analyze_avyaya` method or integrate checks into the main `analyze` flow and lexicon.
    - [ ] Populate lexicon with a comprehensive list of common avyayas (adverbs, conjunctions, interjections).
    - [ ] Handle upasargas when used as standalone avyayas.
    - [ ] Identify and analyze nipātas (particles).

### C. Lexicon Management
- [x] Initial `SanskritLexicon` loader for `dhatu.csv` and `pratipadika.json`.
- [ ] Develop tools or scripts for easier lexicon building, validation, data entry, and expansion.
- [ ] Curate and integrate larger, more comprehensive open-source lexical resources for Sanskrit.
- [ ] Define and document a clear, extensible schema for all lexicon data files.
- [ ] Implement versioning for lexicon data.

### D. Rule System and Engine
- [x] YAML-based rule definition with Pydantic models for validation.
- [x] Initial rule loader and application logic within `MorphologicalAnalyzer`.
- [ ] Enhance rule condition checking capabilities (e.g., more operators like 'not', 'in_list', 'startswith', 'regex_fullmatch'; ability to check across multiple analysis fields or properties of the word segment).
- [ ] Enhance rule action capabilities (e.g., arithmetic operations on confidence, conditional actions based on intermediate states, list manipulation actions like 'prepend' or 'remove').
- [ ] Implement robust rule ordering and conflict resolution strategies (e.g., explicit rule priorities, specificity calculations based on condition count/type).
- [ ] Improve tracing capabilities to show exactly which rules (and their conditions/actions) contributed to a final analysis. (Partially done with `rule_references`)
- [ ] Optimize rule matching for performance with large rule sets.

## II. Expanding Grammatical Coverage (Pāṇinian Alignment)
- [ ] Systematically map and encode Pāṇinian sūtras relevant to morphology (sandhi, subanta, tiṅanta, kṛdanta, taddhita) into the YAML rule format.
    - [ ] Prioritize encoding core sūtras and then expand to more specific ones.
    - [ ] Develop a consistent methodology for translating sūtra logic into declarative rules.
- [ ] Model and implement handling for complex Pāṇinian rule interactions (e.g., utsarga-apavāda, pratiṣedha, vibhāṣā, adhikāra).
- [ ] Incorporate rules for internal sandhi (pada-madhya-sandhi) that occur during word derivation.
- [ ] Address common irregular forms and exceptions as documented in Vārttikas, Mahābhāṣya, and Kāśikā.
- [ ] Ensure the `sutra_ref` field in rules accurately links to Pāṇinian sūtra numbers and potentially other grammatical references.
- [ ] Create a validation system to check consistency and potential conflicts between encoded Pāṇinian rules.

## III. Syntactic Analysis (Building on Morphology)

### A. Kāraka Analysis (Semantic Roles)
- [ ] Design a detailed Intermediate Representation (IR) for Kāraka relations (e.g., building upon `DependencyParse` from the "Reviving Vyākaraṇa..." paper).
- [ ] Develop `karaka.yaml` rules (or a dedicated engine) to identify kāraka roles (kartṛ, karma, karaṇa, sampradāna, apādāna, adhikaraṇa) based on vibhakti, verb semantics (from dhātu lexicon), and other syntactic cues (e.g., presence of specific avyayas).
- [ ] Implement a Kāraka analysis module that takes morphological analyses as input.
- [ ] Handle complexities like gauṇa-mukhya karma (primary/secondary objects).

### B. Samāsa (Compound Word) Analysis
- [ ] Design an IR for compound word analysis, capturing constituent words (pūrva-pada, uttara-pada), their morphological analyses, and the type of samāsa.
- [ ] Develop rules (`samasa.yaml`?) or algorithmic approaches for identifying and decomposing common samāsa types:
    - [ ] Tatpuruṣa (including Karmadhāraya and Dvigu)
    - [ ] Bahuvrīhi
    - [ ] Dvandva
    - [ ] Avyayībhāva
- [ ] Implement a samāsa analysis module.
- [ ] Integrate with the Sandhi splitter to handle external sandhi between compound members if they were initially split, or internal sandhi if the compound is treated as a single unit for morphological analysis first.
- [ ] Address challenges in samāsa analysis like ambiguity in splitting points and type identification.

## IV. Neural Integration (Hybrid Model - Long-Term Vision)
- [ ] Research and develop a clear strategy for integrating neural models with the symbolic core.
- **Ambiguity Resolution**
    - [ ] Train or fine-tune a lightweight Language Model (LLM) to rank multiple valid analyses produced by the symbolic engine, using sentence or document context.
    - [ ] Explore using neural models to propose default analyses or suggestions when the symbolic engine cannot find a high-confidence match.
- **Contextual Understanding & Confidence Boosting**
    - [ ] Investigate how neural embeddings or contextual information can be used to dynamically adjust confidence scores of symbolic analyses.
    - [ ] Use neural models to guide the symbolic engine in complex cases by predicting likely paths or rule applications.
- **Data Augmentation & Generation**
    - [ ] Leverage the symbolic engine to generate grammatically correct synthetic Sanskrit data, which can then be used to train or fine-tune neural components.
- **End-to-End Hybrid Models**
    - [ ] Design and prototype end-to-end architectures where neural and symbolic components collaborate throughout the analysis pipeline.

## V. Tooling, Testing, and Infrastructure

### A. Testing Framework
- [x] Initial `pytest` setup with basic unit tests for lexicon and rule loaders.
- [ ] Develop comprehensive unit tests for all core modules: SandhiSplitter, MorphologicalAnalyzer (all sub-analyzers), Lexicon, Rule Engine, IRs.
- [ ] Create extensive integration tests for the end-to-end analysis pipeline (e.g., raw word -> sandhi splits -> morphological analyses -> (future) syntactic analysis).
- [ ] Build a benchmark dataset of Sanskrit words and sentences with gold-standard annotations for sandhi, morphology, and (eventually) syntax.
- [ ] Implement evaluation scripts to calculate metrics (accuracy, F1-score, etc.) against the benchmark dataset.
- [ ] Set up a CI/CD pipeline (e.g., GitHub Actions) for automated testing, linting (e.g., Black, Flake8, MyPy), and code quality checks.

### B. Data Management
- [ ] Establish a clear, version-controlled process for curating, updating, and validating all linguistic data (lexicon files, rule YAMLs).
- [ ] Consider using a more robust storage solution (e.g., a simple database or structured file formats like Parquet) for very large lexical resources.
- [ ] Develop scripts for converting/importing data from external linguistic resources.

### C. Documentation
- [x] Initial `README.md` providing an overview, setup, and basic usage examples.
- [x] `ripple.md` outlining the broader vision and context.
- [ ] Generate detailed API documentation for all public classes, methods, and functions (e.g., using Sphinx with autodoc).
- [ ] Write comprehensive user guides explaining how to use the toolkit, including advanced features and customization.
- [ ] Create clear contribution guidelines for new developers (coding standards, testing requirements, pull request process, DCO/CLA).
- [ ] Provide in-depth documentation for the rule YAML schema and lexicon data formats, with examples.
- [ ] Maintain a `CHANGELOG.md` to track notable changes across versions.

### D. Packaging and Distribution
- [ ] Fully configure `setup.py` or `pyproject.toml` for robust packaging of the library.
- [ ] Ensure all necessary data files (or a default set) are included in the package distribution.
- [ ] Publish the `sanskrit_nlp_toolkit` to the Python Package Index (PyPI) for easy installation.
- [ ] Implement versioning for the toolkit releases (e.g., semantic versioning).

### E. Performance Optimization
- [ ] Profile key components of the toolkit to identify performance bottlenecks (e.g., regex matching in sandhi, rule iteration in morphology).
- [ ] Optimize critical code paths for speed and memory efficiency, especially for batch processing of large texts.
- [ ] Consider techniques like rule compilation or indexing for faster rule lookup if needed.

## VI. Applications and Demonstrations (Realizing the Vision)
- [ ] Develop example scripts and Jupyter notebooks showcasing various features of the toolkit.
- [ ] Create a simple command-line interface (CLI) for basic sandhi splitting and morphological analysis.
- [ ] Build a small web application or API endpoint to demonstrate the toolkit's capabilities online.
- [ ] Explore and prototype integrations with other tools or projects, e.g.:
    - [ ] A module for improving Sanskrit OCR output by validating/correcting recognized words.
    - [ ] Basic support for generating inflected forms from a root and grammatical features (reverse of analysis).
- [ ] Write blog posts, articles, or tutorials to share the project's progress and encourage adoption.

## VII. Project Management and Community
- [ ] Maintain and regularly update this `TTD.md` as the project's public roadmap.
- [ ] Utilize GitHub Issues effectively for bug tracking, feature requests, and task management.
- [ ] Define project milestones and development sprints if working with a team.
- [ ] Foster an open and welcoming community for users and contributors (e.g., via GitHub Discussions, Gitter, or a mailing list).
- [ ] Actively seek collaboration with Sanskrit scholars, linguists, and other NLP researchers.
- [ ] Ensure clear licensing for the code and data (e.g., MIT, Apache 2.0 for code; CC licenses for data where appropriate). 

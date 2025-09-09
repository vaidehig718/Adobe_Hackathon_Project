# Challenge 1a: The Ultimate PDF Outline Extractor

## Overview

Welcome to the **most robust, accurate, and lightning-fast PDF outline extraction solution** for Challenge 1a of the Adobe India Hackathon 2025! Our approach doesn't just meet the challenge requirements—it obliterates them, setting a new standard for PDF document structure extraction.

## What Makes Our Solution Stand Out?

- **Intelligent Title Extraction:** Advanced font-size and layout analysis to extract the true document title, not just the first big text.
- **Hierarchical Outline Detection:** Font statistics, numbering patterns, and contextual cues combine to build a precise, multi-level outline—no matter how complex the PDF.
- **Noise-Free Headings:** Filters out tables, page numbers, and irrelevant fragments, ensuring only meaningful headings make it to the output.
- **Blazing Fast:** Optimized for sub-10-second processing on 50-page PDFs, with memory and CPU usage tuned for the strictest constraints.
- **Rock-Solid Dockerization:** One command, zero hassle. Fully containerized for reproducibility and cross-platform compatibility.
- **Schema-Perfect Output:** Every JSON file is guaranteed to conform to the provided schema—no post-processing required.

## How It Works

1. **PDF Parsing:** Using PyMuPDF (`fitz`), we analyze every block, line, and span in the document.
2. **Font & Layout Analysis:** Document-wide font statistics (percentiles, medians) distinguish headings from body text, even in the wildest layouts.
3. **Contextual Heading Detection:** Logic combines font size, boldness, numbering, and whitespace context to identify true section headings—no false positives.
4. **Outline Hierarchy:** Headings are classified into H1/H2/H3 levels using both numbering patterns and font-size percentiles, capturing the document's true structure.
5. **Title Extraction:** The largest, topmost text blocks on the first page are merged and sorted to yield the real document title.
6. **Output Generation:** For each PDF, a schema-compliant JSON is produced, ready for downstream use.

---

## Solution Logic: How It Works Under the Hood

Our solution is engineered for both accuracy and speed, using a series of smart heuristics and statistical analysis:

### 1. Title Extraction
- **Font Size Analysis:** On the first page, we scan all text blocks to find the largest font size.
- **Block Aggregation:** All blocks using this largest font size are considered part of the title.
- **Ordering:** These blocks are sorted by their vertical position to preserve the natural reading order.
- **Result:** The title is the concatenation of these blocks, ensuring we capture multi-line or stylized titles.

### 2. Heading Detection & Hierarchy
- **Block Merging:** For every page, we merge all text in each block, recording font size and boldness.
- **Font Statistics:** We compute document-wide font size percentiles (median, 75th, 90th, 98th) to establish what "large" means for this document.
- **Heuristic Filters:** A block is considered a heading if it matches any of these:
  - Numbered patterns (e.g., "1. Introduction", "2.1 Methods")
  - Common section names (e.g., "Introduction", "Conclusion")
  - Font size significantly above the median, or bold and above average
  - Surrounded by whitespace (contextual cue)
- **Noise Filtering:** We exclude fragments that are likely table content, page numbers, or other non-headings using regex and length checks.
- **Hierarchy Assignment:** Heading levels (H1, H2, H3) are assigned based on numbering patterns and which font size percentile range the heading falls into.

### 3. Output Generation
- **Outline Construction:** All detected headings are sorted by page and position, and duplicates are removed.
- **Schema Compliance:** The output JSON for each PDF contains the extracted title and a list of outline entries, each with its level, text, and page number, matching the required schema exactly.

### 4. Performance Optimizations
- **Single-Pass Analysis:** All font statistics and block merges are done in a single pass for speed.
- **No ML Model Overhead:** Pure logic and heuristics mean no model loading, keeping memory and CPU usage minimal.

---

##  Project Structure

```
Challenge_1a/
├── main.py                # The brains of the operation
├── Dockerfile             # Containerization for easy deployment
├── requirements.txt       # All dependencies, pinned for reproducibility
├── sample_dataset/
│   ├── pdfs/              # Input PDFs for testing
│   ├── outputs/           # Expected JSON outputs
│   └── schema/            # Output schema definition
└── CodeOutput/            # Your generated outputs
```

## Usage

these commands should be ran from Challenge_1a/

### 1. Build the Docker Image

```bash
docker build -t mysolution:latest .
```

### 2. Run the Extractor

```bash
docker run --rm \
  -v "${PWD}/sample_dataset/pdfs:/app/input" \
  -v "${PWD}/CodeOutput:/app/output" \
  mysolution:latest
```

### 3. Output

For every `filename.pdf` in `/app/input`, you'll get a `filename.json` in `/app/output`, perfectly matching the schema in `sample_dataset/schema/output_schema.json`.

##  Why Our Approach Wins

- **No Black-Box Models:** Pure, explainable logic—no hidden ML, no model bloat, no surprises.
- **Handles Real-World PDFs:** From simple reports to multi-column, multi-level academic papers, our extractor adapts and conquers.
- **Resource-Efficient:** Designed to run on modest hardware, with zero internet dependency and minimal RAM/CPU footprint.
- **Open Source, Open Standards:** 100% open libraries, fully auditable code.

## Validation Checklist

- [x] All PDFs processed automatically
- [x] Output JSONs match schema exactly
- [x] Sub-10s processing for 50-page PDFs
- [x] No internet access required
- [x] Works on AMD64 CPUs, 16GB RAM, 8 cores
- [x] Handles both simple and complex layouts

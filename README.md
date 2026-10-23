# Sparv parquet import plugin

This Sparv plugin is a an **importer utitily** that allows you to import text and annotations from parquet files into
the Sparv Pipeline. It reads the parquet file using [pyarrow](https://pypi.org/project/pyarrow/), extracts the specified
columns (all columns by default), and writes the text and annotations to the appropriate output files.

## Prerequisites

- [Sparv](https://spraakbanken.gu.se/sparv) v5.3 or newer
- [Python](https://www.python.org/downloads/) 3.10 or newer

## Installation

Install the latest stable version from GitHub:

```bash
sparv plugins install https://github.com/spraakbanken/sparv-sbx-parquet-import/archive/main.zip
```

## Usage

In the `import` section in your corpus configuration file (`config.yaml`), specify the `sbx_parquet_import:parse`
importer:

```yaml
import:
  importer: sbx_parquet_import:parse
```

Use the `sbx_parquet_import` section in your corpus configuration file to adjust any settings:

```yaml
sbx_parquet_import:
  prefix: ""  # Optional prefix to add to all annotation names produced by this importer.
  text_row: text  # Name of the row containing the text. Default value is "text".
  rows:  # Optional list of rows in source files that may be used for renaming.
    - row1 as row1_renamed
    - row2 as row2_renamed
  skip:  # Rows to skip during import.
    - row3
  batch_size: 10000  # Size of the batches to process at a time. Default value is 10000.
  keep_control_chars: false  # Set to true if control characters should not be removed from the text.
  normalize: NFC  # Normalize input using any of the following forms: 'NFC', 'NFKC', 'NFD', and 'NFKD'.
```

## Output

The output produced when using this plugin are files in the `sparv-workdir` directory which can be read by Sparv. The
following annotations are produced:

- `document`: The document annotation spanning all the texts in one parquet file.
- `text`: The texts from the specified row (`text_row`) in the parquet file.
- One annotation `text:<row_name>` for each of the other rows in the parquet file (unless skipped), where `<row_name>`
  is the name of the row in the parquet file.

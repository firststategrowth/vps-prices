# Dataset cards

Cards for copies of this dataset on other platforms, rebuilt from the files here on every update, so their counts,
dates and columns are always this repository's.

- `huggingface/README.md`: the Hugging Face dataset card. Its YAML names one subset per file and reads each CSV
  with `skiprows: 1` (the source line). A Hugging Face copy is this repository with this file as its README.md.
- `kaggle/dataset-metadata.json`: Kaggle's metadata (title, subtitle, CC-BY-4.0, a schema per file in column order).
  Kaggle reads a CSV's first line as its header, so a Kaggle copy holds the same CSVs without the source line, and
  the price history as one file, `price-history.csv`.

Neither copy exists yet.

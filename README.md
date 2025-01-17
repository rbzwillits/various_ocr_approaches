# various_ocr_approaches
A short documentation for the various automated open-source OCR work I have used mostly for my reference. 


# OCR for the Working Historian

This repository provides tools and instructions for applying Optical Character Recognition (OCR) to digitized historical documents. It includes scripts and commands to process PDFs and images, making them searchable and compatible with tools like qualitative data analysis software.

## Introduction

Historians often rely on digitized sources such as:
- PDF files with no recognized text (un-OCRed).
- Image files (e.g., `.jpg`, `.png`, `.HEIC`), often captured in archives.

Without OCR, these files are not searchable, text cannot be highlighted, and the orientation may be incorrect. While commercial tools like Adobe Acrobat can handle OCR, they require expensive licenses and often lack efficiency for large-scale processing. Open-source tools like **Tesseract**, **OCRmyPDF**, and **ImageMagick** provide powerful alternatives, enabling batch processing and customization through the command line.

---

## Prerequisites

Before using the tools in this repository, ensure the following dependencies are installed.

### 1. Install Homebrew (macOS)
Homebrew is a package manager for macOS that simplifies the installation of open-source software. Install it by running:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Install Tesseract
Tesseract is the main OCR engine used for processing image files. Install it via Homebrew:
```bash
brew install tesseract
```

### 3. Install OCRmyPDF
OCRmyPDF is used to apply OCR to PDF files. Install it via Homebrew:
```bash
brew install ocrmypdf
```

### 4. Install ImageMagick
ImageMagick is used for image format conversions (e.g., `.HEIC` to `.tiff`). Install it via Homebrew:
```bash
brew install imagemagick
```

### 5. Verify Installations
Ensure all tools are installed correctly by running:
```bash
tesseract --version
ocrmypdf --version
convert --version
```

---

## Usage Instructions

This section covers specific workflows for processing different types of files.

### Workflow 1: Applying OCR to PDFs

#### A. Batch Processing Multiple PDFs
This command applies OCR to all PDFs in a directory, creating searchable versions with `_OCR` appended to their filenames:
```bash
for file in *.pdf; do ocrmypdf "$file" "${file%.*}_OCR.pdf" --force-ocr; done
```

#### B. Processing a Single PDF
For a single PDF, use:
```bash
ocrmypdf example.pdf output.pdf --force-ocr
```

**Note:** The `--force-ocr` flag ensures OCR is applied even if the PDF already contains text.

---

### Workflow 2: Processing Images with Tesseract

#### A. Converting `.tiff` Images to Text Files
For extracting text from `.tiff` images:
```bash
for file in *.tiff; do
    tesseract "$file" "${file%.*}" -l eng --psm 6 -c tessedit_create_pdf=0 txt
done
```
- `-l eng`: Specifies the language (English in this case).
- `--psm 6`: Optimized for blocks of text.

#### B. Creating Searchable PDFs from Images
To convert an image (e.g., `.jpg`) to a searchable PDF:
```bash
tesseract file.jpg out -l eng pdf
```

#### C. Batch Processing Images into PDFs
For processing multiple `.tiff` images into individual searchable PDFs:
```bash
for file in *.tiff; do
    tesseract "$file" "${file%.tiff}_OCR" pdf
done
```

---

### Workflow 3: Converting Image Formats

#### A. Convert `.HEIC` to `.tiff`
Use ImageMagick to convert `.HEIC` files to `.tiff`:
```bash
mkdir fixed
for file in *.HEIC; do
    convert "$file" "fixed/${file%.HEIC}.tiff"
done
```

---

## Data Management Recommendations

### A. Photographing Archival Documents
1. Organize images into a directory named after the collection and date (e.g., `YYYY-MM-DD_CollectionName`).
2. Name files based on their archive details, e.g., `B1_F1_01_01.HEIC` for Box 1, Folder 1, Document 1, Page 1.
3. Check orientation and correct as needed.

### B. Managing PDFs of Books and Articles
1. Name files systematically (e.g., `author_lastname_title_year.pdf`).
2. Replace original PDFs with OCR-processed versions, appending `_OCR` to filenames.

### C. Newspapers and Journal Articles
1. Use consistent naming conventions (e.g., `date_headline_periodical.pdf` for newspapers).
2. Store files in Zotero or similar tools, replacing original files after OCR processing.

---

## Automation with Scripts

This repository includes a Bash script (`ocr_process.sh`) to streamline PDF OCR processing.

### How to Use the Script

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-repository>.git
   cd <your-repository>
   ```

2. Make the script executable:
   ```bash
   chmod +x ocr_process.sh
   ```

3. Configure input/output folder paths in the script:
   ```bash
   INPUT_FOLDER="/path/to/input/folder"
   OUTPUT_FOLDER="/path/to/output/folder"
   ```

4. Run the script:
   ```bash
   ./ocr_process.sh
   ```

Processed files will appear in the output folder with `_OCR` appended to their names.

---

## Troubleshooting

- **Command not found:** Ensure all tools are installed correctly and included in your PATH.
- **Permission denied:** Use `chmod +x` to make scripts executable.
- **Input folder not found:** Verify the folder paths in the script.

---

## License

This repository is open-source and distributed under the MIT License.

---

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your enhancements or bug fixes.


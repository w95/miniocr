# MiniOCR

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A powerful and easy-to-use Python package for performing Optical Character Recognition (OCR) on images, PDF documents, and PowerPoint presentations using OpenAI's Vision API.

## Features

- 🖼️ **Multi-format support**: Process images (PNG, JPG, JPEG, GIF, BMP, TIFF, WebP), PDF files, and PPTX presentations
- ⚡ **Parallel processing**: Concurrent processing of multiple pages/slides for improved performance
- 🌐 **Cross-platform**: Works on Windows, macOS, and Linux
- 📄 **Visual OCR for PPTX**: Converts PowerPoint slides to images for accurate visual content extraction
- 🔄 **Async support**: Built with asyncio for efficient processing
- 📝 **Markdown output**: Converts documents to clean, structured markdown format

## Installation

### Prerequisites

For PDF processing, you'll need to install Poppler:

**macOS:**
```bash
brew install poppler
```

**Ubuntu/Debian:**
```bash
sudo apt-get install poppler-utils
```

**Windows:**
Download and install from [Poppler for Windows](http://blog.alivate.com.au/poppler-windows/)

For PowerPoint (.pptx) processing, you'll need to install LibreOffice:

**macOS:**
```bash
brew install --cask libreoffice
```

**Ubuntu/Debian:**
```bash
sudo apt-get install libreoffice
```

**Windows:**
Download and install from [LibreOffice official website](https://www.libreoffice.org/download/download/)

### Install MiniOCR

```bash
pip install miniocr
```

Or install from source:

```bash
git clone https://github.com/w95/miniocr.git
cd miniocr
pip install -e .
```

## Quick Start

### Setup

First, you'll need an OpenAI API key. Set it as an environment variable:

```bash
export OPENAI_API_KEY="your-api-key-here"
```

Or pass it directly when initializing the class.

### Basic Usage

```python
import asyncio
from miniocr import MiniOCR

async def main():
    # Initialize with API key (or use environment variable)
    ocr = MiniOCR(api_key="your-api-key-here")
    
    # Process an image
    result = await ocr.ocr("path/to/image.jpg")
    print(result["content"])
    
    # Process a PDF
    result = await ocr.ocr("path/to/document.pdf")
    print(f"Processed {result['pages']} pages")
    print(result["content"])
    
    # Process a PowerPoint presentation
    result = await ocr.ocr("path/to/presentation.pptx")
    print(result["content"])

if __name__ == "__main__":
    asyncio.run(main())
```

### Advanced Usage

```python
import asyncio
from miniocr import MiniOCR

async def advanced_example():
    ocr = MiniOCR()
    
    # Process with custom settings
    result = await ocr.ocr(
        file_path="document.pdf",
        model="gpt-4o",  # Use different OpenAI model
        concurrency=10,  # Process up to 10 pages simultaneously
        output_dir="./output",  # Save markdown to file
        cleanup=True  # Clean up temporary files
    )
    
    print(f"File: {result['file_name']}")
    print(f"Pages processed: {result['pages']}")
    print(f"Content length: {len(result['content'])} characters")

asyncio.run(advanced_example())
```

### Processing URLs

```python
import asyncio
from miniocr import MiniOCR

async def process_url():
    ocr = MiniOCR()
    
    # Process a file from URL
    result = await ocr.ocr("https://example.com/document.pdf")
    print(result["content"])

asyncio.run(process_url())
```

## API Reference

### MiniOCR Class

#### `__init__(api_key: str = None)`

Initialize the MiniOCR instance.

**Parameters:**
- `api_key` (str, optional): OpenAI API key. If not provided, will use `OPENAI_API_KEY` environment variable.

#### `async ocr(file_path, model="gpt-4o-mini", concurrency=5, output_dir=None, cleanup=True)`

Process a file and extract text using OCR.

**Parameters:**
- `file_path` (str): Path or URL to the file to process
- `model` (str): OpenAI model to use (default: "gpt-4o-mini")
- `concurrency` (int): Number of concurrent API requests (default: 5)
- `output_dir` (str, optional): Directory to save markdown output
- `cleanup` (bool): Whether to clean up temporary files (default: True)

**Returns:**
- `dict`: Dictionary containing:
  - `content` (str): Extracted text in markdown format
  - `pages` (int): Number of pages/slides processed
  - `file_name` (str): Name of the processed file

**Supported file types:**
- Images: `.png`, `.jpg`, `.jpeg`, `.gif`, `.bmp`, `.tiff`, `.webp`
- Documents: `.pdf`
- Presentations: `.pptx`

## Configuration

### Environment Variables

- `OPENAI_API_KEY`: Your OpenAI API key (required)

### Model Options

MiniOCR supports various OpenAI models:
- `gpt-4o-mini` (default, cost-effective)
- `gpt-4o` (higher accuracy)
- `gpt-4-turbo`

## Output Format

MiniOCR converts documents to clean markdown with the following features:

- **Tables**: Converted to HTML format for better structure
- **Checkboxes**: Represented as ☐ (unchecked) and ☑ (checked)
- **Special elements**: Logos, watermarks, and page numbers are wrapped in brackets
- **Charts and infographics**: Interpreted and converted to markdown tables when applicable

## Error Handling

```python
import asyncio
from miniocr import MiniOCR

async def handle_errors():
    ocr = MiniOCR()
    
    try:
        result = await ocr.ocr("nonexistent.pdf")
    except ValueError as e:
        print(f"Unsupported file type: {e}")
    except Exception as e:
        print(f"Processing error: {e}")

asyncio.run(handle_errors())
```

## Performance Tips

1. **Adjust concurrency**: Increase `concurrency` parameter for faster processing of multi-page documents
2. **Use appropriate models**: `gpt-4o-mini` for cost-effectiveness, `gpt-4o` for higher accuracy
3. **Process in batches**: For large numbers of files, process them in batches to avoid rate limits
4. **Local processing**: Keep files local when possible to avoid download overhead

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Testing

Run the test suite:

```bash
pytest tests/
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

### v0.0.1
- Initial release
- Support for images, PDF, and PPTX files
- Async processing with concurrency control
- Cross-platform compatibility

## Support

If you encounter any issues or have questions, please [open an issue](https://github.com/w95/miniocr/issues) on GitHub.

## Acknowledgments

- Built with [OpenAI's Vision API](https://platform.openai.com/docs/guides/vision)
- Uses [pdf2image](https://github.com/Belval/pdf2image) for PDF processing
- Uses [python-pptx](https://github.com/scanny/python-pptx) for PowerPoint processing
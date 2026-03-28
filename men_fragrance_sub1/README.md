# Boutiqaat Men's Fragrance Scraper Pipeline

Automated web scraping pipeline for Boutiqaat men's fragrance products with S3 storage and Excel report generation.

## Features

✅ **Automated Web Scraping**
- Scrapes men's fragrance subcategories from boutiqaat.com
- Extracts detailed product information (name, price, brand, description, ratings, etc.)
- Handles infinite scroll pagination automatically

✅ **Image Management**
- Downloads product images from the website
- Uploads to AWS S3 with organized folder structure
- Generates S3 paths for reference in Excel files

✅ **Excel Report Generation**
- Creates one Excel file per category
- Separate worksheet for each subcategory
- Includes summary statistics
- Professional formatting with colors and borders
- S3 image path column for easy reference

✅ **S3 Storage with Date Partitioning**
- Organized folder structure: `bucket/boutiqaat-data/year=YYYY/month=MM/day=DD/men/fragrance/`
- Separate folders for images and Excel files
- Easy to query and organize data by date

## Scraped Categories

This pipeline scrapes the following men's fragrance subcategories:
- Perfumes
- Hair Mist
- Perfume Gift Sets
- Refillable Fragrance Spray
- Body Mist

## Project Structure

```
men_fragrance_sub1/
├── __init__.py                   # Package initializer
├── scraper.py                    # Web scraper module
├── s3_uploader.py               # S3 upload functions
├── excel_generator.py           # Excel file generation
└── main.py                      # Main orchestrator
```

## Setup Instructions

### 1. Prerequisites

- Python 3.8+
- AWS Account with S3 access
- Playwright (for JavaScript rendering)

### 2. Installation

Activate virtual environment:
```bash
# From project root
cd Project8-Baat
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

Install Playwright browsers (if not already installed):
```bash
playwright install chromium
```

### 3. Configuration

AWS credentials should be set in the main `config.py` file at project root.

### 4. Running the Scraper

From the project root directory:
```bash
python -m men_fragrance_sub1.main
```

## Output

### Excel Files
- Location: S3 → `boutiqaat-data/year=YYYY/month=MM/day=DD/men/fragrance/excel-files/`
- Format: `{category_name}_{timestamp}.xlsx`
- Contents:
  - Summary sheet with statistics
  - Individual sheets per subcategory
  - Product details with S3 image paths

### Images
- Location: S3 → `boutiqaat-data/year=YYYY/month=MM/day=DD/men/fragrance/images/{category}/`
- Format: `{sku}_image.jpg`

## Data Schema

Each product contains:
- Product Name
- Brand
- Price (current and old)
- Discount percentage
- SKU
- Description
- Rating
- Number of Reviews
- Available Colors
- Product URL
- Image URL
- S3 Image Path

## Async Processing

The pipeline processes up to 3 subcategories concurrently using asyncio with a semaphore to prevent overwhelming the server.

## Error Handling

- Retries on failed requests
- Graceful degradation if images fail to download
- Comprehensive logging for debugging
- Temporary files cleanup on completion

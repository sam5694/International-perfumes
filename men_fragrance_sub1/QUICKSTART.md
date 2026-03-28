# Quick Start Guide - Men's Fragrance Scraper

## Run the Scraper (1-Minute)

### 1. **Activate Virtual Environment**
```bash
# From project root
cd Project8-Baat
.\venv\Scripts\Activate.ps1  # PowerShell
# OR
venv\Scripts\activate  # CMD
```

### 2. **Run the Pipeline**
```bash
python -m men_fragrance_sub1.main
```

### 3. **Monitor Progress**
Watch the console output for:
- ✅ S3 connection test
- ✅ Product scraping progress
- ✅ Image upload status
- ✅ Excel file generation
- ✅ Final upload summary

## What Will Happen

The pipeline will:
1. ✅ Connect to S3 and verify credentials
2. ✅ Scrape 5 men's fragrance subcategories (max 3 concurrent)
   - Perfumes
   - Hair Mist
   - Perfume Gift Sets
   - Refillable Fragrance Spray
   - Body Mist
3. ✅ Extract all products with infinite scroll
4. ✅ Get detailed info for each product
5. ✅ Download and upload images to S3: `men/fragrance/images/`
6. ✅ Generate Excel reports
7. ✅ Upload Excel files to S3: `men/fragrance/excel-files/`

## Expected Output

```
================================================================================
Starting Men's Fragrance Pipeline (Async – Semaphore=3)
================================================================================
Processing 5 subcategories (max 3 concurrent)
[Slot acquired] Starting: perfumes
[Slot acquired] Starting: hair-mist-1
[Slot acquired] Starting: perfume-gift-sets
...
================================================================================
Pipeline Complete: 5 successful, 0 failed
================================================================================
```

## S3 Output Location

### Images
```
s3://your-bucket/boutiqaat-data/year=2026/month=03/day=28/men/fragrance/images/
```

### Excel Files
```
s3://your-bucket/boutiqaat-data/year=2026/month=03/day=28/men/fragrance/excel-files/
```

## Verify Installation

```bash
# Test imports
python -c "from men_fragrance_sub1.scraper import BoutiqaatScraper; print('✓ Scraper OK')"
python -c "from men_fragrance_sub1.s3_uploader import S3Uploader; print('✓ S3 Uploader OK')"
python -c "from men_fragrance_sub1.excel_generator import ExcelGenerator; print('✓ Excel Generator OK')"
```

## Troubleshooting

### Issue: S3 Connection Failed
**Solution:** Check AWS credentials in `config.py`

### Issue: Playwright Not Found
**Solution:** Install Playwright browsers
```bash
playwright install chromium
```

### Issue: No Products Found
**Solution:** The website might be blocking. Try:
- Running again (temporary network issue)
- Checking if URLs are still valid
- Verifying Playwright is working

### Issue: Slow Performance
**Normal:** Infinite scroll takes time (6-9 seconds per scroll)
**Expected:** 5-10 minutes for all subcategories

## Tips

- **First run takes longer** due to product detail fetching
- **Progress logged** for each product
- **Temp files** cleaned up automatically
- **Safe to interrupt** with Ctrl+C

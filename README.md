## README.md — Methodology and Reflection

### Project Overview
This project analyzes the global aesthetics and urban geography of third-wave coffee shops using a combination of scraped metadata, AI-generated image captions, and census-based regression modeling. The goal was to move beyond simple charting to build a complex data pipeline from raw scraping to statistical inference and interactive storytelling.

### Data Sources
- **Primary source**: [ThirdWaveNearMe.com](https://thirdwavenearme.com), a curated global map of specialty cafés.
- **Instagram content**: Screenshots from verified café pages (2,273 images), OCR-extracted bios and captions.
- **Census data**: American Community Survey (ACS 5-Year Estimates), tract-level data for New York City, Dallas, and Miami.
- **Geospatial data**: U.S. Census TIGER/Line shapefiles + KML exports from Google My Maps.

### Process Summary
- Scraped 173 city pages using Playwright; extracted Google My Maps embed links.
- Manually downloaded 164 KML files after failed attempts to fully automate through Playwright due to Google Maps UI inconsistencies.
- Parsed KML into café point coordinates (5,204 total)
- Automated and refined the script manually for the scraping process of using Playwright to search for and save Instagram, website, Facebook and Tiktok links to csv files.
- Automated and refined the script manually to scrape instamgram bios and website content.
- Automated and refined script to take screenshots of 5,000+ Instagram accounts.
- Automated and refined script to take screenshots of more than 200+ websites which each top, middle and bottom parts of coffee shops.
- OCR-ed Instagram screenshots and matched noisy text to café metadata using fuzzy matching (RapidFuzz).
- Scraped more than 200+ websites using Playwright extract.
- Tried ChatGPT-vision model to detect objects but failed.
- Tried ChatGPT-vision to cluster and define themes of texts but failed. 
- Used BLIP (Salesforce's image captioning model), OWL-Vit to generate descriptive captions and detect objects in 6,000+ screenshots, this was more than 15 hours of laptop burning up. 
- Ran unsupervised topic modeling (FASTopic) on 5,000+ café bios to extract themes like minimal design, dog-friendly, and vegan options.
- Joined café coordinates back to csv with themes extracted files to map. 
- Joined café coordinates to census tracts in NYC, Dallas, and Miami; ran logistic regression models on tract-level variables to estimate the probability of café presence.

### Technical Highlights
- **Scraping**: Playwright, BeautifulSoup; scraped JS-heavy websites and manually reviewed tab automation flows.
- **AI captioning**: BLIP model used locally for ~4 hours (CPU-bound); rejected >5% of generic outputs.
- **Manual supervision** Less than 5% of incorrect Instagram links and websites after manually reviewed thousands of links and ran fact-check with Perplexity and ChatGPT.
- **Topic modeling**: FASTopic clustered bios into ~30 themes; top words per cluster manually validated.
- **Regression**: ACS data joined to café points by census tract; modeled effects of log income, rent, home value, Gini index, and education levels.
- **Visualization**: Created horizontal bar charts (matplotlib) and probability maps using geopandas.

### Regression Coefficients Summary
| Variable              | NYC     | Dallas | Miami  |
|-----------------------|---------|--------|--------|
| log_income            | -0.3649 | -1.2013| -1.9067|
| pct_bachelor_plus     | 0.0008  | 0.0003 | 0.0004 |
| log_rent              | 3.4188  | 1.7494 | 1.7683 |
| log_home_value        | 1.1426  | 1.6046 | 2.0776 |
| gini_index            | 8.0569  | 5.4155 | 1.5188 |

🗺️ These models helped produce city-level maps predicting future café locations based on underlying socioeconomic traits.

### Limitations
- Google My Maps KML export is not publicly accessible via API, requiring semi-manual tab interaction.
- OCR quality varied by font style and screenshot clarity.
- BLIP sometimes failed on image contrast or dark palettes.
- ACS-based regression only applies where census data and café density overlap.
- Student life and no money to support the project led to no chatGPT-vision model success since $90 was too expensive, so very sad. 

### Reflections
This was the most technically ambitious project I’ve built to date. It taught me:
- How to manage fuzzy, multi-source datasets.
- How to chain scraping, AI, geospatial joins, and modeling together.
- The limits of AI-generated captions and the value of manual validation.
- How to convert regression outputs into geographic storytelling.

I would love to expand this work to cluster cafés globally by aesthetic similarity (using computer vision embedding models) or examine how neighborhood change precedes café appearance over time.

### Files
- `scrape_thirdwaveneame.ipynb`: Scraping map data.
- `analyze_and_map.ipynb`: KML verification, café geojoin, and city visuals.
- `usingBLIP.ipynb`: Image captioning + topic modeling.
- `clean_and_merge.ipynb`: OCR matching, fuzzy logic, and dataset consolidation.
- `coffee_logit_coeff_horizontal.svg`: Regression coefficient chart.
- `README.md` (this file): Project narrative + reproducibility notes.

### Data Volume Summary
- 173 cities scraped
- 5,204 cafés geolocated
- 2,273 Instagram screenshots processed
- 2,500+ bios modeled
- 50,000+ data points cleaned, matched, and visualized


